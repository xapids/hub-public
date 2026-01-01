# README

This folder defines the JSON contracts that Nano Banana uses to work with a **single room at a time**. The JSON is an LLM-facing job description: Nano Banana reads it, and the render engine uses it to create images.

It is intentionally **per-room**, not a whole-house schema.

<br><br>

## Files
   * **README** - Architectural Source of Truth (Workflow Logic/Contract)
   * **Perception Prompt** - Extracts inventory quantities and qualitative wall topology sequence from images.
   * **Arithmetic Prompt** - Converts topological data into exact, closed metric coordinates using pure vector math.
   * **Integration Prompt** - Normalizes rigid geometry and populates the final scene JSON with elements and views.
   * **Camera Utility Prompt** - Adds pseudo-3D orbit camera views to an existing room JSON.
   * **Repo Analyser Prompt** - Standalone Repo Tester  

<br><br>

## Pipeline

For each room you want to work with, the intended flow is:

### 1. Perception (Inventory & Topology)
   * **Goal:** See the room, count items, walls, c views and determine the sequence of turns defining the shape.
   * **Prompt:** Perception Prompt.
   * **Inputs:**
      * Marked floor plan (boundary arrows + length label in meters for EVERY perimeter segment; ceiling height label `H=` in meters)
      * Interior reference images.
   * **Output:** `perception` JSON containing elements, views, walls, corners, height inforation.

### 2. Arithmetic (Geometry Calculation)
   * **Goal:** Convert the qualitative topology into exact, mathematically closed metric coordinates. No vision allowed.
   * **Prompt:** Arithmetic Prompt.
   * **Input:** `perception` JSON from Step 1.
   * **Output:** `arithmetic` JSON containing exact metric coordinates.

### 3. Integration
   * **Goal:** Normalize the rigid geometry and populate it with the inventory and cameras.
   * **Prompt:** Integration Prompt.
   * **Inputs:**
       * `perception` JSON from Step 1
       * `arithmetic` JSON from Step 2
       * Marked floor plan
       * Interior reference images
   * **Output:** The room JSON model `integration`, placing all elements within a standardised coordinate system.

### 4. Camera Utility
   * **Goal:** Add canonical pseudo-3D orbit views around a specific focus area (e.g., "kitchen", "desk") without modifying the room geometry or elements, to allow for area specific room specifications.
   * **Prompt:** Camera Utility Prompt.
   * **Inputs:**
       * `integration` JSON from Step 3
       * A **focus area** and `focus_key`.
   * **Output:** `integration` JSON with extra orbit-style camera views added to the `views` array.

### Comments 

Before each Nano Banana call, prune views to control outputs and tokens
   * Decide which camera ids you actually want images for on this call.
   * Delete any `views[*]` entries you do not need.
   * The JSON you send to Nano Banana should typically contain:

     * the full `space` and `elems`, but
     * only a small subset of `views` corresponding to shots you want now.

<br><br>

## Perception Prompt (Step 1)

This prompt acts as the project's **"Surveyor"**. It converts visual data into a rigorous, text-based inventory *before* any JSON coding happens.

By separating the "seeing" (inventory) from the "coding" (JSON formatting), we significantly reduce hallucinations and missing items in the final model.

### What it does

* **Zero Estimation:** It forces the model to count every item (e.g., "3x windows" instead of "some windows").
* **Decomposition:** It breaks complex units into parts (e.g., a Kitchen is broken into "Base unit", "Wall unit", "Sink", "Tap").
* **Architectural Scan:** It specifically targets structural features often missed, like beams, cornices, and risers.

### Inputs

* The *Perception Prompt*
* Marked floor plan (boundary arrows + length label in meters for EVERY perimeter segment; ceiling height label `H=` in meters)
* Interior reference images.

### Output Structure

The output is a valid JSON object (`perception`) containing the inventory, topological wall sequence, and view definitions. 
You will pass this JSON output into the *Arithmetic Prompt* and *Integration Prompt* in subsequent steps.

The JSON block is passed directly into:
1. The **Arithmetic Prompt** (to resolve geometry).
2. The **Integration Prompt** (to populate the scene).

A. Architecture & Openings (The Shell)
* **Windows & Doors:** Exact counts of every opening.
* **Structure:** Beams, columns, steps, and level changes.
* **MEP/Fixed:** Radiators, AC units, wall sconces, ceiling fans.

B. Fixed Joinery & Kitchen (The Built-ins)
* **Kitchen:** Breakdown of drawer fronts, cabinet doors, and appliances.
* **Fixtures:** Sinks and taps listed separately from counters.
* **Storage:** Built-in wardrobes and cupboards.

C. Loose Furniture & Decor
* **Seating:** Every chair, stool, and bench counted.
* **Surfaces:** Tables, desks, and freestanding shelves.
* **Groups:** Distinct clusters of clutter (e.g., "Laundry rack group").

<br><br>

## Arithmetic Prompt (Step 2)

This is a deterministic calculation step. It takes the qualitative wall sequence from the Perception step (lengths and turns) and applies pure vector math to determine the exact, closed metric coordinates of the room footprint. It has zero vision capabilities and relies entirely on the input numbers.

**Output:** An intermediate `raw_geometry` JSON containing exact metric vertices and bounds.

<br>

## Integration Prompt (Step 3)

This is the assembly step. It takes the rigid, pre-calculated geometry from the Arithmetic Prompt, normalizes it to [0,1] space, and uses the visual data from images to populate that rigid shape with the inventory defined in the Perception Prompt.

**The resulting JSON is the room model fed into downstream render/design prompts.**

---

### 1. High-level structure

The JSON has six main parts:

1.1 `legend` – mini dictionary of category codes and flags.  
1.2 `proj` – project metadata.  
1.3 `media` – input image filenames.  
1.4 `space` – room geometry (footprint and walls).  
1.5 `views` – camera positions for each reference image / render.  
1.6 `elems` – all room elements (floor, walls, windows, furniture, appliances, clutter).  

Very compact, but enough for coherent geometry and repeated render passes.

---

#### 1.1 `legend`

```json
"legend": {
  "cat": {
    "arch": "architecture surfaces (floor, wall, ceiling)",
    "open": "openings (window, door, arch)",
    "fix":  "fixed elements (ceiling fan, AC, built-in light)",
    "furn": "furniture",
    "appl": "appliances",
    "dec":  "decor",
    "grp":  "group of small items"
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
}
```

Purpose:

- Makes the JSON self-describing.  
- Safe, compact explanation of short codes for any model or human reading it.

---

#### 1.2 `proj`

```json
"proj": {
  "name": "Room_Renovation_Clearout"
}
```

Just a human label for the current room / scenario. Useful for logging or multi-room batches, but not strictly required for geometry.

---

#### 1.3 `media`

```json
"media": {
  "floor": "floorplan.jpg",
  "refs": [
    { "id": "v1", "file": "view_1.jpg" },
    { "id": "v2", "file": "view_2.jpg" },
    { "id": "v3", "file": "view_3.jpg" }
  ]
}
```

- `floor`: floor plan file name.  
- `refs`: interior reference images (each gets an id used later under `views`).

The extractor LLM sees these images directly; the JSON only stores filenames + ids so later steps can refer to them.

---

#### 1.4 `space` – geometry and walls

```json
"space": {
  "geom": {
    "bounds": [4.5, 3.2],
    "pts": [[0,0],[1,0],[1,0.71],[0.4,0.71],[0,1]],
    "H": 2.6,
    "walls": [
      { "id": "w1", "seq": 1, "p0": 0, "p1": 1 },
      { "id": "w2", "seq": 2, "p0": 1, "p1": 2 },
      { "id": "w3", "seq": 3, "p0": 2, "p1": 3 },
      { "id": "w4", "seq": 4, "p0": 3, "p1": 4 },
      { "id": "w5", "seq": 5, "p0": 4, "p1": 0 }
    ]
  }
}
```

- `bounds`: [width, length] in **physical meters** (read from floor plan dimensions).
- `pts`: polygon vertices of the room footprint, **normalised** to [0,1] via Uniform Scaling (aspect ratio preserved).
  - Index i in this array is used as `p0` / `p1` in walls.  
- `H`: ceiling height in meters
- `walls`: each wall is a segment between two vertices (`p0`→`p1`) and has:
  - `id`: stable identifier (`"w1"`, `"w2"`, …).  
  - `seq`: perimeter order index (1-based).  
  - `label`: short human description.

This gives Nano Banana a reusable, explicit floor shape + wall segmentation, rather than only implicit geometry in text.

---

#### 1.5 `views` – cameras for each reference / render

```json
"views": [
  {
    "id": "v1",
    "ref": "v1",
    "cam": {
      "rel": "corner",
      "w1": "w1",
      "w2": "w2",
      "xy": [0.05, 0.05],
      "h": 1.6
    }
  },
  {
    "id": "v2",
    "ref": "v2",
    "cam": {
      "rel": "wall",
      "w1": "w2",
      "w2": null,
      "xy": [0.85, 0.20],
      "h": 1.6
    }
  }
]
```

- `id`: view id.  
- `ref`: which `media.refs[*].id` this view corresponds to (for reference images).  
- `cam`:
  - `rel`: `"corner"`, `"wall"`, or `"free"` – qualitative camera placement.  
  - `w1`, `w2`: which wall(s) the camera relates to.  
  - `xy`: camera floor position in the same normalised coordinates as `space.geom.pts`.  
  - `h`: camera height (m).

For reference images, the extractor fills these. For synthetic render views, you or a design prompt can add more entries with `ref: null`.

---

#### 1.6 `elems` – all elements in the room

Each entry describes one element or a group of elements.

Example:

```json
"elems": [
  {
    "id": "floor_1",
    "cat": "arch",
    "type": "floor",
    "pos": {
      "rel": "floor",
      "w1": null,
      "w2": null,
      "xy": [0.5, 0.5],
      "h": 0,
      "sz": [null, null, null]
    },
    "views": [],
    "d": "tile_terracotta_30x30_matt_black_grout",
    "rm": false,
    "repl": null
  },
  {
    "id": "win_1",
    "cat": "open",
    "type": "window",
    "pos": {
      "rel": "on",
      "w1": "w2",
      "w2": null,
      "xy": [0.8, 0.1],
      "h": 1.0,
      "sz": [1.6, 1.2, 0.2]
    },
    "views": [
      { "v": "v1", "bb": [0.55, 0.80, 0.30, 0.65], "vis": "f" }
    ],
    "d": "window_casement_black_aluminium_clear_glass",
    "rm": false,
    "repl": null
  }
]
```

Key fields:

- `id`: stable element id.  
- `cat`: high-level category (matches `legend.cat`).  
- `type`: specific element type.  
- `pos`:
  - `rel`: relationship to walls/floor/ceiling (`on`, `between`, `floor`, `ceil`).  
  - `w1`, `w2`: associated wall ids.  
  - `xy`: floor position in the normalised footprint.  
  - `h`: vertical position (m).  
  - `sz`: approximate `[width, height, depth]` (m) or `[null,…]` if unknown.

- `views`: for each reference view this element appears in:
  - `v`: view id (`"v1"`, `"v2"`, …).  
  - `bb`: bounding box in normalised image coordinates `[xmin, xmax, ymin, ymax]` with x,y in [0,1].  
  - `vis`: `"f"`, `"p"`, `"o"` (fully / partial / occluded).

- `d`: short compressed description for materials / colour / shape.  
- `rm`: whether this element should be removed in a “clear-out” render.  
- `repl`: what to show instead if removed (`"plain_wall"`, `"extend_floor_and_walls"`, etc.).

---

### 2. Usage

#### 2.1 Step 1 – Extraction (this repo file)

1. Input:
   - *Integration Prompt*
   - `perception` JSON
   - `arithmetic` JSON
   -  Marked floor plan (boundary arrows + length label in meters for EVERY perimeter segment; ceiling height label `H=` in meters)
   -  Interior reference images.

2. Output:  
   - `integration` JSON, which is now your **room model**.

---

#### 2.2 Step 2 – Design / clear-out / pseudo-3D work

In a new Nano Banana chat:

1. Provide:

   * The room JSON from step 1.
   * Any subset of the original reference images you want to stay aligned with.

2. Ask for changes in terms of this model, for example:

   * “Set `rm = true` on the existing sofa and armchair elements, extend the floor and skirting behind them, and update the JSON.”
   * “Add a kitchen run along wall `w2` under window `win_1`, about 2 m long, with base cabinets and a worktop.”
   * “Create three new orbit views around the kitchen area with ids `v_kit1_front`, `v_kit1_left`, `v_kit1_right`.”

3. The design or view-creation prompt updates:

   * `elems` (adding/removing/modifying elements and their `rm` / `repl` flags).
   * `views` (adding or updating camera positions).

4. Before each render call, keep only the `views` you want images for on that call and delete the rest, then send the pruned JSON to Nano Banana together with the relevant images.

---

### 3. Design principles

- **Geometry first** – `space.geom` + `xy` + `H` ensure the room shape is explicit and reusable.  
- **Per-Element Flags** – `cat` and `rm` / `repl` give you simple clear-out and redesign behaviour per element.
- **Compact but English-like** – field values like `furn`, `appl`, `tile_terracotta_30x30_matt_black_grout` remain readable for humans and LLMs while staying token-efficient.  

This setup lets you use the same JSON both as:

- a compact intermediate representation for Nano Banana; and  
- a stable “contract” between extraction, design, and rendering stages.

<br><br>
  
## Camera Utility Prompt

This spec takes the `integration` JSON and adds a small family of pseudo-3D “orbit” views around a chosen focus area.

### What it does

* You choose a focus area (for example “front of the kitchen run”, “TV wall”, or a specific element / wall) and a short `focus_key` (e.g. `kit1`, `tv`, `desk`).

* The LLM computes a `focus_xy` point inside the room and creates up to four standard views:

  * `v_<focus_key>_front`
  * `v_<focus_key>_left`
  * `v_<focus_key>_right`
  * `v_<focus_key>_over` (optional overhead / high angle)

* All of these are added to the `views` array. They are **just more cameras**; which of them you render on a given call is up to you.

The spec never changes walls or elements. It only adds or updates `views` whose ids start with `v_<focus_key>_`.

### Inputs

To use this spec you need:

* The `integration` JSON
* A text instruction that describes:

  * The `focus_key` you want to use.
  * How to locate the focus:

    * element id(s), or
    * wall id(s), or
    * an explicit normalised coordinate `[x, y]` in [0,1]×[0,1], or
    * a short description like “middle of the south wall” or “centre of the TV wall”.

### Outputs and usage pattern

The result is the same JSON object, but with additional `views` entries.

Typical pattern:

1. Paste the `integration` JSON.

2. Paste the full text from Camera Utility Prompt.

3. Add an instruction, for example:

   * “Using `focus_key = "kit1"`, add orbit views around the main kitchen wall and return the full updated JSON only.”

4. From the updated JSON, keep only the view ids you want to render on this call (for example: `["v_kit1_front", "v_kit1_left"]`) and delete the rest of `views` before sending to Nano Banana.

