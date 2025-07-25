# Using GitHub the Proper Way

A comprehensive guide to collaborating on GitHub: branches, workflows, versioning, labels, CI/CD, and automation best practices.

## Table of Contents

1. [Introduction](#introduction)
2. [Branching Strategy](#branching-strategy)
   - [Production Branch (](#production-branch-master)[`master`](#production-branch-master)[)](#production-branch-master)
   - [Development Branch (](#development-branch-development)[`development`](#development-branch-development)[)](#development-branch-development)
   - [Topic Branches](#topic-branches)
3. [Workflow](#workflow)
4. [Versioning & Releases](#versioning--releases)
5. [Issue & PR Labels](#issue--pr-labels)
6. [CI/CD Workflows](#cicd-workflows)
7. [Automation & GitHub Actions](#automation--github-actions)
8. [Pull Request Guidelines](#pull-request-guidelines)
9. [Best Practices & Tips](#best-practices--tips)
10. [Contributing](#contributing)

---

## Introduction

GitHub provides a platform for teams of any size to collaborate effectively. This guide establishes a standardized workflow to:

- Keep history clean and conflicts minimal
- Enforce quality through code reviews and CI
- Automate releases and notifications

Use this README as the single source of truth for all repository contributors.

## Branching Strategy

Maintain two main long-lived branches:

### Production Branch (`master`)

- **Purpose**: Reflects the exact code running in production.
- **Protection Rules**:
  - No direct pushes
  - Require passing CI checks
  - Require ≥1 review approval
- **Releases**: Only merged into via pull requests from `development`. Each merge must:
  - Use `--no-ff` to preserve history
  - Be tagged with a SemVer version (`vMAJOR.MINOR.PATCH`)

### Development Branch (`development`)

- **Purpose**: Integration branch for all completed work.
- **Protection Rules**:
  - Require passing CI checks
  - Require ≥1 review approval
- **Merges**: From topic branches via pull requests.

### Topic Branches

All new work happens in short-lived, focused branches off `development`:

| Type          | Prefix     | Naming Example                   |
| ------------- | ---------- | -------------------------------- |
| Feature       | `feature/` | `feature/user-authentication`    |
| Bugfix        | `bugfix/`  | `bugfix/login-error-handling`    |
| Documentation | `docs/`    | `docs/branching-strategy-update` |
| Chore         | `chore/`   | `chore/dependencies-update`      |

**Create a topic branch**:

```bash
# Ensure up-to-date
git fetch origin
git checkout development
git pull origin development
# Create feature branch
git checkout -b feature/your-feature-name
```

## Workflow

1. **Sync**: Rebase or merge from `development` frequently.
   ```bash
   git fetch origin
   git checkout feature/your-feature
   git rebase origin/development
   ```
2. **Develop**: Make atomic commits with descriptive messages.
3. **Push**:
   ```bash
   git push -u origin feature/your-feature-name
   ```
4. **Open PR**:
   - Target: `development`
   - Provide description, issue link, and testing steps.
   - Assign reviewers.
5. **Review & Address**:
   - Respond to feedback in the same branch.
   - Rebase if `development` has advanced.
6. **Merge**:
   - Use **Squash and merge** or **Rebase and merge**.
   - Delete the merged branch.

## Versioning & Releases

Follow [Semantic Versioning](https://semver.org/):

- **PATCH** (`v1.2.3` → `v1.2.4`): Bug fixes only.
- **MINOR** (`v1.2.3` → `v1.3.0`): Backward-compatible features.
- **MAJOR** (`v1.2.3` → `v2.0.0`): Breaking changes.

**Release Steps**:

1. Confirm all desired work is merged into `development` and passes CI.
2. Update version in configuration (e.g., `package.json`).
3. Create a PR from `development` → `master` titled `chore(release): vX.Y.Z`.
4. Merge with `--no-ff`, tag `vX.Y.Z`, and push tags:
   ```bash
   git checkout master
   git merge --no-ff development
   git tag vX.Y.Z
   git push origin master --tags
   ```
5. CI/CD pipeline builds and deploys automatically.

## Issue & PR Labels

Consistent labeling helps triage and track progress. Use labels such as:

- `type:bug`, `type:feature`, `type:docs`, `type:chore`
- `status:ready`, `status:in progress`, `status:blocked`
- `priority:high`, `priority:medium`, `priority:low`
- `needs review`, `needs tests`, `help wanted`

Customize your label set in **Settings → Labels**.

## CI/CD Workflows

Automate testing, linting, and deployment via GitHub Actions. Example workflow files:

- ``: Run tests and lint on PRs targeting `development`.
- ``: On tag push (`v*.*.*`), build artifacts and deploy to production.

Sample CI job in `ci.yml`:

```yaml
name: CI
on: [pull_request]
jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Install dependencies
        run: npm ci
      - name: Run lint
        run: npm run lint
      - name: Run tests
        run: npm test
```

## Automation & GitHub Actions

Increase productivity by automating common tasks:

- **Stale Bot**: Close inactive issues/PRs.
- **Dependabot**: Automate dependency updates.
- **Labeler**: Automatically apply labels based on file paths.
- **Release Drafter**: Generate draft release notes from merged PRs.

Configure these apps via **Marketplace** or add their configs to `.github/`.

## Pull Request Guidelines

- **Title**: Clear and concise (e.g., `feat: add user onboarding flow`).
- **Description**:
  - Context and motivation.
  - What changed and how to test.
  - Screenshots or examples if UI is affected.
- **Reviewers**: Assign relevant team members.
- **Checklist** (in PR description):
  -

## Best Practices & Tips

- **Atomic Commits**: Keep each commit focused.
- **Rebase vs. Merge**: Rebase for a linear history.
- **CI First**: Fix failing checks before review.
- **Small PRs**: Easier to review and merge.
- **Documentation**: Keep README, docs, and examples up to date.

## Contributing

1. Fork the repo and clone locally.
2. Set `upstream` remote:
   ```bash
   git remote add upstream git@github.com:your-org/using-github-the-proper-way.git
   ```
3. Create a topic branch, follow the workflow above.
4. Open a PR against `development`.
5. Address feedback and merge when approved.

---

*Thanks for contributing!*

