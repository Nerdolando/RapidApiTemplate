# Agent Work Item Workflow

This workflow applies to agents working on the `RapidApi` repository itself.

## Repository Access

Use the configured __CodeRepositoryEngine__ MCP server as the primary interface for __CodeRepositoryEngine__ data and actions.

At the start of repository related work, check whether __CodeRepositoryEngine__ MCP tools are available in the current session. If they are available, use them before any fallback mechanism for:

- reading issue, work item, pull request, and repository metadata
- creating or updating pull requests, comments, labels, or issue state

Use local `git` for local repository state and source control operations, including `status`, `diff`, branch creation, commits, and push.

Use __CodeRepositoryEngine__ CLI or direct __CodeRepositoryEngine__ REST API calls only if __CodeRepositoryEngine__ MCP tools are unavailable or fail. If a fallback is used, mention the reason in the user-facing update or final answer.

## Starting Work

When the user provides a work item ID, use that item as the source of scope, title, and branch description.

Before changing files:

1. Check the current git status.
2. Do not discard, reset, overwrite, or clean up existing local changes unless the user explicitly asks for that.
3. If local changes would prevent switching branches safely, stop and ask the user how to proceed.

Then prepare the branch:

```powershell
git branch --show-current
git switch main
git pull
git switch -c <branch-name>
```

Skip `git switch main` if `git branch --show-current` already reports `main`.

If the repository uses a different remote default branch in the future, confirm with the user before changing this workflow.

## Branch Naming

Use one of these branch name formats.

When a work item ID is provided:

```text
{prefix}/{work-item-id}-{short-kebab-description}
```

When a work item ID is not provided:

```text
{prefix}/{short-kebab-description}
```

The `short-kebab-description` must be written in English, even when the work item title or user request is written in another language.

Keep the description short, specific, and action-oriented. Use lowercase kebab case.

## Branch Prefixes

Choose the prefix based on the work type:

- `features/` for new features
- `fixes/` for bugs
- `refactor/` for refactoring
- `maint/` for maintenance, tooling, dependency updates, or other repository work

Examples:

```text
features/123-remember-theme
features/remember-theme
fixes/456-template-source-name
maint/update-template-verification
```

## Work Item Scope

If a work item ID is provided, use the connected project context when available.

The agent should identify:

- the work item title
- the requested behavior or outcome
- acceptance criteria, if present
- whether the work is a feature, bug fix, refactor, or maintenance task

If project details are not available through __CodeRepositoryEngine__ MCP in the current session, use the fallback order from the Repository Access section. If no tool can retrieve the project details, continue from the user's description and state that limitation.

## Verification

For template changes, run the verification steps documented in the root `AGENTS.md`.
