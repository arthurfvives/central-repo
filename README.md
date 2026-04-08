# Central Agentic Workflows Repository

This repository is the **single source of truth** for all shared GitHub Agentic Workflows (gh-aw). Projects opt in to specific workflows via a one-line config change — **no workflow files are ever copied**.

## How it works

Instead of copying the compiled `*.lock.yml` files (1000+ lines each) to every project, the workflows **stay here permanently**. Each project only has a tiny trigger file (~10 lines) that calls the central workflow via GitHub Actions' native `workflow_call`.

```
central-repo/                           ← You are here (arthurfvives/central-repo)
├── catalog.yml                         ← Trigger definitions per workflow
├── workflows/
│   └── sync-agentic-workflows.yml      ← Template for project repos
└── .github/
    └── workflows/
        ├── pr-documentation-review.md        ← Source (edit this)
        ├── pr-documentation-review.lock.yml  ← Compiled (callable via workflow_call)
        ├── daily-repo-status.md
        └── daily-repo-status.lock.yml

project-repo/
└── .github/
    ├── aw-config.yml                   ← Opt-in config (only file you edit)
    └── workflows/
        ├── sync-agentic-workflows.yml  ← Generates/removes trigger files
        └── pr-documentation-review.yml ← AUTO-GENERATED tiny trigger (~10 lines)
```

When a project enables a workflow in `aw-config.yml`, the `sync-agentic-workflows` workflow creates a trigger file like this:

```yaml
name: "Reviews codebase documentation..."

"on":
  pull_request:
    types: [opened]

jobs:
  run:
    uses: arthurfvives/central-repo/.github/workflows/pr-documentation-review.lock.yml@main
    secrets: inherit
```

That is the **entire file** — the actual workflow logic runs in this central repo.

## Adding or updating a workflow

1. Edit (or create) the `.md` source in `.github/workflows/`
2. Compile: `gh aw compile` (generates the `.lock.yml`)
3. Register the trigger in `catalog.yml`
4. Commit — all projects pick up the change on their next sync (daily or manual)

> **Note:** The `workflow_call:` trigger was added manually to the existing lock files.
> After running `gh aw compile` in this repo, the lock files will be fully regenerated and the metadata hash will be correct.

## Project setup

In any project that wants to use centralized workflows:

### 1. Create `.github/aw-config.yml`

```yaml
central-repo: arthurfvives/central-repo
central-repo-ref: main

workflows:
  pr-documentation-review: true
  daily-repo-status: false
```

### 2. Copy `workflows/sync-agentic-workflows.yml` into `.github/workflows/`

See [sync-agentic-workflows.yml](workflows/sync-agentic-workflows.yml) for the template.

### 3. Push — trigger files are generated automatically

Run the sync manually the first time via `workflow_dispatch` or just push `aw-config.yml`.

### Requirements

- `central-repo` must be **public** or **internal** within your organisation (GitHub Actions `workflow_call` requirement).
- Each project needs its own `COPILOT_GITHUB_TOKEN` secret — billing goes to the calling repo's token.
- If `central-repo` is private: grant Actions access under _Settings → Actions → General → Access_ and set the `CENTRAL_REPO_TOKEN` secret in the project.

## Available workflows

| Name | Description |
|------|-------------|
| `pr-documentation-review` | Reviews documentation quality on every opened PR |
| `daily-repo-status` | Posts a daily GitHub Issue summarising repository activity |

