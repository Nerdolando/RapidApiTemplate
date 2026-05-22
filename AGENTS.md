# Agent Instructions

This repository contains a `dotnet new` template, not a regular application repository.

The main rule is to keep repository-maintenance agent files separate from files that are meant to be emitted into a user's generated project.

## Repository Layout

- `template/` contains the project content for the `dotnet new` template.
- Files outside `template/` support development of this template repository.
- Root-level `AGENTS.md` and root-level `docs/workflows/agent-work-item-workflow.md` are for agents working on this repository.
- Generated-project agent files must be placed under `template/`, for example:
  - `template/AGENTS.md`
  - `template/docs/workflows/agent-work-item-workflow.md`

Do not add repository-maintenance notes, local workflow assumptions, or template-authoring instructions to files under `template/` unless they are intentionally meant to appear in the generated project.

## Template Work

When changing template output, edit files under `template/`.

When changing how this template repository is maintained, edit files outside `template/`.

If the same logical file exists both at the repository root and under `template/`, treat them as different files with different audiences:

- root file: instructions for maintaining `RapidApiTemplate`
- `template/` file: instructions for developers using a project generated from the template

## Work Item Workflow

When the user asks an agent to work on a specific item or start repository work from an item ID, follow:

```text
docs/workflows/agent-work-item-workflow.md
```

That workflow defines the GitHub repository context, the connected private project, branch preparation, and branch naming rules.

## GitHub Tooling

Use the configured GitHub MCP server first for GitHub repository, issue, pull request, and project data.

Before using GitHub CLI (`gh`) or direct GitHub REST API calls, check whether GitHub MCP tools are available in the current session. If GitHub MCP tools are available, use them for:

- reading work item, issue, and pull request details
- reading or updating GitHub Project item fields
- creating or updating pull requests, comments, labels, or issue state
- reading repository metadata that is not already available locally

Use local `git` commands for local repository state and source control operations such as status, diff, branch creation, commits, and push.

Fall back to `gh` or direct GitHub REST API only when GitHub MCP tools are unavailable or fail. When falling back, state the reason briefly so the user can distinguish a tool limitation from an agent choice.

## Dotnet New Configuration

The template configuration should live at:

```text
template/.template.config/template.json
```

When adding or changing template symbols, choices, conditions, sources, or exclusions, make sure the generated output still matches the intended project layout.

Agent-related files intended for generated projects should be controlled through `template.json` symbols when they are optional.

## Verification

Current baseline check, before `template.json` exists:

```powershell
dotnet build .\template\RapidApi\RapidApi.csproj
```

Once `template/.template.config/template.json` exists, verify template installation and output with a temporary directory:

```powershell
dotnet new install .\template
dotnet new rapidapi -o .\.tmp\generated -n SampleApi
dotnet build .\.tmp\generated\SampleApi\SampleApi.csproj
dotnet new uninstall .\template
```

Do not commit generated temporary output. Prefer `.tmp/` or another ignored temporary directory for local verification.

## Engineering Guidelines

- Follow the existing .NET project style unless the user asks for a structural change.
- Keep changes scoped to the template feature being worked on.
- Prefer simple, idiomatic ASP.NET Core code over speculative abstractions.
- Preserve `Nullable` and `ImplicitUsings` conventions unless there is a deliberate template-level reason to change them.
- When adding generated-project docs or agent instructions, write them for the developer who will consume the generated API project, not for the maintainer of this template repository.
