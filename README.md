# .github

Shared GitHub workflows and profile configuration for [@russjhammond](https://github.com/russjhammond).

## Reusable Workflows

### `claude-runner.yml`

Centralized Claude auto-implement workflow used by repos across the `sure-shot` and `Brunson-Industries` orgs.

**Usage** — add a thin caller to any repo at `.github/workflows/claude.yml`:

```yaml
name: Claude Auto-Implement
on:
  issues:
    types: [labeled]
  issue_comment:
    types: [created]
  pull_request_review_comment:
    types: [created]
  pull_request_review:
    types: [submitted]

concurrency:
  group: claude-${{ github.event.issue.number || github.event.pull_request.number }}
  cancel-in-progress: false

jobs:
  claude-runner:
    uses: russjhammond/.github/.github/workflows/claude-runner.yml@main
    secrets: inherit
    permissions:
      contents: write
      pull-requests: write
      issues: write
      id-token: write
      actions: read
```

**Requirements:**
- `ANTHROPIC_API_KEY` org secret (visibility: all repos)
- Self-hosted runner with `[self-hosted, claude]` labels
- Model picker labels: `claude-haiku`, `claude-sonnet`, `claude-opus`

**Note:** Callers use `secrets: inherit` (not explicit secret mapping) for cross-org compatibility.
