# Full Presets Reference

Complete, batteries-included configurations for specific project types. Each full preset combines base settings, security, automerge, scheduling, and ecosystem-specific configurations.

## Usage

Full presets are the simplest way to get started:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>gfmio/config-renovate:presets/full/<preset-name>"]
}
```

## JavaScript/TypeScript

### typescript-library

**Path:** `presets/full/typescript-library.json`

**Reference:**

- npm: `@gfmio/config-renovate:presets/full/typescript-library`
- GitHub: `github>gfmio/config-renovate:presets/full/typescript-library`

**Description:** TypeScript library with Biome, Vitest, and tsup

**Includes:**

- Base configuration
- Security updates with automerge
- Automerge minor/patch updates
- Weekly update schedule (Monday mornings)
- JavaScript/TypeScript base settings
- Library versioning strategy (version ranges)
- TypeScript + @types grouping
- Biome toolchain grouping
- Vitest testing framework grouping

**Best for:** TypeScript libraries published to npm

**Example:** See `examples/typescript-library.json`

### node-app

**Path:** `presets/full/node-app.json`

**Reference:** `@gfmio/config-renovate:presets/full/node-app`

**Description:** Node.js application with ESLint and Prettier

**Includes:**

- All base layers
- Node.js application settings
- ESLint package grouping
- Prettier package grouping

**Best for:** Node.js backend applications, APIs, services

**Example:** See `examples/node-app.json`

### react-frontend

**Path:** `presets/full/react-frontend.json`

**Reference:** `@gfmio/config-renovate:presets/full/react-frontend`

**Description:** React application with Vite

**Includes:**

- All base layers
- Frontend framework settings
- React package grouping
- Vite build tool configuration

**Best for:** React single-page applications, frontends

**Example:** See `examples/react-frontend.json`

### bun-library

**Path:** `presets/full/bun-library.json`

**Reference:** `@gfmio/config-renovate:presets/full/bun-library`

**Description:** Bun library with Biome

**Includes:**

- All base layers
- Bun runtime configuration
- Library versioning strategy
- Biome toolchain

**Best for:** Libraries using Bun runtime

**Example:** See `examples/bun-library.json`

### cloudflare-worker

**Path:** `presets/full/cloudflare-worker.json`

**Reference:** `@gfmio/config-renovate:presets/full/cloudflare-worker`

**Description:** Cloudflare Worker with Wrangler

**Includes:**

- All base layers
- Cloudflare Workers settings
- Wrangler CLI configuration

**Best for:** Cloudflare Workers, edge functions

**Example:** See `examples/cloudflare-worker.json`

### monorepo

**Path:** `presets/full/monorepo.json`

**Reference:** `@gfmio/config-renovate:presets/full/monorepo`

**Description:** JavaScript/TypeScript monorepo

**Includes:**

- All base layers
- Monorepo workspace detection
- Higher PR limits (20 concurrent, 4/hour)
- Automatic workspace package grouping

**Best for:** Monorepos using npm/yarn/pnpm workspaces

**Example:** See `examples/monorepo.json`

## Python

### python-library

**Path:** `presets/full/python-library.json`

**Reference:** `github>gfmio/config-renovate:presets/full/python-library`

**Description:** Python library with Poetry, pytest, mypy, and ruff

**Includes:**

- All base layers
- Python base configuration
- Library versioning (version ranges)
- pytest grouping
- mypy grouping
- ruff grouping

**Best for:** Python libraries published to PyPI

**Example:** See `examples/python-library.json`

### python-app

**Path:** `presets/full/python-app.json`

**Reference:** `github>gfmio/config-renovate:presets/full/python-app`

**Description:** Python application with uv and ruff

**Includes:**

- All base layers
- Python application settings (pinned dependencies)
- uv package manager
- ruff linter/formatter

**Best for:** Python applications, CLIs, backends

**Example:** See `examples/python-app.json`

### python-jupyter

**Path:** `presets/full/python-jupyter.json`

**Reference:** `github>gfmio/config-renovate:presets/full/python-jupyter`

**Description:** Jupyter notebook project

**Includes:**

- All base layers
- Jupyter-specific grouping
- Notebook tool configuration

**Best for:** Data science notebooks, Jupyter projects

**Example:** See `examples/python-jupyter.json`

### python-ml

**Path:** `presets/full/python-ml.json`

**Reference:** `github>gfmio/config-renovate:presets/full/python-ml`

**Description:** ML/AI project with PyTorch/TensorFlow/Transformers

**Includes:**

- All base layers
- ML framework grouping
- 3-day stability for major ML updates
- Requires approval for major ML framework updates

**Best for:** Machine learning, AI, deep learning projects

**Example:** See `examples/python-ml.json`

## Rust

### rust-library

**Path:** `presets/full/rust-library.json`

**Reference:** `github>gfmio/config-renovate:presets/full/rust-library`

**Description:** Rust library with Serde and Tokio

**Includes:**

- All base layers
- Rust library settings (version ranges)
- Serde grouping
- Tokio grouping

**Best for:** Rust library crates for crates.io

**Example:** See `examples/rust-library.json`

### rust-cli

**Path:** `presets/full/rust-cli.json`

**Reference:** `github>gfmio/config-renovate:presets/full/rust-cli`

**Description:** Rust CLI application with Clap

**Includes:**

- All base layers
- Rust binary settings (pinned dependencies)
- Clap CLI framework

**Best for:** Command-line tools in Rust

**Example:** See `examples/rust-cli.json`

### rust-web-service

**Path:** `presets/full/rust-web-service.json`

**Reference:** `github>gfmio/config-renovate:presets/full/rust-web-service`

**Description:** Rust web service with Axum

**Includes:**

- All base layers
- Rust binary settings
- Axum web framework
- Tokio async runtime

**Best for:** Web services, APIs in Rust

**Example:** See `examples/rust-web-service.json`

### rust-wasm

**Path:** `presets/full/rust-wasm.json`

**Reference:** `github>gfmio/config-renovate:presets/full/rust-wasm`

**Description:** Rust WASM project with wasm-bindgen

**Includes:**

- All base layers
- WASM-specific configuration
- wasm-bindgen grouping

**Best for:** WebAssembly projects in Rust

## Go

### go-library

**Path:** `presets/full/go-library.json`

**Reference:** `github>gfmio/config-renovate:presets/full/go-library`

**Description:** Go library package

**Includes:**

- All base layers
- Go library settings (compatible versions)

**Best for:** Go library packages

**Example:** See `examples/go-library.json`

### go-cli

**Path:** `presets/full/go-cli.json`

**Reference:** `github>gfmio/config-renovate:presets/full/go-cli`

**Description:** Go CLI application with Cobra

**Includes:**

- All base layers
- Go application settings
- Cobra CLI framework

**Best for:** Command-line tools in Go

**Example:** See `examples/go-cli.json`

### go-web-service

**Path:** `presets/full/go-web-service.json`

**Reference:** `github>gfmio/config-renovate:presets/full/go-web-service`

**Description:** Go web service with Gin

**Includes:**

- All base layers
- Go application settings
- Gin web framework

**Best for:** Web services, REST APIs in Go

**Example:** See `examples/go-web-service.json`

### go-grpc-service

**Path:** `presets/full/go-grpc-service.json`

**Reference:** `github>gfmio/config-renovate:presets/full/go-grpc-service`

**Description:** Go gRPC service

**Includes:**

- All base layers
- Go application settings
- gRPC and Protocol Buffers

**Best for:** gRPC services in Go

## Swift

### swift-library

**Path:** `presets/full/swift-library.json`

**Reference:** `github>gfmio/config-renovate:presets/full/swift-library`

**Description:** Swift library package

**Includes:**

- All base layers
- Swift library settings (version ranges)

**Best for:** Swift library packages for Swift Package Manager

**Example:** See `examples/swift-library.json`

### swift-cli

**Path:** `presets/full/swift-cli.json`

**Reference:** `github>gfmio/config-renovate:presets/full/swift-cli`

**Description:** Swift CLI application

**Includes:**

- All base layers
- Swift application settings

**Best for:** Command-line tools in Swift

### swift-ios-app

**Path:** `presets/full/swift-ios-app.json`

**Reference:** `github>gfmio/config-renovate:presets/full/swift-ios-app`

**Description:** iOS/macOS app with SwiftUI

**Includes:**

- All base layers
- iOS/macOS platform settings
- 3-day stability for major updates
- SwiftUI package grouping

**Best for:** iOS and macOS applications

**Example:** See `examples/swift-ios-app.json`

### swift-vapor

**Path:** `presets/full/swift-vapor.json`

**Reference:** `github>gfmio/config-renovate:presets/full/swift-vapor`

**Description:** Vapor web application

**Includes:**

- All base layers
- Swift application settings
- Vapor framework grouping

**Best for:** Web applications using Vapor

## Kotlin/Java

### kotlin-library

**Path:** `presets/full/kotlin-library.json`

**Reference:** `github>gfmio/config-renovate:presets/full/kotlin-library`

**Description:** Kotlin library with Coroutines

**Includes:**

- All base layers
- Kotlin library settings (version ranges)
- Coroutines grouping

**Best for:** Kotlin/Java libraries for Maven Central

**Example:** See `examples/kotlin-library.json`

### kotlin-spring-service

**Path:** `presets/full/kotlin-spring-service.json`

**Reference:** `github>gfmio/config-renovate:presets/full/kotlin-spring-service`

**Description:** Kotlin Spring Boot service

**Includes:**

- All base layers
- Kotlin application settings
- Spring Boot grouping

**Best for:** Backend services using Spring Boot

**Example:** See `examples/kotlin-spring-service.json`

### kotlin-ktor-service

**Path:** `presets/full/kotlin-ktor-service.json`

**Reference:** `github>gfmio/config-renovate:presets/full/kotlin-ktor-service`

**Description:** Kotlin Ktor service

**Includes:**

- All base layers
- Kotlin application settings
- Ktor framework grouping

**Best for:** Backend services using Ktor

### android-app

**Path:** `presets/full/android-app.json`

**Reference:** `github>gfmio/config-renovate:presets/full/android-app`

**Description:** Android app with Compose and Hilt

**Includes:**

- All base layers
- Android-specific settings
- 3-day stability for major updates
- Compose grouping
- Hilt dependency injection grouping

**Best for:** Android applications

**Example:** See `examples/android-app.json`

## Nix

### nix-flake

**Path:** `presets/full/nix-flake.json`

**Reference:** `github>gfmio/config-renovate:presets/full/nix-flake`

**Description:** Nix flake project

**Includes:**

- All base layers
- Nix flake configuration
- Lock file maintenance

**Best for:** Nix flake-based projects

**Example:** See `examples/nix-flake.json`

### nix-devshell

**Path:** `presets/full/nix-devshell.json`

**Reference:** `github>gfmio/config-renovate:presets/full/nix-devshell`

**Description:** Nix development shell

**Includes:**

- All base layers
- Development shell configuration

**Best for:** Development environment flakes

### nixos

**Path:** `presets/full/nixos.json`

**Reference:** `github>gfmio/config-renovate:presets/full/nixos`

**Description:** NixOS system configuration

**Includes:**

- All base layers
- NixOS-specific settings
- 7-day stability for major nixpkgs updates

**Best for:** NixOS system configurations

**Example:** See `examples/nixos.json`

### nix-darwin

**Path:** `presets/full/nix-darwin.json`

**Reference:** `github>gfmio/config-renovate:presets/full/nix-darwin`

**Description:** nix-darwin macOS configuration

**Includes:**

- All base layers
- nix-darwin settings
- darwin-specific grouping

**Best for:** macOS system configuration with nix-darwin

**Example:** See `examples/nix-darwin.json`

## Common Configurations Across All Full Presets

All full presets include:

### Security

- Vulnerability alerts enabled
- Security patches automerged (for stable versions)
- No schedule restrictions for security updates

### Automerge

- Minor and patch updates automerged
- Major updates require approval
- Uses platform automerge (GitHub auto-merge)
- Squash merge strategy

### Scheduling

- Weekly updates (Monday mornings before 6am UTC)
- Lock file maintenance (first day of month, where applicable)
- Immediate PR creation

### Rate Limiting

- 2 PRs per hour (standard)
- 10 concurrent PRs (standard)
- 4 PRs per hour, 20 concurrent (monorepos)

### Labels

- "dependencies" label on all PRs
- "security" label on security updates

## Customizing Full Presets

Full presets can be customized by adding configuration after the `extends`:

```json
{
  "extends": ["github>gfmio/config-renovate:presets/full/python-library"],
  "schedule": ["every weekend"],
  "packageRules": [
    {
      "matchPackageNames": ["my-package"],
      "enabled": false
    }
  ]
}
```

See [Customization Guide](../customization.md) for more examples.

## Choosing the Right Preset

### Libraries vs Applications

**Libraries** (`*-library` presets):

- Use version ranges
- Allow downstream flexibility
- Range strategy: `bump`
- Best when others depend on your code

**Applications** (`*-app`, `*-cli`, `*-service` presets):

- Pin exact versions
- Ensure reproducible builds
- Range strategy: `pin`
- Best for end-user applications

### Ecosystem-Specific Considerations

**JavaScript/TypeScript:**

- Choose based on runtime (Node, Bun) or framework (React, etc.)
- Monorepo preset for workspaces

**Python:**

- Library vs app decision
- ML preset for data science/AI projects
- Jupyter for notebook-based workflows

**Rust:**

- Binary vs library terminology
- WASM preset for WebAssembly targets

**Go:**

- Simple library vs app split
- Framework-specific presets for web services

**Swift:**

- iOS preset for platform-specific apps
- Vapor for backend services

**Kotlin:**

- Android vs backend split
- Spring Boot vs Ktor for web services

**Nix:**

- System config vs application flakes
- Darwin for macOS systems

## Related Documentation

- [Architecture](../architecture.md) - Understanding preset composition
- [Preset Reference](presets.md) - Individual preset details
- [Customization](../customization.md) - Overriding preset settings
- [Quick Start](../quick-start.md) - Getting started guide
