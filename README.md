# PR Welcome Comment Action 👋

Automatically post a welcome and guidance comment on pull requests. This action helps maintainers encourage first-time contributors and guides reviewers on how to inspect, run, and review changes.

## Features

- **Automated Welcomes**: Greets pull request authors with next steps and review instructions.
- **Customizable Message**:
  - `custom-message`: Provide your own markdown message. Supports `{{prNumber}}` and `{{baseBranch}}` placeholders.
- **Duplicate Prevention**: Detects if the action signature already exists in the comments to avoid duplicate posts.

---

## Usage

Create a workflow file in your repository at `.github/workflows/pr-welcome.yml`:

### Default Configuration

```yaml
name: PR Guidance Comment

on:
  pull_request:
    types: [opened]

# Required permissions for reading and writing comments
permissions:
  issues: write

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
| `github-token` | The GitHub token to use for API requests. | No | `${{ github.token }}` |
| `custom-message` | An optional custom Markdown message to post. You can use `{{prNumber}}` and `{{baseBranch}}` as placeholders. | No | `''` (uses default guidance template) |

---

## Permissions

The action requires only `issues: write` permission to retrieve and post comments:

```yaml
permissions:
  issues: write
```
