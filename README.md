# Central Agentic Workflows Repository

This repository is the **single source of truth** for all shared GitHub Agentic Workflows (gh-aw). Projects opt in to specific workflows via a configuration file — no manual copy-pasting of complex YAML files.

## How it works

Note: gh-aw itself expects executable workflow pairs in `.github/workflows` (`*.md` + `*.lock.yml`).
This repository is used as a central catalog/storage location, while project repositories sync selected workflows into their own `.github/workflows`.

```
central-repo/                        ← You are here
├── catalog.yml                      ← Master list of available workflows
└── workflows/
    ├── sync-agentic-workflows.yml            ← Reusable sync workflow template
    ├── pr-documentation-review/
    │   ├── pr-documentation-review.md        ← Source definition (edit this)
    │   └── pr-documentation-review.lock.yml  ← Compiled workflow (generated)
    └── daily-repo-status/
        ├── daily-repo-status.md
        └── daily-repo-status.lock.yml

project-repo/
└── .github/
    ├── aw-config.yml                ← Opt-in config (edit this per project)
    └── workflows/
        ├── sync-agentic-workflows.yml      ← Fetches from central-repo
        └── pr-documentation-review.lock.yml ← Auto-managed, do not edit
```

When a project triggers `sync-agentic-workflows` (on `.github/aw-config.yml` changes, daily schedule, or manual dispatch), it:
1. Reads which workflows are enabled/disabled
2. Downloads the compiled `.lock.yml` from this repository
3. Commits the changes — the workflow is now active (or removed) in that project

## Adding or updating a workflow

1. Edit (or create) the `.md` source definition under `workflows/`
2. Compile it from your project: `gh aw compile` (then copy the output here), or compile directly if the `gh aw` CLI is set up for this repo
3. Commit both the `.md` and the updated `.lock.yml`
4. Projects will receive the update the next time their sync workflow runs

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

### 2. Copy `workflows/sync-agentic-workflows.yml` from this repo

See [sync-agentic-workflows.yml](workflows/sync-agentic-workflows.yml) for the template.

### 3. Push — the sync workflow runs automatically on config changes

You can also run it manually via `workflow_dispatch`; the included template also has a daily scheduled sync.

## Available workflows

| Name | Description |
|------|-------------|
| `pr-documentation-review` | Reviews documentation quality on every opened PR |
| `daily-repo-status` | Posts a daily GitHub Issue summarising repository activity |
