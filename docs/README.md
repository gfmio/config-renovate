# User Documentation

Welcome to the @gfmio/config-renovate user documentation. This guide will help you understand and effectively use these Renovate presets.

## Documentation Structure

### Getting Started

- [Quick Start Guide](quick-start.md) - Get up and running in minutes

### Core Concepts

- [Architecture](architecture.md) - Understanding the preset layering system

### Reference

- [Full Presets](reference/full-presets.md) - Ready-to-use configurations
- [Troubleshooting](troubleshooting.md) - Common issues and solutions

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
- Want to contribute? Read the [Contributing Guide](../CONTRIBUTING.md)
