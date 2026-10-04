# Project Pulse Dashboard Implementation Plan

## Summary and repository findings

Build Mona’s Project Pulse as a lightweight static dashboard for contributors to review project ownership, status, recent activity, and priority at a glance. The dashboard must show multiple project cards, a short contributor-friendly summary for each project, and a polished, accessible, responsive layout.

The repository’s source of truth is `.github/project-pulse-brief.md`; `.github/steps/3-step.md` and `.github/workflows/3-step.yml` specify the implementation and acceptance checks. The `app/` directory currently has no files, and `.vscode/launch.json` does not exist. There is no app framework, package manifest, or test suite to extend, so use plain HTML, CSS, JSON, and browser-native JavaScript without adding dependencies. Existing `.vscode/tasks.json` is unrelated and should remain unchanged.

The custom agent definitions in `.github/agents/` establish the working boundaries: Orchestrator coordinates and integrates; Planner plans but does not implement; Designer advises on UX and visual direction; Coder implements assigned files and can create a launch configuration when assigned.

## Agent responsibilities

- **Orchestrator:** Use this plan to assign non-overlapping work, coordinate dependencies, and verify the integrated dashboard and launch experience.
- **Planner:** Produce the implementation plan and identify ownership, dependencies, sequencing, risks, and validation. The Planner does not implement application code.
- **Designer:** Define the visual hierarchy, card and status-badge treatment, responsive behavior, accessibility requirements, and CSS/HTML hook expectations. The Designer should provide guidance to the Orchestrator and Coder, not edit deliverable files, to avoid conflicting ownership.
- **Coder:** Implement all four dashboard deliverables in the assigned files, using the Designer’s guidance; handle data loading and visible error states; validate the runnable result.

## Ordered implementation steps and file assignments

1. **Plan and confirm the contract — Planner**
   - **File:** `docs/project-pulse-plan.md`
   - Record the dashboard goal, agent roles, assignments, dependencies, parallel work decisions, edge cases, and validation expectations. Confirm the required data keys and launch behavior against `.github/project-pulse-brief.md` and `.github/steps/3-step.md`.

2. **Define the experience and prepare independent deliverables — Designer and Coder**
   - **Designer:** Provide layout, accessibility, responsive, and selector guidance; no file ownership.
   - **Coder — `app/project-data.json`:** Create a top-level `projects` array with multiple representative records. Every record must include `name`, `owner`, `status`, `recentActivity`, and `priority`. Include a short contributor-friendly `summary` per record to meet the brief.
   - **Coder — `.vscode/launch.json`:** Create strict JSON with a configuration named **Run Project Pulse Dashboard**. Use `python3 -m http.server 5500`, set `cwd` to `${workspaceFolder}/app`, and configure `serverReadyAction` to open `http://localhost:%s/index.html`. The launch must open the dashboard page rather than the app directory listing.

3. **Implement and integrate the dashboard — Coder**
   - **Files:** `app/index.html`, `app/styles.css`
   - In `index.html`, set the page title to **Project Pulse**, link `styles.css`, load `project-data.json`, and render a `.project-card` for each project from the JSON data. Display the project name, owner, status, recent activity, priority, and summary.
   - In `styles.css`, implement the Designer’s visual guidance, including `.dashboard` and `.project-card` selectors, readable spacing and typography, visible status/priority treatment, `border-radius`, `box-shadow`, and responsive layout.
   - Keep data rendering robust and accessible: use semantic structure and clear labels, avoid inserting project values as HTML, and show a useful visible message if data loading fails.

4. **Integrate and verify — Orchestrator with Coder**
   - Confirm all deliverables agree on filenames, data keys, CSS hooks, and launch behavior. Resolve issues within the assigned files only; do not change unrelated exercise configuration.

## Dependencies and parallel work

The Planner’s contract comes first. After it is available, the Designer’s design guidance and the Coder’s work on `app/project-data.json` and `.vscode/launch.json` can run **in parallel**: these have distinct scopes and do not depend on one another. The Coder’s `index.html` and `styles.css` implementation must wait for the Designer’s guidance and the data schema so markup, styling hooks, and rendering agree. Final runtime verification must wait until all four deliverables are present.

## Edge cases and risks

- Fetching `project-data.json` from a `file://` URL may fail in browsers; use the provided local HTTP launch configuration for preview.
- A failed or malformed data response must produce a visible error state rather than an empty or success-shaped dashboard.
- Ensure every project record has all required values and that status or priority differences remain understandable without color alone.
- Keep launch JSON comment-free and valid; a server starting successfully is not sufficient unless the browser opens `index.html`.
- The repository’s workflow checks required paths and specific text/JSON properties but do not prove accessibility, responsive behavior, or successful runtime rendering.

## Validation expectations

- Check that all four assigned files exist.
- Parse `app/project-data.json` and `.vscode/launch.json` as JSON, for example with `python3 -m json.tool`.
- Confirm the HTML contains the exact title **Project Pulse**, references `styles.css` and `project-data.json`, and renders `.project-card` elements with `status`, `recentActivity`, and `priority`.
- Confirm the JSON has a top-level `projects` array and each record includes `name`, `owner`, `status`, `recentActivity`, `priority`, and the planned `summary`.
- Confirm the stylesheet includes `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`, and inspect the layout at narrow and wide viewport sizes.
- Confirm the launch file is strict JSON, names the configuration **Run Project Pulse Dashboard**, runs `python3 -m http.server 5500` from `${workspaceFolder}/app`, and opens `http://localhost:%s/index.html`.
- Run the launch configuration and verify the browser displays project cards, not a directory listing; check that data appears and that a loading error is visible if the data request fails.

## Open questions

None block implementation. Use clearly representative sample project data unless Mona’s team provides authoritative project details; the dashboard is static and does not imply live data.
