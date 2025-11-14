# Renovate Configuration Presets

A collection of composable [Renovate](https://docs.renovatebot.com) configuration presets for various programming languages and project types.

## Overview

This repository provides a modular system of Renovate presets that can be combined to create customized dependency update strategies for your projects. Instead of maintaining complex Renovate configurations in each project, you can compose presets that fit your needs.

## Quick Start

Add a `renovate.json` file to your repository and extend the presets you need:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [
    "github>gfmio/config-renovate:presets/base",
    "github>gfmio/config-renovate:presets/security",
    "github>gfmio/config-renovate:presets/automerge",
    "github>gfmio/config-renovate:presets/schedule",
    "github>gfmio/config-renovate:presets/javascript/library",
    "github>gfmio/config-renovate:presets/javascript/tooling/typescript",
    "github>gfmio/config-renovate:presets/javascript/tooling/biome",
    "github>gfmio/config-renovate:presets/ci/github-actions"
  ]
}
```

## Available Presets

### Base Presets

| Preset | Description |
|--------|-------------|
| `presets/base` | Foundation preset with dependency dashboard, labels, and config migration |
| `presets/security` | Security-focused rules with vulnerability alerts and automatic security patches |
| `presets/automerge` | Automerge strategy: automerge minor/patch, require approval for major |
| `presets/schedule` | Weekly update schedule (Monday mornings UTC) with monthly lock file maintenance |

### JavaScript/TypeScript Ecosystem

#### Core Presets

| Preset | Description |
|--------|-------------|
| `presets/javascript/base` | Common settings for npm/yarn/pnpm/bun projects |
| `presets/javascript/library` | Optimized for library projects (semantic commits, versioning strategy) |
| `presets/javascript/node` | Node.js applications, CLIs, and services |
| `presets/javascript/bun` | Bun runtime-specific configuration |
| `presets/javascript/frontend` | Frontend frameworks (React, Vue, Svelte, Next.js, Vite) |
| `presets/javascript/monorepo` | Monorepo workspace configuration |
| `presets/javascript/cloudflare-workers` | Cloudflare Workers and Wrangler |

#### Tooling Presets

| Preset | Description |
|--------|-------------|
| `presets/javascript/tooling/typescript` | TypeScript and @types packages |
| `presets/javascript/tooling/biome` | Biome toolchain |
| `presets/javascript/tooling/eslint` | ESLint packages |
| `presets/javascript/tooling/prettier` | Prettier packages |
| `presets/javascript/tooling/vitest` | Vitest testing framework |
| `presets/javascript/tooling/vitepress` | VitePress documentation |

### CI/CD

| Preset | Description |
|--------|-------------|
| `presets/ci/github-actions` | GitHub Actions with automerge |

## Usage Examples

See the [`examples/`](examples/) directory for complete configuration examples:

- [`typescript-library.json`](examples/typescript-library.json) - TypeScript library with Biome, Vitest, and tsup
- [`node-app.json`](examples/node-app.json) - Node.js application with ESLint and Prettier
- [`react-frontend.json`](examples/react-frontend.json) - React frontend with Vite
- [`bun-library.json`](examples/bun-library.json) - Bun library with built-in test runner
- [`cloudflare-worker.json`](examples/cloudflare-worker.json) - Cloudflare Worker with Wrangler
- [`monorepo.json`](examples/monorepo.json) - JavaScript/TypeScript monorepo

## Design Principles

### 1. Composability

Each preset does one thing well. Combine multiple presets to build your ideal configuration:

```json
{
  "extends": [
    "github>gfmio/config-renovate:presets/base",
    "github>gfmio/config-renovate:presets/javascript/library",
    "github>gfmio/config-renovate:presets/javascript/tooling/typescript"
  ]
}
```

### 2. Layered Inheritance

Presets are organized in layers from general to specific:

- **Base**: Universal settings (labels, dashboard, etc.)
- **Language**: Language/ecosystem-specific settings
- **Project Type**: Library vs app vs monorepo
- **Tooling**: Optional tool-specific grouping

### 3. Easy Overrides

Override any setting in your project's configuration:

```json
{
  "extends": ["github>gfmio/config-renovate:presets/javascript/library"],
  "schedule": ["every weekend"],
  "packageRules": [
    {
      "matchPackageNames": ["my-special-package"],
      "enabled": false
    }
  ]
}
```

## Preset Behaviors

### Security (`presets/security`)

- Vulnerability alerts labeled as "security"
- Automerge security patches for stable (non-0.x) versions
- Security updates run at any time (not restricted to schedule)

### Automerge (`presets/automerge`)

- **Minor & Patch**: Automerged via platform automerge (GitHub auto-merge)
- **Major**: Requires approval via dependency dashboard
- Squash merge strategy

### Schedule (`presets/schedule`)

- Updates run Monday mornings before 6am UTC
- Lock file maintenance runs first day of month
- Reduces PR noise during work week

### JavaScript/TypeScript Presets

- **Range Strategy**: `bump` - keeps semver ranges but bumps to latest
- **Grouping**: Related packages grouped (e.g., all ESLint, all React)
- **Monorepo Support**: Automatically detects and groups monorepo packages
- **Ignore**: Modules and tests ignored by default

## Customization

### Change Update Schedule

```json
{
  "extends": ["github>gfmio/config-renovate:presets/base"],
  "schedule": ["after 10pm every weekday", "every weekend"]
}
```

### Disable Automerge

```json
{
  "extends": [
    "github>gfmio/config-renovate:presets/base",
    "github>gfmio/config-renovate:presets/javascript/library"
  ],
  "packageRules": [
    {
      "matchUpdateTypes": ["minor", "patch"],
      "automerge": false
    }
  ]
}
```

### Add Custom Grouping

```json
{
  "extends": ["github>gfmio/config-renovate:presets/javascript/base"],
  "packageRules": [
    {
      "matchPackageNames": ["my-company-*"],
      "groupName": "Internal packages"
    }
  ]
}
```

## Contributing

Contributions welcome! Feel free to open issues or PRs for:

- New language ecosystems (Python, Rust, Go, etc.)
- Additional tooling presets
- Improved configurations
- Bug fixes

## License

MIT

## Resources

- [Renovate Documentation](https://docs.renovatebot.com)
- [Configuration Options](https://docs.renovatebot.com/configuration-options/)
- [Preset Configs](https://docs.renovatebot.com/presets/)
