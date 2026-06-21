# PR Welcome Comment Action 👋

Automatically post a welcome and guidance comment on pull requests. This action helps maintainers encourage first-time contributors and guides reviewers on how to inspect, run, and review changes.

## Features

- **Automated Welcomes**: Greets pull request authors with next steps and review instructions.
- **Customizable Message**:
  - `custom-message`: Provide your own markdown message. Supports `{{prNumber}}` and `{{baseBranch}}` placeholders.

---

## Usage

Create a workflow file in your repository at `.github/workflows/pr-welcome.yml`:

### Default Configuration

```yaml
name: PR Welcome

on:
  pull_request_target:
    types: [opened]

# Required permissions for reading and writing PR comments
permissions:
  issues: write
  pull-requests: write

jobs:
  pr-welcome:
    runs-on: ubuntu-latest
    steps:
      - name: PR Welcome
        uses: Atliac/pr-welcome@v1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

### With Custom Message

```yaml
      - name: PR Welcome
        uses: Atliac/pr-welcome@v1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          custom-message: |
            ## Hello and Welcome!

            Thanks for opening PR #{{prNumber}} pointing to branch `{{baseBranch}}`.

            Please review our contributing guidelines!
```

---

## Inputs

| Input | Description | Required | Default |
| :--- | :--- | :--- | :--- |
| `github-token` | The GitHub token to use for API requests. | **Yes** | — |
| `custom-message` | An optional custom Markdown message to post. You can use `{{prNumber}}` and `{{baseBranch}}` as placeholders. | No | `''` (uses default guidance template) |

---

## Permissions

The action requires `issues: write` and `pull-requests: write` permissions to post comments on pull requests:

```yaml
permissions:
  issues: write
  pull-requests: write
```

---

## Why `pull_request_target`?

This action uses the [`pull_request_target`](https://docs.github.com/en/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows#pull_request_target) event instead of `pull_request`. Here's why:

| Event | Runs in context of | Token permissions | Works for fork PRs? |
| :--- | :--- | :--- | :--- |
| `pull_request` | The **PR's (fork's)** repository | Read-only (for public repos) | ❌ Will get 403 when posting comments |
| `pull_request_target` | The **base** repository | Full permissions as defined in the workflow | ✅ Works as expected |

> [!IMPORTANT]
> Because `pull_request_target` runs workflows from the **base branch** (e.g. `main`), any changes to `.github/workflows/pr-welcome.yml` in a PR will **not** take effect for that PR. The workflow always executes the version that already exists on the default branch. If you modify the workflow file in a PR and expect the new behavior to apply immediately, it won't — the changes only activate after the PR is merged.

---

## Releases & Versioning

This action follows [Semantic Versioning](https://semver.org/).

When referencing the action in your workflows, you can pin to a major version tag (like `@v1`) to automatically receive backward-compatible updates:

- **Major Tag (e.g. `@v1`)**: Automatically updated to point to the latest minor/patch release of that version.
- **Specific Version Tag (e.g. `@v1.0.0`)**: Recommended if you require absolute immutability and want to pin to a exact release.
