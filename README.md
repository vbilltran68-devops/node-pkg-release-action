# Node Package Release Action

A custom GitHub Action to handle the release process, including building, auditing, bumping version, and publishing for your package.

## Inputs

| Input             | Description                                 | Default  |
| ----------------- | ------------------------------------------- | -------- |
| `trigger-ref`     | Branch or tag that triggered the workflow   | Required |
| `github-token`    | Github Token                                | Required |
| `node-version`    | Node.js version to use                      | `lts/*`  |
| `package-manager` | Package manager to use (npm, yarn, or pnpm) | `npm`    |

## Outputs

| Output    | Description                       |
| --------- | --------------------------------- |
| `version` | The version number of the release |

## Example Usage

```yml
name: Release
on:
  workflow_run:
    workflows: ['Lint & Unit Test']
    branches: [main]
    types:
      - completed

permissions:
  contents: read

jobs:
  release:
    if: ${{ github.event.workflow_run.conclusion == 'success' }}
    runs-on: ubuntu-latest
    permissions:
      contents: write
      packages: write
    steps:
      - name: Node Package Release Action
        uses: vbilltran68-devops/node-pkg-release-action@v1
        with:
          trigger-ref: ${{ github.event.workflow_run.head_branch }}
          github-token: ${{ secrets.GITHUB_TOKEN }}
          package-manager: 'pnpm'
```
