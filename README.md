# PR Review Assignor

![GitHub release (latest by date)](https://img.shields.io/github/v/release/RayYeung1989/pr-review-assignor)
![GitHub](https://img.shields.io/github/license/RayYeung1989/pr-review-assignor)

Automatically assign reviewers to pull requests based on file changes.

## Features

- **Path-based assignment**: Assign reviewers based on file paths
- **Smart matching**: Supports wildcards and prefixes
- **Default reviewers**: Fallback reviewers when no match found
- **Deduplication**: Avoids assigning the same reviewer twice

## Usage

### Create workflow file

Create `.github/workflows/review-assignor.yml`:

```yaml
name: 'PR Review Assignor'

on:
  pull_request:
    types: [opened, synchronize]

jobs:
  assign:
    runs-on: ubuntu-latest
    steps:
      - name: 'Assign Reviewers'
        uses: RayYeung1989/pr-review-assignor@v1.0.0
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          default-reviewers: 'team-lead'
```

### Configuration

Create `.github/review-assign.yml` in your repository:

```yaml
# Frontend files
src/components: john,alice
src/pages: john

# Backend files
server/: bob
api/: charlie

# Config files
.github/: team-lead
*.yml: devops-team
```

### Inputs

| Input | Required | Description |
|-------|----------|-------------|
| `token` | Yes | GitHub token |
| `config-file` | No | Path to config file (default: `.github/review-assign.yml`) |
| `default-reviewers` | No | Comma-separated default reviewers |

### How it works

1. Gets the list of files changed in the PR
2. Matches each file against the config patterns
3. Collects unique reviewers
4. Assigns reviewers to the PR

## Example

With this config:
```
src/: alice,bob
tests/: charlie
*.md: doc-team
```

A PR changing `src/components/Button.tsx` and `tests/app.spec.ts` would assign: `alice`, `bob`, `charlie`

## License

MIT
