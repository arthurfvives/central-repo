# Central Agentic Workflows

Shared GitHub Agentic Workflows (gh-aw) live here. Install them with one command.

## 60-second setup

Run this in the target repository:

```sh
# One-time: install the CLI extension
gh extension install github/gh-aw

# Add one workflow from this repository
gh aw add arthurfvives/central-repo/<workflow-name>
```

Example:

```sh
gh aw add arthurfvives/central-repo/pr-documentation-review
```

After install, gh-aw writes:

```text
.github/workflows/<workflow-name>.md
.github/workflows/<workflow-name>.lock.yml
```

## Install by use case

Documentation review on PR open:

```sh
gh aw add arthurfvives/central-repo/pr-documentation-review
```

Manual test coverage review (`/test-review` or eyes reaction):

```sh
gh aw add arthurfvives/central-repo/test-review
```

React useEffect policy review on PR changes:

```sh
gh aw add arthurfvives/central-repo/react-useeffect-review
```

## Update workflows

Run this in a repository where workflows are installed:

```sh
# Update one
gh aw update <workflow-name>

# Update all installed workflows
gh aw update
```

## Available workflows

| Workflow | Trigger | What it does |
|---|---|---|
| `pr-documentation-review` | PR opened | Reviews documentation completeness and accuracy |
| `test-review` | `/test-review`, eyes reaction, workflow_call | Reviews test coverage and test quality |
| `react-useeffect-review` | PR opened/synchronize/reopened | Reviews `useEffect` usage against Delaware rules |

## Authoring new workflows (maintainers)

1. Add a new markdown workflow file under `workflows/`.
2. Commit and push.
3. Consumers install with `gh aw add arthurfvives/central-repo/<workflow-name>`.

## Repository layout

```text
central-repo/
└── workflows/
    ├── pr-documentation-review.md
    ├── test-review.md
    └── react-useeffect-review.md
```


