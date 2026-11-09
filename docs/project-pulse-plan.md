# Project Pulse Dashboard Implementation Plan

## Goal

Build a lightweight static Project Pulse dashboard so contributors can quickly see which projects are active, who owns them, each project's status, recent activity, and priority or risk. Present the information in a polished, accessible, responsive card layout with a concise contributor-friendly summary. Provide a VS Code launch configuration that serves the app and opens the dashboard page rather than a directory listing.

The project brief specifies a top-level `projects` array in `app/project-data.json`. Each project must contain `name`, `owner`, `status`, `recentActivity`, and `priority`. Keep the app static and limited to the assigned files; do not add frameworks, packages, or extra files without approval.

## Agent responsibilities

- **Planner** researches the repository and requirements, defines phases, file ownership, dependencies, risks, and validation expectations. This document is the planning deliverable.
- **Designer** owns visual and interaction direction, including hierarchy, responsive behavior, accessibility, and implementation in `app/styles.css`. Coordinate stable HTML/CSS hooks with the Coder; at minimum, style `.dashboard` and `.project-card`.
- **Coder** owns `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`; integrates the Designer's styling contract, renders project data, and validates the runnable app. Keep the launch configuration strict JSON.
- **Orchestrator** delegates with these file boundaries, resolves questions, integrates the work, and verifies the finished dashboard. Specialists should not stage, commit, or push changes.

## Implementation phases and file assignments

### Phase 1 — Agree on the UI and data contract

- **Owner:** Designer, with Coder consultation
- **Files:** No implementation files

Agree on card content and hierarchy, responsive behavior, accessible status and priority treatment, and stable class hooks before HTML integration. Use the required project fields from the brief. The brief also requests a contributor-friendly summary but does not define a required summary field; decide whether that summary is optional data or concise static UI copy.

- **Dependency:** This plan and the repository brief.
**Deliverable:** A concise design and data contract so the specialists can work without overlapping ownership.

### Phase 2 — Parallel, non-overlapping implementation

Start these tasks once the Phase 1 contract is clear:

| Owner | File assignment | Work |
| --- | --- | --- |
| Designer | `app/styles.css` | Implement the responsive dashboard styling, including `.dashboard` and `.project-card` hooks, clear spacing and typography, visible status badges, distinct priority/risk treatment, sufficient contrast, and keyboard focus styles. Do not edit Coder-owned files. |
| Coder | `app/project-data.json` | Add representative project data under a top-level `projects` array. Include `name`, `owner`, `status`, `recentActivity`, and `priority` for each entry, using values consistent with the agreed contract. |
| Coder | `.vscode/launch.json` | Add a strict-JSON **Run Project Pulse Dashboard** configuration. Follow the exercise requirements: serve `app/` with `python3 -m http.server 5500`, set working directory to `${workspaceFolder}/app`, and configure `serverReadyAction` to open `http://localhost:%s/index.html`. Verify that the selected VS Code debugger/runtime supports the configuration. |

**Dependencies:** Styles and data follow the Phase 1 contract. The launch configuration is otherwise independent of page markup; its working directory and URL must match the app path.

### Phase 3 — Build the page and connect the pieces

- **Owner:** Coder
**File assignment:** `app/index.html`

Use the exact page title **Project Pulse**. Reference `styles.css` and `project-data.json`, and render project cards from the data. Each card must show the project name, owner, status, recent activity, and priority. Use the agreed CSS hooks, semantic markup, and accessible text labels. If the page fetches JSON, provide visible loading, empty, and error states; do not leave a blank page when data cannot be loaded.

**Dependencies:** Phase 1's contract and Phase 2's stylesheet and data outputs.

### Phase 4 — Integrate and validate

- **Owners:** Coder validates; Orchestrator reviews integration
**File assignments:** Review all four assigned outputs and route any fixes to their file owners.

Run the app through **Run Project Pulse Dashboard**, inspect the rendered page, verify data-to-card mapping and responsive/accessibility behavior, and stop the preview server afterward. Resolve integration issues by returning them to the owning specialist rather than having agents edit the same file.

**Dependencies:** Phases 2 and 3.

## Dependencies and parallel work

- **Parallel:** After the shared contract is agreed, the Designer can implement `app/styles.css` while the Coder creates `app/project-data.json` and `.vscode/launch.json`. These tasks have separate file ownership. The launch configuration is independent of page markup, but its working directory and URL must match the planned app path.
- **Sequential:** Phase 1's contract precedes implementation. Coder integration in `app/index.html` follows the contract and depends on the agreed CSS hooks and JSON fields. Final integration and runtime validation follow all implementation tasks.
- **No overlapping ownership:** Designer edits only `app/styles.css`; Coder edits `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`. If design requires markup changes, Designer requests them from Coder rather than editing the HTML.
- **Orchestration:** The Orchestrator assigns these scopes and handles integration. Specialists do not stage, commit, or push changes.

## Risks and edge cases

- Missing, empty, or malformed `projects` data; a project missing a required field; or an empty projects array.
- Fetch failures when the page is opened with `file://`; use the configured local HTTP server and show a useful error state.
- Long project names, summaries, activity text, or owner names wrapping or overflowing on narrow screens.
- Unexpected status or priority values; keep labels understandable and do not rely on color alone.
- Poor contrast, keyboard-inaccessible content, or non-semantic card markup.
- Port `5500` conflicts, a missing Python executable, or launch behavior differing by VS Code debugger/runtime.
- Avoid inserting JSON-derived strings as unsafe HTML. Render project text as text.
- Python's `http.server` is appropriate for a local exercise preview, not production hosting.

## Validation expectations

### Repository and exercise checks

- Review `.github/workflows/2-step.yml` to confirm the plan addresses Project Pulse, Designer, Coder, all four assigned paths, dependencies, parallel work, and validation.
- Review `.github/workflows/3-step.yml` for implementation expectations. It checks files, required phrases/selectors and project fields, and parses `.vscode/launch.json` as JSON; it does not prove that the browser preview works.
- `scripts/validate-exercise.sh` is primarily a template-integrity check and expects learner answer files not to be tracked. Do not treat it as the dashboard's runtime test.

### App checks

- Confirm `app/project-data.json` parses as JSON, has a top-level `projects` array, and includes all required fields for each project.
- Confirm `app/index.html` uses the exact title, references the stylesheet and data file, and renders `.project-card` elements showing status, recent activity, and priority.
- Confirm `app/styles.css` includes `.dashboard`, `.project-card`, a responsive layout, readable spacing, visible status/priority treatments, and clear focus and contrast.
- Confirm `.vscode/launch.json` is strict JSON, names the configuration **Run Project Pulse Dashboard**, serves from `app/`, uses the required working directory and command, and opens `http://localhost:%s/index.html`.
- Start the configuration in VS Code and verify the browser shows the dashboard UI rather than a directory listing. Check multiple cards, narrow viewport behavior, and loading/error/empty states where applicable, then stop the server.

## Open questions

1. Should the contributor-friendly summary be a required project data field, optional field, or static supporting copy? The brief asks for a summary but does not list a summary key among required fields.
2. Which VS Code debugger/runtime configuration type is available in the Codespace for launching the Python server and applying `serverReadyAction`? The repository specifies the command and URL but contains no existing `launch.json` pattern.
3. Should project statuses and priorities use a fixed vocabulary, or should the UI gracefully display arbitrary values?
