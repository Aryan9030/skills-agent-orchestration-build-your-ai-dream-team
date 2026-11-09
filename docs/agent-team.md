# Agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate this team as we build
Mona's Project Pulse dashboard.

| Agent | Model | Responsibility | Definition |
| --- | --- | --- | --- |
| Orchestrator | Claude Opus 5.5 (copilot) | Coordinates the specialists, breaks the work into scoped phases, manages dependencies, integrates their work, and reports the outcome. It does not implement the dashboard itself. | `.github/agents/orchestrator.agent.md` |
| Planner | Claude Opus 5.5 (copilot) | Researches the repository and requirements, then creates an implementation plan with steps, file assignments, dependencies, risks, edge cases, and validation expectations. | `.github/agents/planner.agent.md` |
| Coder | GPT-5.5 (copilot) | Implements the dashboard functionality and assigned runnable-app support, following the repository patterns and validating the changes. | `.github/agents/coder.agent.md` |
| Designer | Gemini 3.7 Flash (copilot) | Shapes the dashboard's user experience, information hierarchy, accessibility, responsive behavior, and visual design. | `.github/agents/designer.agent.md` |

## How the team works together

The Orchestrator first asks the Planner to research Project Pulse and propose
an implementation plan. It then assigns explicit file scopes and dependencies
to the Designer and Coder. They can work in parallel when their files and tasks
do not overlap; dependent or overlapping work happens sequentially. The
Orchestrator integrates the results, checks that the dashboard works as a
coherent whole, and reports the outcome.
