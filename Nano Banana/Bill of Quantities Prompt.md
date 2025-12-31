#### Task: Analyze floor plan and reference photos to extract:
1. **Geometry:**
    1.1 List of walls including measurements; room height  
    1.2 Enumerate room-corners and wall connectivity of walls listed in 1.1
2. **Inventory:** Rigorous "Bill of Quantities" of ALL interior elements
3. **Views:** Convert EACH reference photo into a "view" 

#### CRITICAL RULES:
1. **Geometry:**
    1.1 Use ONLY user-marked boundary wall segments indicated by arrows + length labels (e.g., "1.9m") defining the target room perimeter; ignore ALL other plan walls/spaces (e.g., stairwell). Assign deterministic wall ids w1..wN in clockwise perimeter order. If any boundary segment is missing/ambiguous, output questions and stop.
    1.2 Output a SINGLE closed loop: list `space.corners[]` CLOCKWISE; each `space.walls[]` uses adjacent corners (c0→c1 … c(n-1)→c0).
2. **Inventory:** 
    *  Zero estimation. Count exactly. Include count in "d" if quantity > 1 (e.g. "3x Chairs", not "some chairs"). Decompose assemblies (Table + Chairs = separate items)
    *  Wall association For wall-attached/embedded elements:
        * Set `"w"` to wall id string (e.g.`"w4"`).
        * If "d" doesn't include explicit count, `"w":"<wall_id>"`
        * If "d" includes explicit count like `"4x ..."`
            * encode ONE wall id per instance (e.g. `"w":"w1, w2, w5, w6"`)
            * repeat ids when multiple are on the same wall (e.g. `"w":"w4, w4, w7, w7"`)
            * amount of ids MUST match explicit count
            * list wall ids in ascending perimeter id order (w1..wN), grouping repeats together.
        * If not wall-tied, set `"w": null`.
        * Never encode wall ids in `"d"`
    *  Look specifically for structural features: beams, columns, arches, steps, dado rails, cornices, and skylights.
3. **Views:** Create exactly one `views[]` entry per reference image with id (e.g., `v_1`, `v_2`) and `ref` with image filename/id

#### OUTPUT FORMAT:
- Return ONLY valid, concise JSON
- Render array objects on single lines
- Use this exact schema:

{
"BoQ": {
  "legend": {
      "arch": "architecture / structural surfaces & features (beams, columns, exposed trusses, steps, level changes, distinctive floor sections (if multiple types), ceiling features)",
      "open": "openings (window, door, passage/arch)",
      "fix": "fixed elements & joinery",
      "appl": "appliances",
      "furn": "furniture",
      "dec": "decor",
      "grp": "grouped clutter volumes",
      "w_id": "wall_id",
      "L": "length_in_meters",
      "H": "ceiling_height_in_meters"
    },
  "space": { "corner_order": "CW",
    "walls": [
      { "id": string, "L": number, "c0": string, "c1": string },
      { "id": string, "L": number, "c0": string, "c1": string }    
    ],
    "corners": [
      { "id": string },
      { "id": string }
    ],
    "H": number
  },
  "elems": [
    { "cat": "arch" | "open" | "fix" | "furn" | "appl" | "dec" | "grp", "w_id": string | null, "d": string },
    { "cat": "arch" | "open" | "fix" | "furn" | "appl" | "dec" | "grp", "w_id": string | null, "d": string }
  ],
  "views": [
    { "id": string, "ref": string },
    { "id": string, "ref": string }
  ]
}
}
