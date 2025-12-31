#### Task: Analyze floor plan and reference photos to extract:
1. **Geometry:**
    1.1 List of walls including measurements; room height  
    1.2 Enumerate room-corners and wall connectivity of walls listed in 1.1
3. **Inventory:** Rigorous "Bill of Quantities" of ALL interior elements
4. **Views:** Convert EACH reference photo into a "view" 

#### CRITICAL RULES:
1. **Geometry:**
    1.1 Use ONLY user-marked boundary wall segments indicated by arrows + length labels (e.g., "1.9m") defining the target room perimeter; ignore ALL other plan walls/spaces (e.g., stairwell). Assign deterministic wall ids w1..wN in clockwise perimeter order. If any boundary segment is missing/ambiguous, output questions and stop.
    1.2 Output a SINGLE closed loop: list `space.corners[]` CLOCKWISE; each `space.walls[]` uses adjacent corners (c0→c1 … c(n-1)→c0).

3. **Inventory:** 
    *  Zero estimation. Count exactly (e.g. "3x Chairs", not "some chairs"). Decompose assemblies (Table + Chairs = separate items)
    *  Look specifically for structural features: beams, columns, arches, steps, dado rails, cornices, and skylights.
4. **Views:** Create exactly one `views[]` entry per reference image with id (e.g., `v_1`, `v_2`) and `ref` with image filename/id

#### OUTPUT FORMAT:
- Return ONLY valid, concise JSON
- Render array objects on single lines
- Use this exact schema:

{
"bill_of_quantities": {
  "legend": {
      "arch": "architecture / structural surfaces & features (beams, columns, exposed trusses, steps, level changes, distinctive floor sections (if multiple types), ceiling features)",
      "open": "openings (window, door, passage/arch)",
      "fix": "fixed elements & joinery",
      "appl": "appliances",
      "furn": "furniture",
      "dec": "decor",
      "grp": "grouped clutter volumes",
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
    { "cat": "arch" | "open" | "fix" | "furn" | "appl" | "dec" | "grp", "d": string },
    { "cat": "arch" | "open" | "fix" | "furn" | "appl" | "dec" | "grp", "d": string }
  ],
  "views": [
    { "id": string, "ref": string },
    { "id": string, "ref": string }
  ]
}
}
