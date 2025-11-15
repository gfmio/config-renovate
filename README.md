# Renovate Configuration Presets

[![CI](https://github.com/gfmio/config-renovate/actions/workflows/ci.yml/badge.svg)](https://github.com/gfmio/config-renovate/actions/workflows/ci.yml)
[![Release](https://github.com/gfmio/config-renovate/actions/workflows/release.yml/badge.svg)](https://github.com/gfmio/config-renovate/actions/workflows/release.yml)
[![Documentation](https://github.com/gfmio/config-renovate/actions/workflows/docs.yml/badge.svg)](https://github.com/gfmio/config-renovate/actions/workflows/docs.yml)
[![npm version](https://badge.fury.io/js/@gfmio%2Fconfig-renovate.svg)](https://www.npmjs.com/package/@gfmio/config-renovate)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A collection of composable [Renovate](https://docs.renovatebot.com) configuration presets for various programming languages and project types.

## Overview

This repository provides a modular system of Renovate presets that can be combined to create customized dependency update strategies for your projects. Instead of maintaining complex Renovate configurations in each project, you can compose presets that fit your needs.

## Quick Start

### Simplest: Use a Full Preset

For JavaScript/TypeScript projects (using npm):

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["@gfmio/config-renovate:presets/full/typescript-library"]
}
```

For Python/Rust/Go projects (using GitHub):

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>gfmio/config-renovate:presets/full/python-library"]
}
```

### Advanced: Compose Individual Presets

For JavaScript/TypeScript (npm):

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [
    "@gfmio/config-renovate:presets/base",
    "@gfmio/config-renovate:presets/security",
    "@gfmio/config-renovate:presets/automerge",
    "@gfmio/config-renovate:presets/schedule",
    "@gfmio/config-renovate:presets/javascript/library",
    "@gfmio/config-renovate:presets/javascript/tooling/typescript"
  ]
}
```

For Python (GitHub):

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [
    "github>gfmio/config-renovate:presets/base",
    "github>gfmio/config-renovate:presets/security",
    "github>gfmio/config-renovate:presets/python/library"
  ]
}
```

## Available Presets

### Full Presets (Ready-to-Use)

Complete configurations that combine all necessary presets for specific use cases:

| Preset | Reference | Description |
|--------|-----------|-------------|
| `presets/full/typescript-library` | npm: `@gfmio/config-renovate:` | TypeScript library with Biome, Vitest, tsup |
| `presets/full/node-app` | npm: `@gfmio/config-renovate:` | Node.js app with ESLint, Prettier |
| `presets/full/react-frontend` | npm: `@gfmio/config-renovate:` | React app with Vite |
| `presets/full/bun-library` | npm: `@gfmio/config-renovate:` | Bun library with Biome |
| `presets/full/cloudflare-worker` | npm: `@gfmio/config-renovate:` | Cloudflare Worker with Wrangler |
| `presets/full/monorepo` | npm: `@gfmio/config-renovate:` | JS/TS monorepo |
| `presets/full/python-library` | GitHub: `github>gfmio/config-renovate:` | Python library with Poetry |
| `presets/full/python-app` | GitHub: `github>gfmio/config-renovate:` | Python app with uv |
| `presets/full/python-jupyter` | GitHub: `github>gfmio/config-renovate:` | Jupyter notebooks |
| `presets/full/python-ml` | GitHub: `github>gfmio/config-renovate:` | ML/AI with PyTorch/Transformers |
| `presets/full/rust-library` | GitHub: `github>gfmio/config-renovate:` | Rust library with Serde, Tokio |
| `presets/full/rust-cli` | GitHub: `github>gfmio/config-renovate:` | Rust CLI with Clap |
| `presets/full/rust-web-service` | GitHub: `github>gfmio/config-renovate:` | Rust web service with Axum |
| `presets/full/rust-wasm` | GitHub: `github>gfmio/config-renovate:` | Rust WASM with wasm-bindgen |
| `presets/full/go-library` | GitHub: `github>gfmio/config-renovate:` | Go library package |
| `presets/full/go-cli` | GitHub: `github>gfmio/config-renovate:` | Go CLI with Cobra |
| `presets/full/go-web-service` | GitHub: `github>gfmio/config-renovate:` | Go web service with Gin |
| `presets/full/go-grpc-service` | GitHub: `github>gfmio/config-renovate:` | Go gRPC service |

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

### Python Ecosystem

#### Core Presets

| Preset | Description |
|--------|-------------|
| `presets/python/base` | Common settings for pip/poetry/pipenv/pdm/uv projects |
| `presets/python/library` | Python libraries published to PyPI (uses version ranges) |
| `presets/python/app` | Python applications, CLIs, and services (pins dependencies) |
| `presets/python/jupyter` | Jupyter notebook projects with notebook-specific grouping |
| `presets/python/ml` | ML/AI/LLM projects (PyTorch, TensorFlow, Transformers, LangChain) |

#### Tooling Presets

| Preset | Description |
|--------|-------------|
| `presets/python/tooling/pytest` | pytest testing framework |
| `presets/python/tooling/black` | Black code formatter |
| `presets/python/tooling/ruff` | Ruff linter and formatter |
| `presets/python/tooling/mypy` | mypy type checker and type stubs |
| `presets/python/tooling/poetry` | Poetry package manager |
| `presets/python/tooling/uv` | uv package manager |
| `presets/python/tooling/sphinx` | Sphinx documentation generator |

### Rust Ecosystem

#### Core Presets

| Preset | Description |
|--------|-------------|
| `presets/rust/base` | Common settings for Cargo projects |
| `presets/rust/library` | Rust library crates published to crates.io (uses version ranges) |
| `presets/rust/binary` | Rust binary crates - CLIs, applications, services (pins dependencies) |
| `presets/rust/wasm` | Rust WASM projects with wasm-bindgen configuration |

#### Tooling Presets

| Preset | Description |
|--------|-------------|
| `presets/rust/tooling/tokio` | Tokio async runtime |
| `presets/rust/tooling/serde` | Serde serialization framework |
| `presets/rust/tooling/clap` | Clap CLI framework |
| `presets/rust/tooling/tracing` | Tracing instrumentation framework |
| `presets/rust/tooling/axum` | Axum web framework |

### Go Ecosystem

#### Core Presets

| Preset | Description |
|--------|-------------|
| `presets/go/base` | Common settings for Go modules projects |
| `presets/go/library` | Go library packages (uses compatible versions) |
| `presets/go/app` | Go applications, CLIs, and services |

#### Tooling Presets

| Preset | Description |
|--------|-------------|
| `presets/go/tooling/gin` | Gin web framework |
| `presets/go/tooling/echo` | Echo web framework |
| `presets/go/tooling/cobra` | Cobra CLI framework and Viper |
| `presets/go/tooling/grpc` | gRPC and Protocol Buffers |
| `presets/go/tooling/testify` | Testify testing framework |

### CI/CD

| Preset | Description |
|--------|-------------|
| `presets/ci/github-actions` | GitHub Actions with automerge |

## Usage Examples

See the [`examples/`](examples/) directory for complete configuration examples:

**JavaScript/TypeScript:**

- [`typescript-library.json`](examples/typescript-library.json) - TypeScript library with Biome, Vitest, and tsup
- [`node-app.json`](examples/node-app.json) - Node.js application with ESLint and Prettier
- [`react-frontend.json`](examples/react-frontend.json) - React frontend with Vite
- [`bun-library.json`](examples/bun-library.json) - Bun library with built-in test runner
- [`cloudflare-worker.json`](examples/cloudflare-worker.json) - Cloudflare Worker with Wrangler
- [`monorepo.json`](examples/monorepo.json) - JavaScript/TypeScript monorepo

**Python:**
- [`python-library.json`](examples/python-library.json) - Python library with Poetry, pytest, mypy, and ruff
- [`python-app.json`](examples/python-app.json) - Python application with uv and ruff
- [`python-jupyter.json`](examples/python-jupyter.json) - Jupyter notebook project for data science
- [`python-ml.json`](examples/python-ml.json) - ML/AI project with PyTorch and Hugging Face Transformers

**Rust:**
- [`rust-library.json`](examples/rust-library.json) - Rust library with Serde and Tokio
- [`rust-cli.json`](examples/rust-cli.json) - Rust CLI application with Clap
- [`rust-web-service.json`](examples/rust-web-service.json) - Rust web service with Axum and Tokio

**Go:**
- [`go-library.json`](examples/go-library.json) - Go library package
- [`go-cli.json`](examples/go-cli.json) - Go CLI application with Cobra
- [`go-web-service.json`](examples/go-web-service.json) - Go web service with Gin

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

## npm Package

When published to npm, this package includes:

- All preset configurations (`presets/`)
- The default configuration (`default.json`)

**Note:** Example configurations are only available via GitHub references and are not published to npm. They serve as documentation and reference material.

You can reference any preset file from the npm package:

```json
{
  "extends": ["@gfmio/config-renovate:presets/python/ml"]
}
```

## Contributing

Contributions welcome! Feel free to open issues or PRs for:

- New language ecosystems (Rust, Go, Swift, etc.)
- Additional tooling presets
- Improved configurations
- Bug fixes

## License

MIT

## Resources

- [Renovate Documentation](https://docs.renovatebot.com)
- [Configuration Options](https://docs.renovatebot.com/configuration-options/)
- [Preset Configs](https://docs.renovatebot.com/presets/)
