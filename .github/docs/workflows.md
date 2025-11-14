# GitHub Actions Workflows

This directory contains the CI/CD workflows for the config-renovate repository.

## Workflows

### CI (`ci.yml`)

**Triggers**: Push to `main`, Pull Requests to `main`

**Purpose**: Validates all configuration files and ensures code quality.

**Steps**:

1. **Validate configurations**: Runs JSON validation and Renovate config validation
2. **Check formatting and linting**: Runs Biome checks
3. **Lint commit messages**: Validates commit message format (PR title for PRs, commit messages for pushes)

**Scripts used**:

- `bun run validate` - Validates JSON syntax and Renovate config schema
- `bun run check` - Runs Biome format and lint checks

### Release (`release.yml`)

**Triggers**: Push to `main`

**Purpose**: Creates release PRs and GitHub releases using release-please.

**How it works**:

- Analyzes conventional commits since last release
- Creates/updates a release PR with changelog
- When release PR is merged, creates a GitHub release with tag

**Configuration**:

- `release-please-config.json` - Release configuration
- `.release-please-manifest.json` - Version tracking

### Publish (`publish.yml`)

**Triggers**: GitHub Release published

**Purpose**: Publishes the package to npm registry.

**Requirements**:

- `NPM_TOKEN` secret configured in repository
- `npm` environment configured in repository settings

**Note**: Publishing to npm is optional. This repository can be used directly from GitHub via:

```json
{
  "extends": ["github>gfmio/config-renovate:presets/base"]
}
```

### Documentation (`docs.yml`)

**Triggers**: Push to `main`, Pull Requests to `main`

**Purpose**: Validates markdown documentation and checks for broken links.

**Configuration**:

- `markdown-link-check-config.json` - Link checker configuration

## Local Development

### Run validations locally

```bash
# Validate all configurations
bun run validate

# Check formatting and linting
bun run check

# Fix formatting issues
bun run format

# Validate commit messages (requires commitlint)
echo "feat: add new preset" | bunx commitlint
```

### Test before pushing

```bash
# Run all CI checks
bun run validate && bun run check
```

## Workflow Simplifications vs. template-typescript-library

This repository has simpler workflows compared to `template-typescript-library` because:

1. **No TypeScript compilation** - This is a pure config repository
2. **No tests** - Configuration validity is checked via JSON schema validation
3. **No build artifacts** - Configs are used directly
4. **No documentation generation** - Documentation is written in Markdown
5. **No Nix/devenv** - Uses Bun directly for simplicity
6. **No examples to run** - The examples are reference configs, not runnable code

## Required Secrets

For the full CI/CD pipeline to work, configure these secrets:

- `NPM_TOKEN` - npm access token for publishing (if using npm)

## Required Environments

- `npm` - Configured in repository settings for npm publishing

## Branch Protection

Recommended branch protection rules for `main`:

- Require pull request reviews
- Require status checks to pass:
  - `Validate Configuration`
  - `Validate Documentation`
- Require conversation resolution before merging
- Require linear history
