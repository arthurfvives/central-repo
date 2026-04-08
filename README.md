# Central Agentic Workflows Repository

This is the **single source of truth** for shared GitHub Agentic Workflows (gh-aw), modelled after [githubnext/agentics](https://github.com/githubnext/agentics).

Workflows are authored and maintained here. Project repos **install** them with a single CLI command — no scripts, no sync jobs, no secrets.

## Structure

```
central-repo/
└── .github/
    └── workflows/
        ├── pr-documentation-review.md        ← source (edit this)
        ├── pr-documentation-review.lock.yml  ← compiled (generated)
        ├── daily-repo-status.md
        └── daily-repo-status.lock.yml
```

## Installing a workflow in a project

```sh
gh extension install github/gh-aw
gh aw add arthurfvives/central-repo/pr-documentation-review
```

This copies two files into the project's `.github/workflows/`:

```
.github/workflows/
├── pr-documentation-review.md       ← links back to this repo via source:
└── pr-documentation-review.lock.yml
```

That's it. No extra infrastructure needed.

## Updating installed workflows

```sh
# Update one workflow
gh aw update pr-documentation-review

# Update all workflows in the project
gh aw update
```

Updates are **explicit** — project owners decide when to adopt a new version.

## Adding a new workflow

1. Create `.github/workflows/<name>.md` with frontmatter + natural language instructions
2. Compile: `gh aw compile`
3. Commit both `.md` and `.lock.yml`
4. Projects install it with `gh aw add arthurfvives/central-repo/<name>`

## Available workflows

| Name | Description |
|------|-------------|
| `pr-documentation-review` | Reviews documentation quality on every opened PR |
| `daily-repo-status` | Posts a daily GitHub Issue summarising repository activity |


