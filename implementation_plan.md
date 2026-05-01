# VectorShift Frontend Assessment Implementation Plan

This plan addresses the four parts of the technical assessment: Node Abstraction, Styling, Text Node Logic, and Backend Integration.

## User Review Required

Please review the proposed node abstractions and styling direction before I proceed.

## Proposed Changes

### 1. Node Abstraction (Frontend)
I will create a `BaseNode` abstraction to manage common node structures, styles, handles, and headers.
- **[NEW]** `frontend/src/nodes/BaseNode.js`: A reusable wrapper for nodes.
- **[MODIFY]** `frontend/src/nodes/inputNode.js`: Refactor to use `BaseNode`.
- **[MODIFY]** `frontend/src/nodes/outputNode.js`: Refactor to use `BaseNode`.
- **[MODIFY]** `frontend/src/nodes/textNode.js`: Refactor to use `BaseNode`.
- **[MODIFY]** `frontend/src/nodes/llmNode.js`: Refactor to use `BaseNode`.
- **[NEW]** 5 new node types demonstrating the abstraction flexibility:
  - `NoteNode.js`: Simple node for display-only text/notes.
  - `TransformNode.js`: Node to handle functional transformation configurations.
  - `DataNode.js`: Node for data source selection.
  - `FilterNode.js`: Node for setting filter parameters.
  - `ActionNode.js`: Node that triggers an external action/webhook.
- **[MODIFY]** `frontend/src/ui.js` and `frontend/src/toolbar.js`: Register and display the new nodes.

### 2. Styling (Frontend)
- **[MODIFY]** `frontend/src/index.css`: Introduce a comprehensive design system utilizing a modern, premium "glassmorphism" aesthetic with a sleek dark mode theme. It will feature gradients, custom fonts (Inter), rounded borders, subtle box shadows, and smooth micro-animations.
- **[MODIFY]** `frontend/src/App.js`, `frontend/src/toolbar.js`, `frontend/src/submit.js`: Apply the new CSS classes for a unified layout and appealing UI.

### 3. Text Node Logic (Frontend)
- **[MODIFY]** `frontend/src/nodes/textNode.js`: 
  - Switch the standard `<input>` to a `<textarea>` that auto-resizes its height as content expands.
  - Implement regex logic `/{{\s*([a-zA-Z_$][a-zA-Z0-9_$]*)\s*}}/g` to detect valid JavaScript variables within the text.
  - Dynamically render left-side `<Handle>` components corresponding to unique variables detected in the input.

### 4. Backend Integration
- **[MODIFY]** `backend/main.py`:
  - Update the `/pipelines/parse` endpoint to accept JSON data from the frontend containing `nodes` and `edges`.
  - Implement logic to compute `num_nodes` and `num_edges`.
  - Implement a Depth-First Search (DFS) algorithm to determine if the pipeline is a Directed Acyclic Graph (`is_dag`).
  - Configure CORS using `CORSMiddleware` so the frontend can communicate with the backend.
- **[MODIFY]** `frontend/src/submit.js`:
  - Fetch `nodes` and `edges` using `useStore`.
  - Send an HTTP POST request to the backend with the pipeline data upon clicking "Submit".
  - Display a styled modal or native `alert()` showing the response (`num_nodes`, `num_edges`, `is_dag`).

## Verification Plan

### Automated/Manual Testing
- Drag and drop each of the 9 total nodes from the toolbar to the canvas.
- Test `TextNode` auto-resizing by typing a long multi-line paragraph.
- Test `TextNode` dynamic handles by typing `{{ var1 }}` and `{{ var2 }}` and ensuring corresponding handles appear immediately.
- Connect various nodes and click the `Submit` button.
- Verify the alert pops up with accurate node/edge counts and correctly identifies if a cycle exists (is_dag boolean).
