# Quick Start Guide

Get started with @gfmio/config-renovate in under 5 minutes.

## Step 1: Choose Your Preset

The fastest way to get started is using a **full preset** that matches your project type.

### JavaScript/TypeScript Projects

| Your Project | Use This Preset |
|--------------|----------------|
| TypeScript library | `@gfmio/config-renovate:presets/full/typescript-library` |
| Node.js application | `@gfmio/config-renovate:presets/full/node-app` |
| React frontend | `@gfmio/config-renovate:presets/full/react-frontend` |
| Bun library | `@gfmio/config-renovate:presets/full/bun-library` |
| Cloudflare Worker | `@gfmio/config-renovate:presets/full/cloudflare-worker` |
| Monorepo | `@gfmio/config-renovate:presets/full/monorepo` |

### Python Projects

| Your Project | Use This Preset |
|--------------|----------------|
| Python library | `github>gfmio/config-renovate:presets/full/python-library` |
| Python application | `github>gfmio/config-renovate:presets/full/python-app` |
| Jupyter notebooks | `github>gfmio/config-renovate:presets/full/python-jupyter` |
| ML/AI project | `github>gfmio/config-renovate:presets/full/python-ml` |

### Rust Projects

| Your Project | Use This Preset |
|--------------|----------------|
| Rust library | `github>gfmio/config-renovate:presets/full/rust-library` |
| Rust CLI | `github>gfmio/config-renovate:presets/full/rust-cli` |
| Rust web service | `github>gfmio/config-renovate:presets/full/rust-web-service` |
| Rust WASM | `github>gfmio/config-renovate:presets/full/rust-wasm` |

### Go Projects

| Your Project | Use This Preset |
|--------------|----------------|
| Go library | `github>gfmio/config-renovate:presets/full/go-library` |
| Go CLI | `github>gfmio/config-renovate:presets/full/go-cli` |
| Go web service | `github>gfmio/config-renovate:presets/full/go-web-service` |
| Go gRPC service | `github>gfmio/config-renovate:presets/full/go-grpc-service` |

### Swift Projects

| Your Project | Use This Preset |
|--------------|----------------|
| Swift library | `github>gfmio/config-renovate:presets/full/swift-library` |
| Swift CLI | `github>gfmio/config-renovate:presets/full/swift-cli` |
| iOS/macOS app | `github>gfmio/config-renovate:presets/full/swift-ios-app` |
| Vapor app | `github>gfmio/config-renovate:presets/full/swift-vapor` |

### Kotlin/Java/Android Projects

| Your Project | Use This Preset |
|--------------|----------------|
| Kotlin library | `github>gfmio/config-renovate:presets/full/kotlin-library` |
| Spring Boot service | `github>gfmio/config-renovate:presets/full/kotlin-spring-service` |
| Ktor service | `github>gfmio/config-renovate:presets/full/kotlin-ktor-service` |
| Android app | `github>gfmio/config-renovate:presets/full/android-app` |

### Nix Projects

| Your Project | Use This Preset |
|--------------|----------------|
| Nix flake | `github>gfmio/config-renovate:presets/full/nix-flake` |
| Nix devshell | `github>gfmio/config-renovate:presets/full/nix-devshell` |
| NixOS configuration | `github>gfmio/config-renovate:presets/full/nixos` |
| nix-darwin configuration | `github>gfmio/config-renovate:presets/full/nix-darwin` |

## Step 2: Install (JavaScript/TypeScript Only)

If you're using JavaScript or TypeScript, install the package:

```bash
npm install -D @gfmio/config-renovate
```

**Other languages:** No installation needed! Skip to Step 3.

## Step 3: Create Configuration File

Create a `renovate.json` file in your repository root:

### For JavaScript/TypeScript (via npm)

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["@gfmio/config-renovate:presets/full/typescript-library"]
}
```

### For Other Languages (via GitHub)

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>gfmio/config-renovate:presets/full/python-library"]
}
```

Replace the preset name with one from the tables above.

## Step 4: Enable Renovate

### GitHub

1. Install the [Renovate GitHub App](https://github.com/apps/renovate)
2. Grant it access to your repository
3. Renovate will automatically create an onboarding PR

### Self-Hosted

Follow the [Renovate self-hosted documentation](https://docs.renovatebot.com/self-hosted-configuration/).

## What Happens Next?

Once Renovate is enabled, you'll see:

1. **Onboarding PR** - Renovate creates a PR to help you configure it
2. **Dependency Dashboard** - An issue tracking all pending updates
3. **Update PRs** - Automated PRs for dependency updates

## Default Behavior

These full presets provide:

**Security Updates:**

- ✅ Immediate security patches
- ✅ Automerged for stable (non-0.x) packages
- ✅ Labeled with "security"

**Regular Updates:**

- 📅 Run Monday mornings (before 6am UTC)
- ✅ Automerge minor and patch updates
- ⏸️ Major updates require approval
- 📦 Related packages grouped together

**Rate Limiting:**

- 2 PRs per hour
- 10 concurrent PRs maximum
- 🔒 Lock file maintenance monthly

## Customization

Need to adjust the defaults? Add your overrides:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>gfmio/config-renovate:presets/full/python-library"],
  "schedule": ["every weekend"],
  "packageRules": [
    {
      "matchPackageNames": ["critical-package"],
      "automerge": false
    }
  ]
}
```

See [Customization Guide](customization.md) for more examples.

## Common Scenarios

### I Want Updates More/Less Frequently

```json
{
  "extends": ["@gfmio/config-renovate:presets/full/typescript-library"],
  "schedule": ["every weekend"]
}
```

Options:

- `"every weekend"`
- `"after 10pm every weekday"`
- `"before 5am on Monday"`
- `"at any time"` (no schedule)

### I Don't Want Automerge

```json
{
  "extends": ["@gfmio/config-renovate:presets/full/typescript-library"],
  "packageRules": [
    {
      "matchUpdateTypes": ["minor", "patch"],
      "automerge": false
    }
  ]
}
```

### I Want to Group Custom Packages

```json
{
  "extends": ["@gfmio/config-renovate:presets/full/typescript-library"],
  "packageRules": [
    {
      "groupName": "My Framework",
      "matchPackageNames": ["@mycompany/core", "@mycompany/utils"]
    }
  ]
}
```

## Next Steps

- ✅ **Working well?** You're done! Renovate will keep your dependencies updated.
- 🔧 **Need customization?** See [Customization Guide](customization.md)
- 🏗️ **Want more control?** Learn about [Preset Composition](composition.md)
- 🔐 **Private packages?** Configure [Authentication](https://docs.renovatebot.com/getting-started/private-packages/)

## Troubleshooting

### Renovate Isn't Creating PRs

Check:

1. Renovate is enabled for your repository
2. Your `renovate.json` is valid (use [config validator](https://docs.renovatebot.com/config-validation/))
3. The schedule matches your current time (default: Monday mornings UTC)
4. You haven't hit rate limits (check dependency dashboard)

### Too Many PRs

Reduce the rate:

```json
{
  "extends": ["@gfmio/config-renovate:presets/full/typescript-library"],
  "prHourlyLimit": 1,
  "prConcurrentLimit": 5
}
```

Or change to a less frequent schedule:

```json
{
  "extends": ["@gfmio/config-renovate:presets/full/typescript-library"],
  "schedule": ["on the first day of the month"]
}
```

### PRs Aren't Automerging

Ensure:

1. You have GitHub auto-merge enabled in repo settings
2. Branch protection allows auto-merge
3. CI/checks are passing
4. Update is minor or patch (not major)

See [Automerge Guide](automerge.md) for details.

## Getting Help

- [Troubleshooting Guide](troubleshooting.md) - Common issues and solutions
- [Examples Directory](../examples/) - Real-world configurations
- [GitHub Issues](https://github.com/gfmio/config-renovate/issues) - Report bugs or ask questions
- [Renovate Docs](https://docs.renovatebot.com) - Official Renovate documentation
