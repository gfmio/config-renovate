# @gfmio/config-renovate

[![CI](https://github.com/gfmio/config-renovate/actions/workflows/ci.yml/badge.svg)](https://github.com/gfmio/config-renovate/actions/workflows/ci.yml)
[![npm version](https://badge.fury.io/js/@gfmio%2Fconfig-renovate.svg)](https://www.npmjs.com/package/@gfmio/config-renovate)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Composable [Renovate](https://docs.renovatebot.com) configuration presets for automated dependency updates across multiple language ecosystems.

## Overview

This repository provides 100+ modular Renovate presets that can be composed to create customized dependency update strategies. Instead of maintaining complex Renovate configurations in each repository, you can combine presets that fit your project's needs.

**Supported Ecosystems:**

- JavaScript/TypeScript (npm, yarn, pnpm, Bun)
- Python (pip, poetry, pipenv, pdm, uv)
- Rust (Cargo)
- Go (Go modules)
- Swift (Swift Package Manager)
- Kotlin/Java (Gradle, Maven)
- Nix (flakes, NixOS, nix-darwin)

## Quick Start

### Option 1: Use a Complete Preset (Recommended)

The fastest way to get started is using one of the 33 ready-to-use full presets:

**JavaScript/TypeScript projects (via npm):**

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["@gfmio/config-renovate:presets/full/typescript-library"]
}
```

**Other languages (via GitHub):**

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>gfmio/config-renovate:presets/full/rust-library"]
}
```

See the [Full Presets](#full-presets) section for a complete list.

### Option 2: Compose Individual Presets

For more control, compose individual presets:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [
    "github>gfmio/config-renovate:presets/base",
    "github>gfmio/config-renovate:presets/security",
    "github>gfmio/config-renovate:presets/automerge",
    "github>gfmio/config-renovate:presets/schedule",
    "github>gfmio/config-renovate:presets/python/library",
    "github>gfmio/config-renovate:presets/python/tooling/pytest",
    "github>gfmio/config-renovate:presets/python/tooling/ruff"
  ]
}
```

## Installation

### For JavaScript/TypeScript Projects

Install the npm package (allows shorter preset references):

```bash
npm install -D @gfmio/config-renovate
# or
yarn add -D @gfmio/config-renovate
# or
pnpm add -D @gfmio/config-renovate
# or
bun add -D @gfmio/config-renovate
```

Then reference presets using the shorter syntax:

```json
{
  "extends": ["@gfmio/config-renovate:presets/base"]
}
```

### For Other Languages

No installation needed. Reference presets directly via GitHub:

```json
{
  "extends": ["github>gfmio/config-renovate:presets/base"]
}
```

## Full Presets

Complete, batteries-included configurations combining base settings, security, automerge, and ecosystem-specific rules:

### JavaScript/TypeScript

| Preset | Description |
|--------|-------------|
| `presets/full/typescript-library` | TypeScript library with Biome, Vitest, tsup |
| `presets/full/node-app` | Node.js application with ESLint and Prettier |
| `presets/full/react-frontend` | React application with Vite |
| `presets/full/bun-library` | Bun library with Biome |
| `presets/full/cloudflare-worker` | Cloudflare Worker with Wrangler |
| `presets/full/monorepo` | JavaScript/TypeScript monorepo |

### Python

| Preset | Description |
|--------|-------------|
| `presets/full/python-library` | Python library with Poetry, pytest, mypy, ruff |
| `presets/full/python-app` | Python application with uv and ruff |
| `presets/full/python-jupyter` | Jupyter notebook project |
| `presets/full/python-ml` | ML/AI project with PyTorch/TensorFlow/Transformers |

### Rust

| Preset | Description |
|--------|-------------|
| `presets/full/rust-library` | Rust library with Serde and Tokio |
| `presets/full/rust-cli` | Rust CLI application with Clap |
| `presets/full/rust-web-service` | Rust web service with Axum |
| `presets/full/rust-wasm` | Rust WASM project with wasm-bindgen |

### Go

| Preset | Description |
|--------|-------------|
| `presets/full/go-library` | Go library package |
| `presets/full/go-cli` | Go CLI application with Cobra |
| `presets/full/go-web-service` | Go web service with Gin |
| `presets/full/go-grpc-service` | Go gRPC service |

### Swift

| Preset | Description |
|--------|-------------|
| `presets/full/swift-library` | Swift library package |
| `presets/full/swift-cli` | Swift CLI application |
| `presets/full/swift-ios-app` | iOS/macOS app with SwiftUI |
| `presets/full/swift-vapor` | Vapor web application |

### Kotlin/Java

| Preset | Description |
|--------|-------------|
| `presets/full/kotlin-library` | Kotlin library with Coroutines |
| `presets/full/kotlin-spring-service` | Kotlin Spring Boot service |
| `presets/full/kotlin-ktor-service` | Kotlin Ktor service |
| `presets/full/android-app` | Android app with Compose and Hilt |

### Nix

| Preset | Description |
|--------|-------------|
| `presets/full/nix-flake` | Nix flake project |
| `presets/full/nix-devshell` | Nix development shell |
| `presets/full/nixos` | NixOS system configuration |
| `presets/full/nix-darwin` | nix-darwin macOS configuration |

## Modular Presets

### Base Presets

Foundation presets that work across all languages:

| Preset | Description |
|--------|-------------|
| `presets/base` | Dependency dashboard, labels, config migration, UTC timezone |
| `presets/security` | Vulnerability alerts with automerge for security patches |
| `presets/automerge` | Automerge minor/patch updates, require approval for major |
| `presets/schedule` | Weekly updates (Monday mornings), monthly lock file maintenance |

### Language Ecosystem Presets

Each ecosystem provides base configuration plus project type specialization:

<details>
<summary><strong>JavaScript/TypeScript</strong></summary>

**Core:**

- `presets/javascript/base` - Common settings for npm/yarn/pnpm/bun
- `presets/javascript/library` - Libraries with semantic commits and version ranges
- `presets/javascript/node` - Node.js apps, CLIs, services
- `presets/javascript/bun` - Bun runtime-specific configuration
- `presets/javascript/frontend` - React, Vue, Svelte, Next.js, Vite
- `presets/javascript/monorepo` - Monorepo workspace configuration
- `presets/javascript/cloudflare-workers` - Cloudflare Workers

**Tooling:**

- `presets/javascript/tooling/typescript` - TypeScript and @types packages
- `presets/javascript/tooling/biome` - Biome toolchain
- `presets/javascript/tooling/eslint` - ESLint packages
- `presets/javascript/tooling/prettier` - Prettier packages
- `presets/javascript/tooling/vitest` - Vitest testing framework
- `presets/javascript/tooling/vitepress` - VitePress documentation

</details>

<details>
<summary><strong>Python</strong></summary>

**Core:**

- `presets/python/base` - Common settings for pip/poetry/pipenv/pdm/uv
- `presets/python/library` - Libraries with version ranges
- `presets/python/app` - Applications with pinned dependencies
- `presets/python/jupyter` - Jupyter notebook projects
- `presets/python/ml` - ML/AI projects with PyTorch, TensorFlow, Transformers

**Tooling:**

- `presets/python/tooling/pytest` - pytest testing framework
- `presets/python/tooling/black` - Black code formatter
- `presets/python/tooling/ruff` - Ruff linter and formatter
- `presets/python/tooling/mypy` - mypy type checker
- `presets/python/tooling/poetry` - Poetry package manager
- `presets/python/tooling/uv` - uv package manager
- `presets/python/tooling/sphinx` - Sphinx documentation

</details>

<details>
<summary><strong>Rust</strong></summary>

**Core:**

- `presets/rust/base` - Common settings for Cargo projects
- `presets/rust/library` - Library crates with version ranges
- `presets/rust/binary` - Binary crates with pinned dependencies
- `presets/rust/wasm` - WASM projects with wasm-bindgen

**Tooling:**

- `presets/rust/tooling/tokio` - Tokio async runtime
- `presets/rust/tooling/serde` - Serde serialization
- `presets/rust/tooling/clap` - Clap CLI framework
- `presets/rust/tooling/tracing` - Tracing instrumentation
- `presets/rust/tooling/axum` - Axum web framework

</details>

<details>
<summary><strong>Go</strong></summary>

**Core:**

- `presets/go/base` - Common settings for Go modules
- `presets/go/library` - Library packages with compatible versions
- `presets/go/app` - Applications and services

**Tooling:**

- `presets/go/tooling/gin` - Gin web framework
- `presets/go/tooling/echo` - Echo web framework
- `presets/go/tooling/cobra` - Cobra CLI framework
- `presets/go/tooling/grpc` - gRPC and Protocol Buffers
- `presets/go/tooling/testify` - Testify testing framework

</details>

<details>
<summary><strong>Swift</strong></summary>

**Core:**

- `presets/swift/base` - Common settings for Swift Package Manager
- `presets/swift/library` - Library packages with version ranges
- `presets/swift/app` - Applications and CLIs
- `presets/swift/ios` - iOS/macOS applications

**Tooling:**

- `presets/swift/tooling/vapor` - Vapor web framework
- `presets/swift/tooling/alamofire` - Alamofire HTTP networking
- `presets/swift/tooling/kingfisher` - Kingfisher image loading
- `presets/swift/tooling/swiftui` - SwiftUI packages (TCA, etc.)

</details>

<details>
<summary><strong>Kotlin/Java</strong></summary>

**Core:**

- `presets/kotlin/base` - Common settings for Gradle and Maven
- `presets/kotlin/library` - Libraries with version ranges
- `presets/kotlin/app` - Applications and services
- `presets/kotlin/android` - Android applications

**Tooling:**

- `presets/kotlin/tooling/spring` - Spring Boot and Framework
- `presets/kotlin/tooling/ktor` - Ktor web framework
- `presets/kotlin/tooling/coroutines` - Kotlin Coroutines
- `presets/kotlin/tooling/serialization` - Serialization libraries
- `presets/kotlin/tooling/testing` - Testing frameworks
- `presets/kotlin/tooling/dagger` - Dagger and Hilt DI
- `presets/kotlin/tooling/room` - Android Room database

</details>

<details>
<summary><strong>Nix</strong></summary>

**Core:**

- `presets/nix/base` - Common settings for Nix flakes
- `presets/nix/flake` - Nix flake configuration
- `presets/nix/devshell` - Development shell configuration
- `presets/nix/nixos` - NixOS system configuration

**Tooling:**

- `presets/nix/tooling/home-manager` - Home Manager
- `presets/nix/tooling/darwin` - nix-darwin for macOS
- `presets/nix/tooling/flake-parts` - flake-parts framework
- `presets/nix/tooling/devenv` - devenv.sh framework
- `presets/nix/tooling/fenix` - fenix Rust toolchain
- `presets/nix/tooling/nixpkgs-fmt` - Nix formatters

</details>

### CI/CD Presets

| Preset | Description |
|--------|-------------|
| `presets/ci/github-actions` | GitHub Actions with automerge |

## Key Features

### Security-First

- **Vulnerability Alerts:** Automatically enabled with "security" label
- **Security Patches:** Automerge patches for stable (non-0.x) versions
- **No Schedule Restrictions:** Security updates run immediately

### Smart Automerge

- **Minor/Patch Updates:** Automerged using GitHub's native auto-merge
- **Major Updates:** Require approval via dependency dashboard
- **Squash Strategy:** Clean commit history

### Intelligent Scheduling

- **Weekly Updates:** Monday mornings before 6am UTC
- **Lock File Maintenance:** First day of each month
- **Rate Limiting:** 2 PRs/hour, 10 concurrent (20/4 for monorepos)

### Version Range Strategies

**Libraries** (maximize compatibility):

- Range Strategy: `bump` - maintains semver ranges
- Rationale: Don't break downstream consumers

**Applications** (maximize reproducibility):

- Range Strategy: `pin` - exact versions
- Lock File Maintenance: Enabled monthly
- Rationale: Reproducible builds

### Package Grouping

Automatically groups related packages to reduce PR noise:

- **Monorepo Packages:** Detected and grouped automatically
- **Framework Families:** React, Vue, PyTorch, Spring Boot, etc.
- **Toolchains:** TypeScript + @types, Rust toolchain, Python version
- **Dev Dependencies:** Separate groups for testing, linting, formatting

## Customization

### Override Schedule

```json
{
  "extends": ["github>gfmio/config-renovate:presets/base"],
  "schedule": ["every weekend"]
}
```

### Disable Automerge

```json
{
  "extends": ["github>gfmio/config-renovate:presets/automerge"],
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
  "extends": ["github>gfmio/config-renovate:presets/python/base"],
  "packageRules": [
    {
      "groupName": "Internal packages",
      "matchPackageNames": ["my-company-*"]
    }
  ]
}
```

### Extend Stability Days

```json
{
  "extends": ["github>gfmio/config-renovate:presets/full/python-ml"],
  "packageRules": [
    {
      "matchPackageNames": ["torch", "tensorflow"],
      "minimumReleaseAge": "7 days"
    }
  ]
}
```

## Examples

See the [`examples/`](examples/) directory for complete configurations demonstrating:

- Simple usage with full presets
- Custom package grouping
- Schedule overrides
- Multi-ecosystem projects

## Architecture

### Layered Preset System

Presets are organized in four layers from general to specific:

1. **Base Layer:** Universal settings (dashboard, labels, timezone)
2. **Language Layer:** Ecosystem-specific configuration
3. **Project Type Layer:** Library vs application vs monorepo
4. **Tooling Layer:** Fine-grained package grouping

### Composition Example

```json
{
  "extends": [
    "github>gfmio/config-renovate:presets/base",           // Layer 1
    "github>gfmio/config-renovate:presets/security",       // Layer 1
    "github>gfmio/config-renovate:presets/automerge",      // Layer 1
    "github>gfmio/config-renovate:presets/python/base",    // Layer 2
    "github>gfmio/config-renovate:presets/python/library", // Layer 3
    "github>gfmio/config-renovate:presets/python/tooling/pytest" // Layer 4
  ]
}
```

## Contributing

Contributions are welcome! See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on:

- Adding new language ecosystems
- Creating tooling presets
- Testing and validation
- Documentation standards

## Documentation

- [User Guide](docs/) - Detailed usage documentation
- [Developer Guide](.github/docs/) - Architecture and development
- [Examples](examples/) - Real-world configurations

## License

[MIT](LICENSE)

## Resources

- [Renovate Documentation](https://docs.renovatebot.com)
- [Configuration Options](https://docs.renovatebot.com/configuration-options/)
- [Preset Configs](https://docs.renovatebot.com/presets/)

## Author

Frédérique Mittelstaedt ([@gfmio](https://github.com/gfmio))
