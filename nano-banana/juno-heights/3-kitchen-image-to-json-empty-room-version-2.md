Step 1: Review the 4 attached images: Floorplan, Reference 1, Reference 2, Reference 3
Step 2: Faithfully follow this JSON rendering all views of the exact room, removing all elements marked as ""rm": true"


{
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
    "name": "Utility Room"
  },
  "media": {
    "floor": "image_0.png",
    "refs": [
      { "id": "v_1", "file": "Reference_1.png" },
      { "id": "v_2", "file": "Reference_2.png" },
      { "id": "v_3", "file": "Reference_3.png" }
    ]
  },
  "space": {
    "geom": {
      "bounds": [3.6, 7.2],
      "pts": [
        [0.0, 1.0],
        [0.2639, 1.0],
        [0.2639, 0.5972],
        [0.5, 0.5972],
        [0.5, 0.0],
        [0.1389, 0.0],
        [0.0, 0.1389]
      ],
      "H": 2.7,
      "orientation": "+y",
      "walls": [
        { "id": "w1", "seq": 1, "p0": 0, "p1": 1 },
        { "id": "w2", "seq": 2, "p0": 1, "p1": 2 },
        { "id": "w3", "seq": 3, "p0": 2, "p1": 3 },
        { "id": "w4", "seq": 4, "p0": 3, "p1": 4 },
        { "id": "w5", "seq": 5, "p0": 4, "p1": 5 },
        { "id": "w6", "seq": 6, "p0": 5, "p1": 6 },
        { "id": "w7", "seq": 7, "p0": 6, "p1": 0 }
      ]
    }
  },
  "views": [
    {
      "id": "v_1",
      "ref": "v_1",
      "cam": { "rel": "free", "w1": null, "w2": null, "xy": [0.25, 0.5], "h": 1.6 }
    },
    {
      "id": "v_2",
      "ref": "v_2",
      "cam": { "rel": "corner", "w1": "w7", "w2": "w1", "xy": [0.05, 0.95], "h": 1.6 }
    },
    {
      "id": "v_3",
      "ref": "v_3",
      "cam": { "rel": "wall", "w1": "w4", "w2": null, "xy": [0.45, 0.3], "h": 1.6 }
    }
  ],
  "elems": [
    {
      "id": "floor_1",
      "cat": "arch",
      "type": "floor",
      "pos": { "rel": "floor", "w1": null, "w2": null, "xy": [0.25, 0.5], "h": 0, "sz": [null, null, null] },
      "views": [
        { "v": "v_1", "bb": [0.0, 1.0, 0.0, 0.5], "vis": "p" },
        { "v": "v_2", "bb": [0.0, 1.0, 0.0, 0.6], "vis": "p" },
        { "v": "v_3", "bb": [0.0, 1.0, 0.0, 0.5], "vis": "p" }
      ],
      "d": "terracotta_tiled_floor",
      "rm": false,
      "repl": null
    },
    {
      "id": "fan_1",
      "cat": "fix",
      "type": "ceiling_fan",
      "pos": { "rel": "ceil", "w1": null, "w2": null, "xy": [0.25, 0.5], "h": 2.7, "sz": [1.2, 0.4, 1.2] },
      "views": [
        { "v": "v_1", "bb": [0.4, 0.6, 0.8, 1.0], "vis": "f" },
        { "v": "v_2", "bb": [0.5, 0.7, 0.8, 0.95], "vis": "f" },
        { "v": "v_3", "bb": [0.45, 0.65, 0.85, 1.0], "vis": "f" }
      ],
      "d": "ceiling_fan_with_light",
      "rm": false,
      "repl": null
    },
    {
      "id": "door_1",
      "cat": "open",
      "type": "door",
      "pos": { "rel": "on", "w1": "w6", "w2": null, "xy": [0.07, 0.07], "h": 1.0, "sz": [0.9, 2.0, null] },
      "views": [
        { "v": "v_2", "bb": [0.0, 0.1, 0.2, 0.8], "vis": "p" },
        { "v": "v_3", "bb": [0.85, 1.0, 0.2, 0.8], "vis": "p" }
      ],
      "d": "black_aluminium_door",
      "rm": false,
      "repl": null
    },
    {
      "id": "door_2",
      "cat": "open",
      "type": "door",
      "pos": { "rel": "on", "w1": "w7", "w2": null, "xy": [0.0, 0.5], "h": 1.0, "sz": [0.9, 2.0, null] },
      "views": [
        { "v": "v_2", "bb": [0.1, 0.3, 0.2, 0.8], "vis": "p" }
      ],
      "d": "black_aluminium_door",
      "rm": false,
      "repl": null
    },
    {
      "id": "win_1",
      "cat": "open",
      "type": "window",
      "pos": { "rel": "on", "w1": "w1", "w2": null, "xy": [0.13, 1.0], "h": 1.5, "sz": [1.0, 1.2, null] },
      "views": [
        { "v": "v_1", "bb": [0.25, 0.4, 0.3, 0.7], "vis": "f" },
        { "v": "v_2", "bb": [0.6, 0.75, 0.3, 0.6], "vis": "f" }
      ],
      "d": "black_aluminium_window",
      "rm": false,
      "repl": null
    },
    {
      "id": "win_2",
      "cat": "open",
      "type": "window",
      "pos": { "rel": "on", "w1": "w4", "w2": null, "xy": [0.5, 0.3], "h": 1.5, "sz": [1.2, 1.2, null] },
      "views": [
        { "v": "v_1", "bb": [0.85, 1.0, 0.3, 0.7], "vis": "p" },
        { "v": "v_3", "bb": [0.5, 0.7, 0.3, 0.7], "vis": "f" }
      ],
      "d": "black_aluminium_window",
      "rm": false,
      "repl": null
    },
    {
      "id": "win_3",
      "cat": "open",
      "type": "window",
      "pos": { "rel": "on", "w1": "w5", "w2": null, "xy": [0.3, 0.0], "h": 1.5, "sz": [1.0, 1.2, null] },
      "views": [
        { "v": "v_3", "bb": [0.7, 0.85, 0.3, 0.7], "vis": "f" }
      ],
      "d": "black_aluminium_window",
      "rm": false,
      "repl": null
    },
    {
      "id": "win_4",
      "cat": "open",
      "type": "window",
      "pos": { "rel": "on", "w1": "w7", "w2": null, "xy": [0.0, 0.8], "h": 1.5, "sz": [1.0, 1.2, null] },
      "views": [
        { "v": "v_1", "bb": [0.0, 0.1, 0.3, 0.7], "vis": "p" },
        { "v": "v_2", "bb": [0.3, 0.45, 0.3, 0.6], "vis": "f" }
      ],
      "d": "black_aluminium_window",
      "rm": false,
      "repl": null
    },
    {
      "id": "sconce_1",
      "cat": "fix",
      "type": "sconce",
      "pos": { "rel": "on", "w1": "w4", "w2": null, "xy": [0.5, 0.45], "h": 1.8, "sz": [null, null, null] },
      "views": [],
      "d": "black_wall_sconce",
      "rm": false,
      "repl": null
    },
    {
      "id": "sconce_2",
      "cat": "fix",
      "type": "sconce",
      "pos": { "rel": "on", "w1": "w4", "w2": null, "xy": [0.5, 0.15], "h": 1.8, "sz": [null, null, null] },
      "views": [],
      "d": "black_wall_sconce",
      "rm": false,
      "repl": null
    },
    {
      "id": "sconce_3",
      "cat": "fix",
      "type": "sconce",
      "pos": { "rel": "on", "w1": "w7", "w2": null, "xy": [0.0, 0.65], "h": 1.8, "sz": [null, null, null] },
      "views": [],
      "d": "black_wall_sconce",
      "rm": false,
      "repl": null
    },
    {
      "id": "sconce_4",
      "cat": "fix",
      "type": "sconce",
      "pos": { "rel": "on", "w1": "w7", "w2": null, "xy": [0.0, 0.35], "h": 1.8, "sz": [null, null, null] },
      "views": [],
      "d": "black_wall_sconce",
      "rm": false,
      "repl": null
    },
    {
      "id": "arch_1",
      "cat": "open",
      "type": "archway",
      "pos": { "rel": "on", "w1": "w3", "w2": null, "xy": [0.38, 0.5972], "h": 1.0, "sz": [1.0, 2.0, null] },
      "views": [
        { "v": "v_1", "bb": [0.6, 0.8, 0.2, 0.8], "vis": "f" },
        { "v": "v_3", "bb": [0.0, 0.2, 0.2, 0.8], "vis": "p" }
      ],
      "d": "countertop_height_archway",
      "rm": true,
      "repl": "extended_wall"
    },
    {
      "id": "dumbwaiter_1",
      "cat": "fix",
      "type": "dumbwaiter",
      "pos": { "rel": "on", "w1": "w2", "w2": null, "xy": [0.2639, 0.8], "h": 1.0, "sz": [0.6, 0.8, 0.6] },
      "views": [
        { "v": "v_1", "bb": [0.4, 0.5, 0.3, 0.6], "vis": "f" },
        { "v": "v_2", "bb": [0.75, 0.85, 0.3, 0.5], "vis": "f" }
      ],
      "d": "dumbwaiter_wooden_frame_metal_doors",
      "rm": false,
      "repl": null
    },
    {
      "id": "counter_1",
      "cat": "fix",
      "type": "countertop",
      "pos": { "rel": "on", "w1": "w4", "w2": null, "xy": [0.5, 0.3], "h": 0.9, "sz": [2.0, 0.05, 0.6] },
      "views": [
        { "v": "v_1", "bb": [0.7, 1.0, 0.4, 0.5], "vis": "p" },
        { "v": "v_3", "bb": [0.3, 0.8, 0.4, 0.5], "vis": "f" }
      ],
      "d": "wooden_countertop_with_sink",
      "rm": true,
      "repl": "extend_floor_and_walls"
    },
    {
      "id": "cab_lower_1",
      "cat": "fix",
      "type": "cabinet",
      "pos": { "rel": "on", "w1": "w4", "w2": null, "xy": [0.5, 0.3], "h": 0.0, "sz": [2.0, 0.9, 0.6] },
      "views": [
        { "v": "v_1", "bb": [0.7, 1.0, 0.1, 0.4], "vis": "p" },
        { "v": "v_3", "bb": [0.3, 0.8, 0.1, 0.4], "vis": "f" }
      ],
      "d": "wooden_lower_kitchen_cabinets",
      "rm": true,
      "repl": "extend_floor_and_walls"
    },
    {
      "id": "cab_upper_1",
      "cat": "fix",
      "type": "cabinet",
      "pos": { "rel": "on", "w1": "w3", "w2": null, "xy": [0.38, 0.5972], "h": 1.5, "sz": [0.8, 0.6, 0.3] },
      "views": [
        { "v": "v_1", "bb": [0.6, 0.7, 0.6, 0.8], "vis": "f" }
      ],
      "d": "wall_mounted_wooden_upper_cabinet",
      "rm": true,
      "repl": "plain_wall"
    },
    {
      "id": "shelf_1",
      "cat": "fix",
      "type": "shelf",
      "pos": { "rel": "on", "w1": "w4", "w2": null, "xy": [0.5, 0.3], "h": 1.5, "sz": [2.0, 0.05, 0.3] },
      "views": [
        { "v": "v_1", "bb": [0.7, 1.0, 0.6, 0.7], "vis": "p" },
        { "v": "v_3", "bb": [0.3, 0.8, 0.6, 0.7], "vis": "f" }
      ],
      "d": "wooden_shelf_above_countertop",
      "rm": true,
      "repl": "plain_wall"
    },
    {
      "id": "clothesline_1",
      "cat": "fix",
      "type": "clothesline",
      "pos": { "rel": "ceil", "w1": null, "w2": null, "xy": [0.25, 0.5], "h": 2.5, "sz": [null, null, null] },
      "views": [
        { "v": "v_1", "bb": [0.0, 1.0, 0.7, 1.0], "vis": "f" },
        { "v": "v_2", "bb": [0.0, 1.0, 0.7, 1.0], "vis": "f" },
        { "v": "v_3", "bb": [0.0, 1.0, 0.7, 1.0], "vis": "f" }
      ],
      "d": "clothesline_string_system",
      "rm": true,
      "repl": "extend_floor_and_walls"
    },
    {
      "id": "wash_1",
      "cat": "appl",
      "type": "washing_machine",
      "pos": { "rel": "on", "w1": "w4", "w2": null, "xy": [0.5, 0.15], "h": 0.0, "sz": [0.6, 0.85, 0.6] },
      "views": [
        { "v": "v_3", "bb": [0.1, 0.3, 0.1, 0.4], "vis": "f" }
      ],
      "d": "washing_machine",
      "rm": true,
      "repl": "extend_floor_and_walls"
    },
    {
      "id": "dryer_1",
      "cat": "appl",
      "type": "dryer",
      "pos": { "rel": "on", "w1": "w4", "w2": null, "xy": [0.5, 0.45], "h": 0.0, "sz": [0.6, 0.85, 0.6] },
      "views": [
        { "v": "v_1", "bb": [0.6, 0.7, 0.1, 0.4], "vis": "p" }
      ],
      "d": "dryer",
      "rm": true,
      "repl": "extend_floor_and_walls"
    },
    {
      "id": "ac_1",
      "cat": "appl",
      "type": "ac_unit",
      "pos": { "rel": "on", "w1": "w4", "w2": null, "xy": [0.5, 0.5], "h": 2.2, "sz": [0.8, 0.3, 0.2] },
      "views": [
        { "v": "v_1", "bb": [0.8, 1.0, 0.8, 0.95], "vis": "f" }
      ],
      "d": "wall_mounted_ac_unit",
      "rm": false,
      "repl": null
    },
    {
      "id": "table_1",
      "cat": "furn",
      "type": "table",
      "pos": { "rel": "floor", "w1": null, "w2": null, "xy": [0.3, 0.5], "h": 0.0, "sz": [1.8, 0.75, 1.0] },
      "views": [
        { "v": "v_1", "bb": [0.2, 0.6, 0.2, 0.5], "vis": "f" },
        { "v": "v_2", "bb": [0.4, 0.7, 0.2, 0.5], "vis": "f" },
        { "v": "v_3", "bb": [0.5, 0.8, 0.2, 0.5], "vis": "f" }
      ],
      "d": "large_black_oval_dining_table",
      "rm": true,
      "repl": "extend_floor_and_walls"
    },
    {
      "id": "chair_1",
      "cat": "furn",
      "type": "chair",
      "pos": { "rel": "floor", "w1": null, "w2": null, "xy": [0.25, 0.4], "h": 0.0, "sz": [0.5, 0.8, 0.5] },
      "views": [
        { "v": "v_1", "bb": [0.1, 0.3, 0.2, 0.4], "vis": "p" }
      ],
      "d": "black_metal_dining_chair",
      "rm": true,
      "repl": "extend_floor_and_walls"
    },
    {
      "id": "chair_2",
      "cat": "furn",
      "type": "chair",
      "pos": { "rel": "floor", "w1": null, "w2": null, "xy": [0.35, 0.4], "h": 0.0, "sz": [0.5, 0.8, 0.5] },
      "views": [],
      "d": "black_metal_dining_chair",
      "rm": true,
      "repl": "extend_floor_and_walls"
    },
    {
      "id": "chair_3",
      "cat": "furn",
      "type": "chair",
      "pos": { "rel": "floor", "w1": null, "w2": null, "xy": [0.45, 0.4], "h": 0.0, "sz": [0.5, 0.8, 0.5] },
      "views": [],
      "d": "black_metal_dining_chair",
      "rm": true,
      "repl": "extend_floor_and_walls"
    },
    {
      "id": "chair_4",
      "cat": "furn",
      "type": "chair",
      "pos": { "rel": "floor", "w1": null, "w2": null, "xy": [0.25, 0.6], "h": 0.0, "sz": [0.5, 0.8, 0.5] },
      "views": [],
      "d": "black_metal_dining_chair",
      "rm": true,
      "repl": "extend_floor_and_walls"
    },
    {
      "id": "chair_5",
      "cat": "furn",
      "type": "chair",
      "pos": { "rel": "floor", "w1": null, "w2": null, "xy": [0.35, 0.6], "h": 0.0, "sz": [0.5, 0.8, 0.5] },
      "views": [],
      "d": "black_metal_dining_chair",
      "rm": true,
      "repl": "extend_floor_and_walls"
    },
    {
      "id": "chair_6",
      "cat": "furn",
      "type": "chair",
      "pos": { "rel": "floor", "w1": null, "w2": null, "xy": [0.45, 0.6], "h": 0.0, "sz": [0.5, 0.8, 0.5] },
      "views": [],
      "d": "black_metal_dining_chair",
      "rm": true,
      "repl": "extend_floor_and_walls"
    },
    {
      "id": "chair_7",
      "cat": "furn",
      "type": "chair",
      "pos": { "rel": "floor", "w1": null, "w2": null, "xy": [0.2, 0.5], "h": 0.0, "sz": [0.5, 0.8, 0.5] },
      "views": [],
      "d": "black_metal_dining_chair",
      "rm": true,
      "repl": "extend_floor_and_walls"
    },
    {
      "id": "chair_8",
      "cat": "furn",
      "type": "chair",
      "pos": { "rel": "floor", "w1": null, "w2": null, "xy": [0.5, 0.5], "h": 0.0, "sz": [0.5, 0.8, 0.5] },
      "views": [],
      "d": "black_metal_dining_chair",
      "rm": true,
      "repl": "extend_floor_and_walls"
    },
    {
      "id": "ironing_board_1",
      "cat": "furn",
      "type": "ironing_board",
      "pos": { "rel": "floor", "w1": null, "w2": null, "xy": [0.4, 0.8], "h": 0.0, "sz": [1.2, 0.9, 0.4] },
      "views": [
        { "v": "v_2", "bb": [0.6, 0.8, 0.4, 0.6], "vis": "f" },
        { "v": "v_3", "bb": [0.8, 1.0, 0.4, 0.6], "vis": "p" }
      ],
      "d": "ironing_board",
      "rm": true,
      "repl": "extend_floor_and_walls"
    },
    {
      "id": "poster_1",
      "cat": "dec",
      "type": "poster",
      "pos": { "rel": "on", "w1": "w7", "w2": null, "xy": [0.0, 0.7], "h": 1.5, "sz": [0.5, 0.7, null] },
      "views": [
        { "v": "v_1", "bb": [0.1, 0.2, 0.4, 0.6], "vis": "f" }
      ],
      "d": "vintage_art_poster",
      "rm": false,
      "repl": null
    },
    {
      "id": "poster_2",
      "cat": "dec",
      "type": "poster",
      "pos": { "rel": "on", "w1": "w7", "w2": null, "xy": [0.0, 0.4], "h": 1.5, "sz": [0.5, 0.7, null] },
      "views": [],
      "d": "vintage_art_poster",
      "rm": false,
      "repl": null
    },
    {
      "id": "basket_1",
      "cat": "grp",
      "type": "laundry_basket",
      "pos": { "rel": "floor", "w1": null, "w2": null, "xy": [0.1, 0.2], "h": 0.0, "sz": [0.4, 0.5, 0.4] },
      "views": [
        { "v": "v_1", "bb": [0.0, 0.3, 0.0, 0.4], "vis": "p" }
      ],
      "d": "blue_laundry_basket",
      "rm": true,
      "repl": "extend_floor_and_walls"
    }
  ]
}
