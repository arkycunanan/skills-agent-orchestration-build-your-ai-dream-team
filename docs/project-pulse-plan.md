# Project Pulse Implementation Plan

## 1. Summary
Project Pulse is a lightweight static dashboard for contributors to Mona's team. It is being built to give contributors a fast, readable view of active projects, ownership, status, recent activity, priority or risk, and a short summary through a polished card-based UI. The plan also supports the learning goal of practicing agent orchestration in GitHub Copilot CLI, with clear separation between planning, design, coding, and validation responsibilities.

## 2. Ordered Implementation Steps (numbered phases)
### Phase 1 – Data
**Owner:** Coder  
**Deliverable:** `app/project-data.json`

1. Create `app/project-data.json` first so the dashboard has a stable data contract before UI rendering is finalized.
2. Use a top-level `"projects"` key.
3. Include at least 3 sample project objects.
4. Ensure every project includes `name`, `owner`, `status`, `recentActivity`, and `priority`.
5. Keep the JSON valid and deterministic so `app/index.html` can safely consume or reference it.

### Phase 2 – Parallel
**Owners:** Designer + Coder  
**Deliverables:** `app/styles.css` and `app/index.html`

1. **Designer** creates `app/styles.css` with the dashboard visual system.
2. **Coder** creates `app/index.html` using the data contract established in Phase 1.
3. These scopes do not overlap, so they can run in parallel once the JSON structure is known.
4. `app/index.html` should reference `styles.css` and `project-data.json`, use semantic HTML, and render visible project cards.
5. `app/styles.css` should provide the polished dashboard styling, responsive layout behavior, readable spacing, and accessible visual hierarchy needed by the markup.

### Phase 3 – Launch config
**Owner:** Coder  
**Deliverable:** `.vscode/launch.json`

1. Create `.vscode/launch.json` after the app files exist so the launch target is concrete and verifiable.
2. Use strict JSON with no comments.
3. Add a configuration named `"Run Project Pulse Dashboard"`.
4. Set `cwd` to `${workspaceFolder}/app`.
5. Configure the launch behavior so it opens `index.html` rather than a directory listing.
6. Align the launch behavior with the repository guidance for serving the static app from the `app/` directory.

### Phase 4 – Validation
**Owners:** Orchestrator-led validation across outputs  
**Deliverable:** verified plan compliance

1. Confirm each required file exists in the assigned location.
2. Check that required selectors, class names, JSON keys, and launch configuration values are present.
3. Verify the files fit together: the HTML references the stylesheet and data file, the CSS styles the required dashboard hooks, the JSON parses, and the launch config opens the dashboard entry point.
4. Record any remaining risks or ambiguities for handoff.

## 3. File Assignments
- `app/index.html` → **Coder**: Build the semantic dashboard markup and render project cards using the agreed data shape and CSS hooks.
- `app/styles.css` → **Designer**: Define the polished visual system, responsive layout, status treatments, and dashboard card styling.
- `app/project-data.json` → **Coder**: Provide valid sample project data under a top-level `projects` array for the dashboard.
- `.vscode/launch.json` → **Coder**: Add the runnable VS Code launch configuration that serves `app/` and opens the dashboard.

## 4. Dependencies Between Steps
- **Phase 1 -> Phase 2:** `app/project-data.json` should come first because `app/index.html` depends on the project schema and sample content. Defining the data contract early avoids rework in the markup. `app/styles.css` is independent of the JSON file's exact contents, but Phase 2 as a whole should start once the dashboard structure and required fields are clear.
- **Within Phase 2:** Designer's `app/styles.css` and Coder's `app/index.html` can proceed at the same time because they are separate files with non-overlapping ownership. Coordination is limited to the agreed class hooks such as `.dashboard` and `.project-card`.
- **Phase 2 -> Phase 3:** `.vscode/launch.json` should follow the app file creation so the launch configuration can point at the final `app/index.html` entry point and use the correct working directory.
- **Phase 3 -> Phase 4:** Validation must happen after all files are in place so the integrated dashboard, data contract, and launch setup can be checked together.

## 5. Parallel Work Decisions
### Tasks that CAN run in parallel
- **Designer on `app/styles.css` + Coder on `app/index.html`** can run in parallel after the required CSS hooks and data shape are agreed, because they modify different files and only share interface-level conventions.
- **Designer on `app/styles.css` + Coder on `app/project-data.json`** can also overlap because styling does not depend on the exact sample data values, only on the dashboard structure and states that need visual treatment.
- Taken together, the practical parallel window is **Designer on `app/styles.css` while Coder completes `app/project-data.json` and then `app/index.html`**, since these are non-overlapping scopes with manageable coordination.

### Tasks that MUST be sequential
- **`app/project-data.json` before finalizing `app/index.html`:** the HTML needs a stable field set (`name`, `owner`, `status`, `recentActivity`, `priority`) and sample records to render correctly.
- **App files before `.vscode/launch.json`:** the launch configuration should be created after the app entry point and directory structure are confirmed.
- **All implementation before validation:** validation depends on the completed set of deliverables and their integration.

## 6. Designer Responsibilities
The Designer owns `app/styles.css` and must produce a polished, accessible dashboard stylesheet that includes:

- A required `.dashboard` selector that defines the main layout container.
- A required `.project-card` selector that gives each card visible structure.
- `border-radius` on project cards to create clear, modern card affordances.
- `box-shadow` on project cards to separate cards from the page background.
- Responsive layout behavior so the dashboard remains readable across narrower screens and wider desktop layouts.
- Status badge styling so project states are immediately scannable.
- Priority treatment that makes urgent or high-risk projects stand out without reducing readability.
- Readable spacing, typography, and information hierarchy so contributors can quickly scan card content.
- Accessible color contrast and spacing decisions that support clear comprehension.

## 7. Coder Responsibilities
The Coder owns `app/project-data.json`, `app/index.html`, and `.vscode/launch.json` and must produce:

### `app/index.html`
- Semantic HTML for a static Project Pulse dashboard.
- Markup that uses the `.dashboard` and `.project-card` classes.
- Rendering for visible project cards based on the project data.
- Presentation of project name, owner, status, recent activity, and priority in the card UI.
- References to `styles.css` and `project-data.json` so the dashboard is styled and connected to the defined data source.

### `app/project-data.json`
- Strictly valid JSON.
- A top-level `"projects"` key.
- At least 3 sample projects.
- For every project: `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Data values that are realistic enough to demonstrate the dashboard layout and status/priority treatments.

### `.vscode/launch.json`
- Strict JSON with no comments.
- A configuration named `"Run Project Pulse Dashboard"`.
- `cwd` set to `${workspaceFolder}/app`.
- Launch behavior that opens `index.html` instead of a directory listing.
- A configuration structure compatible with previewing the static dashboard from the app directory.

## 8. Edge Cases
- **Accessibility:** status and priority should not rely on color alone; the markup and styling should preserve readable text labels, good contrast, and keyboard-safe structure.
- **Missing data fields:** if any project field is absent or empty, the UI should fail gracefully with a sensible placeholder or omission strategy rather than breaking layout.
- **Responsive breakpoints:** cards should remain readable on small screens, avoid cramped metadata rows, and stack cleanly when horizontal space is limited.
- **JSON validity:** malformed JSON would break data loading or referencing, so strict syntax validation is required.
- **Empty state:** if the `projects` array is empty, the dashboard should still show a useful empty-state message or container rather than appearing broken.

## 9. Validation Expectations
A passing result should satisfy all of the following:

- `app/styles.css`: contains `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`.
- `app/index.html`: contains the `project-card` class in the markup.
- `app/project-data.json`: contains a top-level `"projects"` array, with objects that include `name`, `owner`, `status`, `recentActivity`, and `priority`.
- `.vscode/launch.json`: is valid JSON, includes the config name `"Run Project Pulse Dashboard"`, references `index.html`, and sets `cwd` to `${workspaceFolder}/app`.
- Integrated behavior expectation: the HTML, CSS, JSON, and launch configuration agree on the dashboard structure and open the Project Pulse frontend rather than a directory listing.

## 10. Open Questions
- The brief mentions a contributor-friendly summary, but the required JSON schema does not include a `summary` field. The team should decide whether summary text is out of scope for this exercise or should be derived from existing fields.
- The repository step guidance suggests serving the app with `python3 -m http.server 5500` and opening `http://localhost:%s/index.html`; if exact launch mechanics matter, the implementation should follow that convention explicitly.
- The brief does not define exact status values or priority levels, so the Coder should choose a small, consistent sample set that the Designer can style predictably.
- It is not fully specified whether `app/index.html` should fetch JSON dynamically or reference it in another static-friendly way, so implementation should prefer the simplest approach that still demonstrates the required data relationship.
