{
  "diagram_spec": {
    "meta": {
      "spec_version": "1.0.0",
      "title": "AI Pipeline Development Stack",
      "description": "Technical infographic of a layered AI system architecture. Vertical stack with Layer 5 at top and Layer 0 at bottom. Execution flow arrows run top → bottom. Style: minimal, old fantasy, high contrast, clear separation of responsibilities. All text sans-serif. Includes a parallel side channel for Persistence & Traceability and a separate side layer for Screen/GUI Environment. Use dashed connectors for side channels and screen visibility. Avoid marketing language; keep precise and technical.",
      "author": "",
      "tags": [
        "infographic",
        "architecture",
        "ai",
        "cli",
        "agents",
        "human-in-the-loop",
        "minimal",
        "high-contrast",
        "old-fantasy-tech",
        "sans-serif"
      ]
    },
    "canvas": {
      "width": 1920,
      "height": 1080,
      "unit": "px",
      "direction": "bottom_to_top"
    },
    "semantics": {
      "diagram_type": "flowchart",
      "primary_relationship": "control_flow",
      "swimlanes": []
    },
    "nodes": [
      {
        "id": "title",
        "label": "AI Pipeline Development Stack",
        "role": "title",
        "lane": null,
        "group_id": "grp_header",
        "position": { "x": 140, "y": 40 },
        "size": { "width": 1140, "height": 70 },
        "style": { "shape": "rectangle", "fill_color": "#FFFFFF", "border_color": "#FFFFFF" },
        "data": { "font_family": "sans-serif", "font_weight": "700", "text_align": "center" }
      },
      {
        "id": "subtitle",
        "label": "Execution flow: top → bottom",
        "role": "note",
        "lane": null,
        "group_id": "grp_header",
        "position": { "x": 140, "y": 105 },
        "size": { "width": 1140, "height": 40 },
        "style": { "shape": "rectangle", "fill_color": "#FFFFFF", "border_color": "#FFFFFF" },
        "data": { "font_family": "sans-serif", "font_weight": "400", "text_align": "center" }
      },
      {
        "id": "layer5",
        "label": "LAYER 5 — Reasoning / Planning (LLMs)\nLocation: Model inference\nPurpose: Decide what should be done\nExamples:\n• Codex\n• Claude Code\n• Planning / ReAct logic\n• Tool selection\nDescription:\n“Produces intent and plans. No direct side effects.”",
        "role": "process",
        "lane": null,
        "group_id": "grp_main_stack",
        "position": { "x": 140, "y": 160 },
        "size": { "width": 1140, "height": 130 },
        "style": { "shape": "rectangle", "fill_color": "#FFFFFF", "border_color": "#111827" },
        "data": { "font_family": "sans-serif" }
      },
      {
        "id": "layer4",
        "label": "LAYER 4 — Agent Runtime / Orchestrator\nLocation: Application runtime\nPurpose: Coordinate workflows and tools\nExamples:\n• Agents\n• Sub-agents\n• Skill runners\n• Task orchestration\nDescription:\n“Plans and sequences actions. Requests execution but does not touch the OS directly.”",
        "role": "process",
        "lane": null,
        "group_id": "grp_main_stack",
        "position": { "x": 140, "y": 300 },
        "size": { "width": 1140, "height": 140 },
        "style": { "shape": "rectangle", "fill_color": "#FFFFFF", "border_color": "#111827" },
        "data": { "font_family": "sans-serif" }
      },
      {
        "id": "layer3",
        "label": "LAYER 3 — Execution Boundary (Critical Control Plane)\nLocation: Inside agent runtime or wrapper process\nPurpose: Gate side-effecting actions\nExamples:\n• HumanLayer (human-in-the-loop approval wrapper)\n• Policy/ approval wrappers\n• SDK calls pass through this boundary before execution\n• Hooks (lifecycle + execution triggers)\nDescription:\n“Intercepts tool/SDK calls. Pauses before side effects. Requires approval. Logs decisions.”\n\nBoundary Between Intent and Action\nSDK → Wrapper → Execute",
        "role": "process",
        "lane": null,
        "group_id": "grp_main_stack",
        "position": { "x": 140, "y": 450 },
        "size": { "width": 1140, "height": 170 },
        "style": { "shape": "rectangle", "fill_color": "#FFFFFF", "border_color": "#F97316" },
        "data": { "font_family": "sans-serif", "emphasis": "highlight_border_orange" }
      },
      {
        "id": "layer2",
        "label": "LAYER 2 — CLI Programs (Deterministic Tools)\nLocation: Executable processes and scripts\nPurpose: Perform deterministic actions\nExamples:\n• CLI Programs\n• Scripts (wrapping CLI + SDK calls)\n• SDKs (libraries used by scripts/tools)\nDescription:\n“Deterministic programs. Call CLIs and SDK functions. Take inputs → produce outputs; no planning, no long-term memory.”",
        "role": "process",
        "lane": null,
        "group_id": "grp_main_stack",
        "position": { "x": 140, "y": 630 },
        "size": { "width": 1140, "height": 150 },
        "style": { "shape": "rectangle", "fill_color": "#FFFFFF", "border_color": "#111827" },
        "data": { "font_family": "sans-serif" }
      },
      {
        "id": "layer1",
        "label": "LAYER 1 — Terminal + Shell (Command Interface)\nLocation: Terminal application\nPurpose: Text-based command interpretation and I/O\nExamples:\n• Terminal.app\n• Bash / zsh\nDescription:\n“Terminal provides text-only window; shell interprets commands + launches tools via text I/O streams. No screen or GUI awareness.”",
        "role": "process",
        "lane": null,
        "group_id": "grp_main_stack",
        "position": { "x": 140, "y": 790 },
        "size": { "width": 1140, "height": 130 },
        "style": { "shape": "rectangle", "fill_color": "#FFFFFF", "border_color": "#111827" },
        "data": { "font_family": "sans-serif" }
      },
      {
        "id": "layer0",
        "label": "LAYER 0 — Operating System\nLocation: Physical machine\nPurpose: Execute real side effects\nExamples:\n• Linux / macOS Kernel\n• Filesystem\n• Network stack\nDescription:\n“Where irreversible actions happen: processes, files, network I/O.”",
        "role": "process",
        "lane": null,
        "group_id": "grp_main_stack",
        "position": { "x": 140, "y": 930 },
        "size": { "width": 1140, "height": 120 },
        "style": { "shape": "rectangle", "fill_color": "#FFFFFF", "border_color": "#111827" },
        "data": { "font_family": "sans-serif" }
      },
      {
        "id": "persist_header",
        "label": "LAYER 2.5 — Persistence & Traceability (Side Channel)",
        "role": "note",
        "lane": null,
        "group_id": "grp_persistence",
        "position": { "x": 1330, "y": 260 },
        "size": { "width": 450, "height": 50 },
        "style": { "shape": "rectangle", "fill_color": "#FFFFFF", "border_color": "#111827" },
        "data": { "font_family": "sans-serif", "text_align": "center" }
      },
      {
        "id": "persist_body",
        "label": "Location: Local + remote services\nPurpose: Store history and collaboration state\nExamples:\n• Git (local repository)\n• GitHub (remote repo, PRs, audit history)\nDescription:\n“Durable memory and traceability. Not execution. Not reasoning.”\n\nDesign note:\nShow this as a parallel vertical column connected to CLI and Agent layers with dashed lines.",
        "role": "process",
        "lane": null,
        "group_id": "grp_persistence",
        "position": { "x": 1330, "y": 315 },
        "size": { "width": 450, "height": 270 },
        "style": { "shape": "rectangle", "fill_color": "#FFFFFF", "border_color": "#111827" },
        "data": { "font_family": "sans-serif" }
      },
      {
        "id": "gui_layer",
        "label": "SEPARATE SIDE LAYER — Screen / GUI Environment\nLocation: Desktop environment\nPurpose: Visual user interface\nExamples:\n• Browser windows\n• IDEs\n• Desktop apps\nDescription:\n“Not visible to terminal or CLIs by default.”",
        "role": "process",
        "lane": null,
        "group_id": "grp_gui",
        "position": { "x": 1330, "y": 610 },
        "size": { "width": 450, "height": 240 },
        "style": { "shape": "rectangle", "fill_color": "#FFFFFF", "border_color": "#111827" },
        "data": { "font_family": "sans-serif" }
      },
      {
        "id": "tool_call_request_note",
        "label": "tool call request",
        "role": "note",
        "lane": null,
        "group_id": "grp_main_stack",
        "position": { "x": 980, "y": 560 },
        "size": { "width": 260, "height": 40 },
        "style": { "shape": "rectangle", "fill_color": "#FFFFFF", "border_color": "#111827" },
        "data": { "font_family": "sans-serif", "text_align": "center" }
      },
      {
        "id": "wrapper_approval_note",
        "label": "Wrapper/approval",
        "role": "note",
        "lane": null,
        "group_id": "grp_main_stack",
        "position": { "x": 980, "y": 510 },
        "size": { "width": 260, "height": 40 },
        "style": { "shape": "rectangle", "fill_color": "#FFFFFF", "border_color": "#F97316" },
        "data": { "font_family": "sans-serif", "text_align": "center" }
      },
  {
    "id": "footer_row1",
    "label": "“Terminals stream text I/O  -  CLIs and SDKs provide interfaces  -  LLMs reason  -  Agents orchestrate -  Wrappers gate execution (HumanLayer gates with human approval)  -  Git records what happened",
    "role": "note",
    "lane": null,
    "group_id": "grp_footer",
    "position": { "x": 140, "y": 1045 },
    "size": { "width": 1640, "height": 50 },
    "style": { "shape": "rectangle", "fill_color": "#FFFFFF", "border_color": "#FFFFFF" },
    "data": {
      "font_family": "sans-serif",
      "font_size_hint": "small",
      "text_align": "left",
      "text_runs": [
        { "text": "“" },
        { "text": "Terminals", "attributes": { "bold": true } },
        { "text": " stream text I/O  -  " },
        { "text": "CLIs", "attributes": { "bold": true } },
        { "text": " and " },
        { "text": "SDKs", "attributes": { "bold": true } },
        { "text": " provide interfaces  -  " },
        { "text": "LLMs", "attributes": { "bold": true } },
        { "text": " reason  -  " },
        { "text": "Agents", "attributes": { "bold": true } },
        { "text": " orchestrate -  " },
        { "text": "Wrappers", "attributes": { "bold": true } },
        { "text": " gate execution (HumanLayer gates with human approval)  -  " },
        { "text": "Git", "attributes": { "bold": true } },
        { "text": " records what happened" }
      ]
    }
  },
  {
    "id": "footer_row2",
    "label": "“Slash Commands: user-invoked shortcuts that start workflows  -  Agents autonomously orchestrate work  -  Hooks: automatic triggers on start/finish cycles and events”",
    "role": "note",
    "lane": null,
    "group_id": "grp_footer",
    "position": { "x": 140, "y": 1115 },
    "size": { "width": 1640, "height": 50 },
    "style": { "shape": "rectangle", "fill_color": "#FFFFFF", "border_color": "#FFFFFF" },
    "data": {
      "font_family": "sans-serif",
      "font_size_hint": "small",
      "text_align": "left",
      "text_runs": [
        { "text": "“" },
        { "text": "Slash Commands", "attributes": { "bold": true } },
        { "text": ": user-invoked shortcuts that start workflows  -  " },
        { "text": "Agents", "attributes": { "bold": true } },
        { "text": " autonomously orchestrate work  -  " },
        { "text": "Hooks", "attributes": { "bold": true } },
        { "text": ": automatic triggers on start/finish cycles and events”" }
      ]
    }
  }
    ],
    "edges": [
      { "id": "e_5_4", "from": "layer5", "to": "layer4", "label": "", "style": { "line_type": "straight", "arrowhead": "standard" } },
      { "id": "e_4_3", "from": "layer4", "to": "layer3", "label": "", "style": { "line_type": "straight", "arrowhead": "standard" } },
      { "id": "e_3_2", "from": "layer3", "to": "layer2", "label": "", "style": { "line_type": "straight", "arrowhead": "standard" } },
      { "id": "e_2_1", "from": "layer2", "to": "layer1", "label": "", "style": { "line_type": "straight", "arrowhead": "standard" } },
      { "id": "e_1_0", "from": "layer1", "to": "layer0", "label": "", "style": { "line_type": "straight", "arrowhead": "standard" } },
      { "id": "e_persist_to_layer2", "from": "persist_body", "to": "layer2", "label": "", "style": { "line_type": "dashed", "arrowhead": "none" } },
      { "id": "e_persist_to_layer4", "from": "persist_body", "to": "layer4", "label": "", "style": { "line_type": "dashed", "arrowhead": "none" } },
      { "id": "e_gui_to_layer1", "from": "gui_layer", "to": "layer1", "label": "screen visibility (dashed)", "style": { "line_type": "dashed", "arrowhead": "none" } },
      { "id": "e_gui_to_layer2", "from": "gui_layer", "to": "layer2", "label": "screen visibility (dashed)", "style": { "line_type": "dashed", "arrowhead": "none" } },
      { "id": "e_tool_request_4_to_3", "from": "layer4", "to": "wrapper_approval_note", "label": "", "style": { "line_type": "dashed", "arrowhead": "standard" } },
      { "id": "e_tool_request_3_to_2", "from": "wrapper_approval_note", "to": "layer2", "label": "", "style": { "line_type": "dashed", "arrowhead": "standard" } },
      { "id": "e_tool_request_label_anchor", "from": "tool_call_request_note", "to": "layer2", "label": "", "style": { "line_type": "none", "arrowhead": "none" } }
    ],
    "groups": [
      { "id": "grp_header", "label": "Header", "type": "container", "bounds": { "x": 120, "y": 20, "width": 1180, "height": 140 } },
      { "id": "grp_main_stack", "label": "Main Stack", "type": "swimlane", "bounds": { "x": 120, "y": 150, "width": 1180, "height": 910 } },
      { "id": "grp_persistence", "label": "Persistence & Traceability", "type": "swimlane", "bounds": { "x": 1310, "y": 240, "width": 490, "height": 360 } },
      { "id": "grp_gui", "label": "Screen / GUI Environment", "type": "swimlane", "bounds": { "x": 1310, "y": 595, "width": 490, "height": 270 } },
      { "id": "grp_footer", "label": "Footer", "type": "container", "bounds": { "x": 120, "y": 1035, "width": 1680, "height": 140 } }
    ],
    "constraints": {
      "layout_lock": true,
      "allow_auto_routing": true
    }
  }
}
