# User Documentation

Welcome to the @gfmio/config-renovate user documentation. This guide will help you understand and effectively use these Renovate presets.

## Documentation Structure

### Getting Started

- [Quick Start Guide](quick-start.md) - Get up and running in minutes
- [Installation](installation.md) - Installation instructions for different ecosystems

### Core Concepts

- [Architecture](architecture.md) - Understanding the preset layering system
- [Preset Composition](composition.md) - How to combine presets effectively
- [Configuration Strategies](strategies.md) - Library vs app, security, automerge

### Language Ecosystem Guides

- [JavaScript/TypeScript](languages/javascript.md) - npm, yarn, pnpm, Bun projects
- [Python](languages/python.md) - pip, poetry, pipenv, pdm, uv projects
- [Rust](languages/rust.md) - Cargo projects
- [Go](languages/go.md) - Go modules projects
- [Swift](languages/swift.md) - Swift Package Manager projects
- [Kotlin/Java](languages/kotlin.md) - Gradle and Maven projects
- [Nix](languages/nix.md) - Nix flakes, NixOS, nix-darwin

### Advanced Topics

- [Customization](customization.md) - Overriding and extending presets
- [Security](security.md) - Security update handling and best practices
- [Automerge](automerge.md) - Configuring automatic merge strategies
- [Scheduling](scheduling.md) - Controlling when updates run
- [Package Grouping](grouping.md) - Understanding and customizing package groups

### Reference

- [Preset Reference](reference/presets.md) - Complete preset inventory
- [Full Presets](reference/full-presets.md) - Ready-to-use configurations
- [Troubleshooting](troubleshooting.md) - Common issues and solutions
- [Migration Guide](migration.md) - Migrating from other tools

## Quick Links

### Most Common Use Cases

**TypeScript Library:**

```json
{
  "extends": ["@gfmio/config-renovate:presets/full/typescript-library"]
}
```

**Python Application:**

```json
{
  "extends": ["github>gfmio/config-renovate:presets/full/python-app"]
}
```

**Rust CLI:**

```json
{
  "extends": ["github>gfmio/config-renovate:presets/full/rust-cli"]
}
```

**Go Web Service:**

```json
{
  "extends": ["github>gfmio/config-renovate:presets/full/go-web-service"]
}
```

### Getting Help

- Check the [Troubleshooting Guide](troubleshooting.md)
- Review the [Examples](../examples/)
- Open an [Issue](https://github.com/gfmio/config-renovate/issues)
- Consult [Renovate Documentation](https://docs.renovatebot.com)

## Philosophy

These presets are designed around three core principles:

1. **Composability** - Mix and match presets to build your ideal configuration
2. **Opinionated Defaults** - Sensible defaults that work for most projects
3. **Easy Customization** - Override any setting to match your workflow

## What's Next?

- New to Renovate? Start with the [Quick Start Guide](quick-start.md)
- Migrating from Dependabot? See the [Migration Guide](migration.md)
- Need advanced customization? Check [Customization](customization.md)
- Want to contribute? Read the [Contributing Guide](../CONTRIBUTING.md)
