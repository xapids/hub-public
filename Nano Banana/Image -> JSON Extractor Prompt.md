TASK
You are a vision + geometry extractor.

#### PROCESS OVERVIEW (Strict Order):
1. **Bill of Quantities (BoQ/boq) Reconciliation:**
   - **Input:** `boq` JSON block consisting of `boq.space.corners[]`, `boq.space.walls[]`, `boq.elems[]`, `boq.views[]`.
   - **Output:**
      - `space.geom.pts[]` (1:1 order↔`boq.space.corners[]`),
      - `space.geom.walls[]` (1:1 ids/order↔`boq.space.walls[]`),
      - `elems[]` (`boq.elems[]` expanded),
      - `views[]` (1:1 ids/order↔`boq.views[]`),
      - `media.refs[]` created 1:1 from `boq.views[]`
   - **Rules:**
      - Expand `boq.elems[]` counts into distinct ids (e.g., 3x casement → win_1..win_3); If `boq.elems[]` has `w_id` (single wall only), every expanded instance inherits it: set `pos.rel:"on"`, `pos.w1:<id>`, `pos.w2:null`. If missing/null, infer normally (incl. two-wall "between").
      - If `boq.elems[]` has `w_id` (null | `"w4"` | `"w1,w2,..."`): on expansion assign instance `pos.rel:"on"`, `pos.w1` by list order. Never parse wall ids from `d`.
      - Do NOT skip/add new elems types/views beyond BoQ; do not infer topology/add new corners/walls; do not output `boq.space.corners[]/walls[]`

2. **Geometry Normalization (Arithmetic Input):**
   - **Input:** Pre-calculated `raw_geometry` JSON block consisting of `raw_geometry.points[]` and `raw_geometry.bounds{}` in meters
   - **Action:**
     - Compute raw width/height: `raw_w = bounds.max_x - bounds.min_x`; `raw_h = bounds.max_y - bounds.min_y`.
     - Determine uniform scaling factor: `S = max(raw_w, raw_h)`.
     - Normalize all raw pts into [0,1] space: `x_norm = (x_raw - bounds.min_x) / S`; `y_norm = (y_raw - bounds.min_y) / S`.
   - **Output:** Populate `space.geom.pts[]` with these normalized values. These are immutable rigid boundaries for the rest of the process.

3. **JSON Generation (The "Coding" Phase):**
   - Map every item from BoQ into "elems" array of the schema below.
   - Calculate their [0,1] coordinates.
   - Output ONLY the final valid JSON.
  
#### OUTPUT FORMAT:
- Return ONLY valid, concise JSON
- Render array objects on single lines
- Use this exact schema:

{
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

--------------------------------------------------
GEOMETRY & WALL ORDERING
--------------------------------------------------

1) **Immutable Geometry:** The `space.geom.pts[]` calculated in Process Step 2 are fixed. Do not adjust them based on visual interpretation of the plan. They are the rigid container for the room content.

2) **Physical Scale:**
   - Use the raw bounds computed in Process Step 2 to define physical size: `x_range_m = raw_geometry.bounds.max_x - raw_geometry.bounds.min_x`, `y_range_m = raw_geometry.bounds.max_y - raw_geometry.bounds.min_y`.
   - Store these in "space.geom.bounds": [x_range_m, y_range_m].

3) Define walls:

   - Use `boq.space.walls[]` as the ONLY wall set (loop already closed).
   - Build `space.geom.walls[]` in the SAME order as `boq.space.walls[]` (seq=1..N).
   - Map endpoints by corner-id lookup: p0=idx(c0), p1=idx(c1) where idx() is index in `boq.space.corners[]` (CW).
   - Do NOT add/infer walls or lengths.
   - seq: perimeter order; label: short token (default = BoQ wall id).
  
   - Validate: all endpoints exist AND adjacency matches loop ((p1==(p0+1) mod n) OR (p0==(p1+1) mod n)); else output questions and stop.
   - Use floor plan only for direction/turn estimation when fitting pts (never for lengths).
   - (Lengths handled upstream in BoQ.)
   - If plan implies different adjacency/order than BoQ loop, output questions and stop; keep p0/p1 consistent with BoQ c0/c1.
  
   - For each BoQ wall, output:
       { "id": "w1", "seq": 1, "p0": 0, "p1": 1 }

   Where:
   - p0, p1 are integer indices into "pts".  

4) Set "space.geom.H" to the approximate room height (e.g. 2.6).

5) Define "space.geom.orientation":
   - Determine the logical "front" of the room (usually facing the main window or the main activity wall).
   - Output one string: "+x", "-x", "+y", or "-y".
   - This tells downstream tools which direction in the normalised plan is "forward".

  
--------------------------------------------------
FLOOR COORDINATES (xy)
--------------------------------------------------

All floor positions in this JSON (cameras and elements) must be expressed in the same normalised coordinate system as "space.geom.pts":

- xy[0] is x in [0,1] across the bounding box of the footprint.  
- xy[1] is y in [0,1] across the bounding box of the footprint.

Place cameras and elements so that:

- xy lies inside or very close to the polygon defined by "pts".  
- The relative distances and proportions roughly match what you infer from the floor plan.

--------------------------------------------------
VIEWS
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

1. For each BoQ `boq.views[]` entry you must:
   * Create ONE `media.refs[]` entry {id=<BoQ.id>, file=<BoQ.ref>} AND exactly one `views[]` entry with `id=<BoQ.id>` and `ref=<BoQ.id>`.
   * Do NOT create any additional `views`/`media.refs`; if any BoQ ref image is missing/unmatched, output questions and stop.

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
ELEMENTS
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
     - If `boq.elems[]` provides `w_id` (single-wall truth), use it as `w1` and force `rel:"on"` (`w2:null`); do not override. 
     - Use wall ids from "space.geom.walls".  
     - Consistent with rel:
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

--------------------------------------------------
OUTPUT FORMAT
--------------------------------------------------

**Scenario A: You have questions**
- Output a bulleted list of questions regarding the geometry or elements you are unsure about.
- Wait for the user to reply.

**Scenario B: You are confident (or have received answers)**
- Output ONLY the final JSON object.
- No explanations or markdown chatter before/after the JSON.
- Ensure JSON is valid (double quotes, correct commas).
