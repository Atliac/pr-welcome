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
  post-guidance:
    runs-on: ubuntu-latest
    steps:
      - name: Post guidance comment
        uses: Atliac/pr-welcome@v1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

### With Custom Message

```yaml
      - name: Post guidance comment
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

## Releases & Versioning

This action follows [Semantic Versioning](https://semver.org/).

When referencing the action in your workflows, you can pin to a major version tag (like `@v1`) to automatically receive backward-compatible updates:

- **Major Tag (e.g. `@v1`)**: Automatically updated to point to the latest minor/patch release of that version.
- **Specific Version Tag (e.g. `@v1.0.0`)**: Recommended if you require absolute immutability and want to pin to a exact release.
