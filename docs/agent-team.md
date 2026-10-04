# Agent team for Mona's Project Pulse dashboard

This project uses a custom multi-agent setup defined under `.github/agents/` and orchestrated through GitHub Copilot CLI in a Codespace. The team is designed to split planning, design, implementation, and coordination work while keeping responsibilities explicit and scoped.

## Team overview

- Orchestrator — Model: Claude Opus 4.7 (copilot)
  - Responsibility: Coordinates the overall workstream, breaks the dashboard build into phases, delegates tasks to specialist agents, and verifies that the final result fits together.
  - Definition: `.github/agents/orchestrator.agent.md`

- Planner — Model: Claude Opus 4.7 (copilot)
  - Responsibility: Researches the repo, reviews constraints and dependencies, and produces a practical implementation plan with file ownership, sequencing, edge cases, and validation expectations.
  - Definition: `.github/agents/planner.agent.md`

- Designer — Model: Gemini 3.1 Pro (copilot)
  - Responsibility: Owns the dashboard UX and visual design, including layout, accessibility, information hierarchy, interaction flow, and frontend polish for the Project Pulse experience.
  - Definition: `.github/agents/designer.agent.md`

- Coder — Model: GPT-5.5 (copilot)
  - Responsibility: Implements the actual application logic and UI code in the assigned files, validates behavior, and keeps the work aligned with project patterns and clear testable outcomes.
  - Definition: `.github/agents/coder.agent.md`

## How the team works together

The Orchestrator manages the team by first getting a plan from the Planner, then delegating design and implementation to the Designer and Coder in scoped phases. This keeps work parallel where possible while maintaining clear ownership and reducing overlap between agents. The whole workflow is run from GitHub Copilot CLI in the Codespace, which acts as the execution and coordination layer for the custom agent team.
