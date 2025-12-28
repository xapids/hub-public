#### Task: Analyze floor plan and reference photos to extract:
1. **Geometry:**
    1.1 List of walls including measurements; room height
    1.2 Enumerate room-corners and wall connectivity of walls listed in 1.1
2. **Inventory:** Rigorous "Bill of Quantities" of ALL interior elements
3. **Views:** Convert EACH reference photo into a "view" 

#### CRITICAL RULES:
1. **Geometry:**
    1.1 Scan floor plan for dimension text (e.g., "1.9m"). List ONLY walls with explicit text. Ignore unmeasured lines.
    1.2 Using ONLY walls in 1.1, define `space.corners[]` + each wall’s endpoints c0, c1, …
3. **Inventory:** Zero estimation. Count exactly (e.g. "3x ...hairs"). Decompose assemblies (Table + Chairs = separate items) 
2. **Inventory:** 
    *  Zero estimation. Count exactly (e.g. "3x Chairs", not "some chairs"). Decompose assemblies (Table + Chairs = separate items)
    *  Look specifically for structural features: beams, columns, arches, steps, dado rails, cornices, and skylights.
3. **Views:** Create exactly one `views[]` entry per reference image with id (e.g., `v_1`, `v_2`) and `ref` with image filename/id

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
      "wall": "walls",
      "L": "length_in_meters",
      "H": "ceiling_height_in_meters"
    },
  "space": {
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
  ]
  "views": [
    { "id": string, "ref": string },
    { "id": string, "ref": string }
  ],
}
}
