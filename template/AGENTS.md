# AGENTS.md

## Project Context
This is application built with net10.0.

## Stack
- net10.0

<!--#if (CodeRepositoryEngine != "No repository") -->
## Repository
Repository type: __CodeRepositoryEngine__

<!--#if (CodeRepositoryProjectName != "") -->
Project name: __CodeRepositoryProjectName__
<!--#endif -->
Repository name: __CodeRepositoryName__

## Branch Workflow

When the user asks for code or documentation changes that should be implemented on a branch, follow the workflow in `docs/workflows/agent-work-item-workflow.md`. If the user provides an work item ID, include the specific steps from that workflow.

## Git And Editing

- Keep changes scoped to the requested task.
- Do not rewrite unrelated files.
- Do not revert user changes unless explicitly asked.
- Prefer small, readable edits over broad refactors.
- Preserve existing style unless there is a clear reason to change it.
<!--#endif -->
