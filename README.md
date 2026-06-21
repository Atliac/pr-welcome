# PR Welcome Comment Action 👋

Automatically post a customizable welcome and guidance comment on pull requests. This action helps maintainers encourage first-time contributors and guides reviewers on how to inspect, run, and review changes.

## Features

- **Automated Welcomes**: Greets pull request authors with next steps and review instructions.
- **Configurable Skipping**:
  - `skip-draft-pr`: Skip posting comments while the PR is in a draft state.
  - `skip-collaborators`: Skip posting comments for repository owners, collaborators, or organization members.
- **Duplicate Prevention**: Detects if the action signature already exists in the comments to avoid duplicate posts.

---

## Usage

Create a workflow file in your repository at `.github/workflows/pr-welcome.yml`:

```yaml
name: PR Guidance Comment

on:
  pull_request:
    types: [opened, ready_for_review]

# Required permissions for reading and writing comments
permissions:
  pull-requests: write
  issues: write

jobs:
  post-guidance:
    runs-on: ubuntu-latest
    steps:
      - name: Post guidance comment
        uses: Atliac/pr-welcome@v1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
          skip-draft-pr: 'true'
          skip-collaborators: 'false'
```

---

## Inputs

| Input | Description | Required | Default |
| :--- | :--- | :--- | :--- |
| `github-token` | The GitHub token to use for API requests. | No | `${{ github.token }}` |
| `skip-draft-pr` | Set to `'true'` to skip posting comments on draft PRs. | No | `'false'` |
| `skip-collaborators` | Set to `'true'` to skip comments on PRs created by owners/collaborators/members. | No | `'false'` |

---

## Permissions

The action requires the following permissions to retrieve and post comments:

```yaml
permissions:
  pull-requests: write
  issues: write
```
