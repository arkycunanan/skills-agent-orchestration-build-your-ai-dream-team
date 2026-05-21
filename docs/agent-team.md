# Project Pulse Agent Team

This document summarizes the custom agent team used to build Mona's Project Pulse dashboard.

## Agents

### Orchestrator
- **File:** `.github/agents/orchestrator.agent.md`
- **Model:** Claude Opus 4.7 (copilot)
- **Responsibility:** Coordinates the entire team. Breaks requests into phases, delegates tasks to specialist agents, enforces file-scope boundaries, and reports the final integrated result. Does not write code or design assets itself.

### Planner
- **File:** `.github/agents/planner.agent.md`
- **Model:** Claude Opus 4.7 (copilot)
- **Responsibility:** Researches the codebase and documentation, identifies dependencies and edge cases, and produces an ordered implementation plan with file assignments and parallel/sequential work decisions. Does not write code.

### Designer
- **File:** `.github/agents/designer.agent.md`
- **Model:** Gemini 3.1 Pro (copilot)
- **Responsibility:** Handles UI/UX direction, accessibility, information hierarchy, and visual design. For Project Pulse, creates a polished dashboard with project cards, status badges, responsive layout, and deterministic CSS hooks such as `.dashboard` and `.project-card`.

### Coder
- **File:** `.github/agents/coder.agent.md`
- **Model:** GPT-5.5 (copilot)
- **Responsibility:** Implements code within the file scope assigned by the Orchestrator. Writes `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json`. Validates changes before reporting completion.

## How the team works together

1. The **Orchestrator** receives a high-level request (e.g., "build the Project Pulse dashboard") and asks the **Planner** to produce a phased implementation plan.
2. The **Planner** researches the repository, identifies file ownership and dependencies, and returns an ordered plan with parallel/sequential work decisions.
3. The **Orchestrator** parses the plan into phases and delegates non-overlapping tasks in parallel where possible:
   - **Designer** handles `app/styles.css` — layout, card styling, color, and typography.
   - **Coder** handles `app/index.html`, `app/project-data.json`, and `.vscode/launch.json` — structure, data, and launch configuration.
4. Tasks that share file scope are kept in separate sequential phases to prevent conflicts.
5. The **Orchestrator** verifies the integrated result and surfaces any blockers before reporting the final outcome.
