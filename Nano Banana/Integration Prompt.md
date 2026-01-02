### PROCESS OVERVIEW (Strict Order):
#### 1. Perception Prompt Reconciliation:
   - **Input:** `perception` JSON block consisting of `perception.space.corners[]`, `perception.space.walls[]`, `perception.elems[]`, `perception.views[]`.
   - **Action:**
      - Expand `perception.elems[]` counts into distinct ids (e.g., 3x casement → win_1..win_3)
      - Do NOT skip/add new elems types/views beyond Perception; do not infer topology/add new corners/walls; do not output `perception.space.corners[]/walls[]`
   - **Output:**
      - `space.geom.pts[]` (1:1 order↔`perception.space.corners[]`),
      - `space.geom.walls[]` (1:1 ids/order↔`perception.space.walls[]`),
      - `elems[]` (`perception.elems[]` expanded),
      - `views[]` (1:1 ids/order↔`perception.views[]`),
      - `media.refs[]` created 1:1 from `perception.views[]`

#### 2. Arithmetic Prompt Reconciliation (Geometry Normalization):
   - **Input:** Pre-calculated `raw_geometry` JSON block consisting of `raw_geometry.pts[]` and `raw_geometry.bounds{}` in meters
   - **Action:**
     - Compute raw width/height: `raw_w = bounds.max_x - bounds.min_x`; `raw_h = bounds.max_y - bounds.min_y`.
     - Determine uniform scaling factor: `S = max(raw_w, raw_h)`.
     - Normalize all raw pts into [0,1] space: `x_norm = (x_raw - bounds.min_x) / S`; `y_norm = (y_raw - bounds.min_y) / S`.
   - **Output:** Populate `space.geom.pts[]` with these normalized values. These are immutable rigid boundaries for the rest of the process.

#### 3. JSON Generation (The "Coding" Phase):
   - Map every item from Perception into "elems" array of the schema below.
   - Calculate their [0,1] coordinates.
   - Output the final valid JSON.
  
### OUTPUT FORMAT:
**Scenario A: You have questions**
- Output a bulleted list of clarification questions.
- Do NOT output JSON.
- Stop.

**Scenario B: You are confident (or have received answers)**
- Output ONLY concise JSON object.
- No explanations or markdown
- Use this exact schema:

{
"integration": {
  "task": {
    "intent": "Architecturally_prepare_room_for_future_design_renders",
    "scope": "render_ALL_views",
    "goal": "architectural_preparation",
    "description": "remove ALL elements with 'rm': true; replace according to 'rm'; render IDENTICAL room"
  },
  "legend": {
    "cat": {
      "arch": "architecture surfaces (floor, wall, ceiling)",
      "open": "openings (window, door, arch)",
      "fix": "fixed elements (ceiling fan, AC, built-in light, radiator)",
      "furn": "furniture (tables, chairs, benches, sofa, shelves)",
      "appl": "appliances",
      "dec": "decor (art, posters, decorative lamps, plants)",
      "grp": "group of small items / clutter"
    },
    "pos_rel": {
      "on": "on one wall",
      "between": "between two walls or at a corner",
      "floor": "free-standing on floor",
      "ceil": "attached mainly to ceiling"
    },
    "vis": {
      "f": "fully visible",
      "p": "partially visible",
      "o": "mostly occluded"
    }
  },
  "proj": {
    "name": string
  },
  "media": {
    "floor": string,
    "refs": [
  { "id": string, "file": string },
  { "id": string, "file": string }
    ]
  },
"space": {
    "geom": {
      "bounds": [number, number],
      "pts": [
        [number, number],
        [number, number],
        [number, number]
      ],
      "H": number,
      "orientation": "+x" | "-x" | "+y" | "-y",
      "walls": [
        { "id": string, "seq": int, "p0": int, "p1": int },
        { "id": string, "seq": int, "p0": int, "p1": int }
      ]
    }
  },
  "views": [
    {
      "id": string,
      "ref": string | null,
      "cam": { "rel": "corner" | "wall" | "free", "w1": string | null, "w2": string | null, "xy": [number, number], "h": number }
    }
  ],
  "elems": [
    {
      "id": string,
      "cat": "arch" | "open" | "fix" | "furn" | "appl" | "dec" | "grp",
      "type": string,
      "pos": { "rel": "on" | "between" | "floor" | "ceil", "w1": string | null, "w2": string | null, "xy": [number, number], "h": number | null, "sz": [number | null, number | null, number | null] },
      "views": [
        { "v": string, "bb": [number, number, number, number], "vis": "f" | "p" | "o" },
        { "v": string, "bb": [number, number, number, number], "vis": "f" | "p" | "o" }
      ],
      "d": string,
      "rm": boolean,
      "repl": string | null
    }
  ]
 }
}

--------------------------------------------------
### GEOMETRY & WALL ORDERING
--------------------------------------------------

1) **Immutable Geometry:** The `space.geom.pts[]` calculated in Process Step 2 are fixed. Do not adjust them based on visual interpretation of the plan. They are the rigid container for the room content.

2) **Physical Scale:**
   - Use the raw bounds computed in Process Step 2 to define physical size: `x_range_m = raw_geometry.bounds.max_x - raw_geometry.bounds.min_x`, `y_range_m = raw_geometry.bounds.max_y - raw_geometry.bounds.min_y`.
   - Store these in "space.geom.bounds": [x_range_m, y_range_m].

3) Define walls:

   - Use `perception.space.walls[]` as the ONLY wall set (loop already closed).
   - Build `space.geom.walls[]` in the SAME order as `perception.space.walls[]` (seq=1..N).
   - Map endpoints by corner-id lookup: p0=idx(c0), p1=idx(c1) where idx() is index in `perception.space.corners[]` (CW).
   - Do NOT add/infer walls or lengths.
   - seq: perimeter order; label: short token (default = Perception wall id).
  
   - Validate: all endpoints exist AND adjacency matches loop ((p1==(p0+1) mod n) OR (p0==(p1+1) mod n)); else output questions and stop.
   - Use floor plan only for direction/turn estimation when fitting pts (never for lengths).
   - (Lengths handled upstream in Perception.)
   - If plan implies different adjacency/order than Perception loop, output questions and stop; keep p0/p1 consistent with Perception c0/c1.
  
   - For each Perception wall, output:
       { "id": "w1", "seq": 1, "p0": 0, "p1": 1 }

   Where:
   - p0, p1 are integer indices into "pts".  

4) Set "space.geom.H" to the approximate room height (e.g. 2.6).

5) Define "space.geom.orientation":
   - Determine the logical "front" of the room (usually facing the main window or the main activity wall).
   - Output one string: "+x", "-x", "+y", or "-y".
   - This tells downstream tools which direction in the normalised plan is "forward".

  
--------------------------------------------------
### FLOOR COORDINATES (xy)
--------------------------------------------------

All floor positions in this JSON (cameras and elements) must be expressed in the same normalised coordinate system as "space.geom.pts":

- xy[0] is x in [0,1] across the bounding box of the footprint.  
- xy[1] is y in [0,1] across the bounding box of the footprint.

Place cameras and elements so that:

- xy lies inside or very close to the polygon defined by "pts".  
- The relative distances and proportions roughly match what you infer from the floor plan.

--------------------------------------------------
### VIEWS
--------------------------------------------------

The "views" array defines the cameras for this room. Each entry is a camera definition:

* "id": string – unique view id, e.g. "v_ref_1", "v_kit1_front".
- "ref": string | null – id of the source reference image (for views tied to an input photo), or null for synthetic views (e.g. orbit cameras).
* "cam":

  * "rel": "corner" | "wall" | "free" – how the camera position is anchored.
  * "w1", "w2": wall ids or null (for "free" cameras).
  * "xy": [x, y] in [0,1]×[0,1] – camera position in the floor-plan coordinate system.
  * "h": number – camera height in metres above floor.

For each reference image you must:

1. For each Perception `perception.views[]` entry you must:
   * Create ONE `media.refs[]` entry {id=<Perception.id>, file=<Perception.ref>} AND exactly one `views[]` entry with `id=<Perception.id>` and `ref=<Perception.id>`.
   * Do NOT create any additional `views`/`media.refs`; if any Perception ref image is missing/unmatched, output questions and stop.

2. Set cam.rel:

   * "corner" if the camera is clearly near the intersection of two walls.
   * "wall"   if the camera is clearly along one main wall.
   * "free"   otherwise (somewhere inside the room).

3. Set cam.w1 and cam.w2:

   * cam.w1: id of the main wall the camera is associated with, or null.
   * cam.w2: second wall for a corner camera (the other wall meeting at the corner), or null.

4. Set cam.xy:

   * Approximate floor position of the camera centre in the normalised footprint coordinates.
   * Use the floor plan plus your understanding of the views.

5. Set cam.h:

   * Camera height above floor (m).
   * Usually about 1.4–1.7 for eye-level interior photos; default ~1.6 if uncertain.

--------------------------------------------------
### ELEMENTS
--------------------------------------------------

For each element in the Bill of Quantities, create an "elems" entry.

1) id:

   - Stable machine id, e.g. "win_1", "door_1", "tbl_1", "appl_wash_1".

2) cat (category):

   - "arch" = structural surfaces (floor, ceiling, structural wall, beam)  
   - "open" = window, door, arch opening, niche, pass-through  
   - "fix"  = fixed non-structural element (ceiling fan, AC, radiator, built-in light)  
   - "furn" = furniture (tables, chairs, benches, sofa, loose shelves)  
   - "appl" = appliances  
   - "dec"  = decor (artwork, posters, decorative lamps, plants)  
   - "grp"  = grouped small items / clutter treated as one element

3) type:

   - More specific subtype within cat:
     - Example: "window", "door", "base_cabinets", "dining_table", "sofa", "washing_machine", "wall_art", "small_appliance_group".

4) pos (position):

   - rel:
     - "on"      = attached to one wall (sconce, radiator, artwork).  
     - "between" = at/between two walls or in a corner.  
     - "floor"   = free-standing on the floor (table, freestanding shelf).  
     - "ceil"    = mainly attached to the ceiling (ceiling fan, pendant).

   - w1, w2:
     - Treat `perception.elems[].w_id` as a human-checked anchor: if non-null, it is authoritative; do not override.
     - If `w_id` is a single wall id: force `pos.rel:"on"`, set `pos.w1:<id>`, `pos.w2:null`.
     - If `w_id` is a comma-list: it MUST be consumed during expansion (1 wall id per expanded instance, by list order); post-expansion each instance `w_id` MUST be a single id; force `pos.rel:"on"`, `pos.w2:null`.
     - If `w_id` is null: infer `pos.rel`/`xy`; set `pos.w1/pos.w2` ONLY when strongly supported (e.g., xy is within ε of two wall segments for "between"/corner), otherwise leave null.
     - Do NOT parse wall ids from `d`.
     - Validate wall ids against "space.geom.walls".  
     - Consistent with rel:
       - "on": w1 is the wall it is on; w2 = null.  
       - "between": w1 and w2 are the two walls it is between.  
       - "floor"/"ceil": w1/w2 may be null or indicate the nearest wall(s).

   - w1, w2:
     1. Do NOT parse wall ids from `d`.
     2. Normalise `w_id`: If `w_id` is a comma-list, it MUST be consumed during expansion (1 wall id per expanded instance by list order); post-expansion each instance `w_id` MUST be single wall id or null.
     3. Validate: If `w_id` is non-null, it MUST match an id in `space.geom.walls[].id`; otherwise output questions and stop.
     4. If `w_id` non-null: Treat as human-checked anchor; force `pos.rel:"on"`, set `pos.w1:<id>`, `pos.w2:null`
     5. If `w_id` null: Infer `pos.rel`/`xy`; set `pos.w1/pos.w2` ONLY when strongly supported (e.g., xy lies very near two wall segments meeting at a corner for "between"), otherwise leave null.
     6. Consistency: Consistent with rel:
       - "on": w1 is the wall it is on; w2 = null.  
       - "between": w1 and w2 are the two walls it is between.  
       - "floor"/"ceil": w1/w2 may be null or indicate the nearest wall(s).

   - xy:
     - Element floor position in the same normalised footprint coordinates as "pts".  
     - For on-wall elements, xy should lie on or close to the associated wall segment.  
     - For floor elements, xy is the plan position of the element’s centre.

   - h:
     - Vertical position of the element's **anchor point** (metres from floor).
     - **Floor items:** h = 0 (Anchor is bottom).
     - **Wall items:** h = height of the **centre** of the object (e.g., for a sconce or painting).
     - **Ceiling items:** h = approximate height of the fixture's attachment point (usually close to Room H).

   - sz:
     - Approximate [width, height, depth] in metres, if you can estimate.  
     - If you cannot estimate, use [null, null, null].

5) views (appearances in reference images):

   If you can locate the element in a reference image:

   - Add an entry:
     { "v": "v1", "bb": [xmin, xmax, ymin, ymax], "vis": "f" | "p" | "o" }

   Where:
   - v: view id ("v1", "v2", …).  
   - bb: [xmin, xmax, ymin, ymax] in normalised frame coordinates of that image:
     - x, y in [0,1], with x from left→right, y from bottom→top.  
   - vis:
     - "f" = fully visible  
     - "p" = partially visible / cropped by frame  
     - "o" = mostly occluded

6) d (description):

   - Short compressed English tokens, not long sentences.  
   - Use underscores between tokens:
     - "tile_terracotta_30x30_matt_black_grout"  
     - "wall_plaster_warm_cream_matt"  
     - "window_casement_black_aluminium_clear_glass"  
     - "table_rect_black_metal_top"

7) rm and repl (clear-out semantics):

   - rm:
     - true  = element should be removed in a clear-out / redesign render.  
     - false = element should remain.

   - repl:
     - If rm=true: short instruction for what to show instead, e.g.:
       - "plain_wall"  
       - "extend_floor_and_walls"  
       - "empty_floor"  
     - If rm=false: null.
