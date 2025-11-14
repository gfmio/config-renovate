# Migration Guide

## From `renovate.current.json` to Modular Presets

Your original `renovate.current.json` configuration has been refactored into composable presets. Here's how the settings map:

### Original Configuration

```json
{
  "extends": [
    "config:recommended",
    ":dependencyDashboard",
    ":semanticCommits",
    // ... many more presets
  ],
  "timezone": "UTC",
  "schedule": ["before 6am on Monday"],
  // ... all your custom rules
}
```

### New Modular Approach

The same configuration can now be achieved by composing presets:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [
    "github>gfmio/config-renovate:presets/base",
    "github>gfmio/config-renovate:presets/security",
    "github>gfmio/config-renovate:presets/automerge",
    "github>gfmio/config-renovate:presets/schedule",
    "github>gfmio/config-renovate:presets/javascript/base",
    "github>gfmio/config-renovate:presets/javascript/library",
    "github>gfmio/config-renovate:presets/javascript/tooling/typescript",
    "github>gfmio/config-renovate:presets/javascript/tooling/biome",
    "github>gfmio/config-renovate:presets/javascript/tooling/vitest",
    "github>gfmio/config-renovate:presets/javascript/tooling/vitepress",
    "github>gfmio/config-renovate:presets/javascript/tooling/eslint",
    "github>gfmio/config-renovate:presets/javascript/tooling/prettier",
    "github>gfmio/config-renovate:presets/ci/github-actions"
  ]
}
```

Or use the Bun variant if you're using Bun:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [
    "github>gfmio/config-renovate:presets/base",
    "github>gfmio/config-renovate:presets/security",
    "github>gfmio/config-renovate:presets/automerge",
    "github>gfmio/config-renovate:presets/schedule",
    "github>gfmio/config-renovate:presets/javascript/base",
    "github>gfmio/config-renovate:presets/javascript/library",
    "github>gfmio/config-renovate:presets/javascript/bun",
    "github>gfmio/config-renovate:presets/javascript/tooling/typescript",
    "github>gfmio/config-renovate:presets/javascript/tooling/biome",
    "github>gfmio/config-renovate:presets/javascript/tooling/vitest",
    "github>gfmio/config-renovate:presets/ci/github-actions"
  ]
}
```

## Mapping Table

| Original Setting | New Preset Location |
|-----------------|---------------------|
| `config:recommended` | `presets/base.json` |
| `:dependencyDashboard` | `presets/base.json` |
| `timezone`, `labels`, `rebaseWhen` | `presets/base.json` |
| `vulnerabilityAlerts` | `presets/security.json` |
| Automerge minor/patch rules | `presets/automerge.json` |
| Major update approval | `presets/automerge.json` |
| `schedule`, `lockFileMaintenance` | `presets/schedule.json` |
| `:semanticCommits`, `:separateMajorReleases` | `presets/javascript/library.json` |
| `rangeStrategy: bump` | `presets/javascript/base.json` |
| `group:monorepos`, `:ignoreModulesAndTests` | `presets/javascript/base.json` |
| TypeScript grouping | `presets/javascript/tooling/typescript.json` |
| Biome grouping | `presets/javascript/tooling/biome.json` |
| Vitest grouping | `presets/javascript/tooling/vitest.json` |
| VitePress grouping | `presets/javascript/tooling/vitepress.json` |
| ESLint grouping | `presets/javascript/tooling/eslint.json` |
| Prettier grouping | `presets/javascript/tooling/prettier.json` |
| GitHub Actions automerge | `presets/ci/github-actions.json` |
| Bun major/patch rules | `presets/javascript/bun.json` |
| tsup pinning | `presets/javascript/library.json` |
| Linting tools grouping | `presets/javascript/base.json` |
| Documentation tools grouping | `presets/javascript/base.json` |

## Benefits of the New Structure

### 1. Reusability
Use the same presets across multiple projects without copy-pasting.

### 2. Maintainability
Update configurations in one place, all projects benefit.

### 3. Flexibility
Mix and match only the presets you need for each project.

### 4. Clarity
Each preset has a single, clear purpose with good documentation.

### 5. Evolution
Easy to add new presets for new tools or languages.

## Examples by Project Type

### TypeScript Library (like your original)
```json
{
  "extends": [
    "github>gfmio/config-renovate:presets/base",
    "github>gfmio/config-renovate:presets/security",
    "github>gfmio/config-renovate:presets/automerge",
    "github>gfmio/config-renovate:presets/schedule",
    "github>gfmio/config-renovate:presets/javascript/library",
    "github>gfmio/config-renovate:presets/javascript/tooling/typescript",
    "github>gfmio/config-renovate:presets/javascript/tooling/biome"
  ]
}
```

### Node.js App
```json
{
  "extends": [
    "github>gfmio/config-renovate:presets/base",
    "github>gfmio/config-renovate:presets/security",
    "github>gfmio/config-renovate:presets/automerge",
    "github>gfmio/config-renovate:presets/schedule",
    "github>gfmio/config-renovate:presets/javascript/node",
    "github>gfmio/config-renovate:presets/javascript/tooling/typescript"
  ]
}
```

### React Frontend
```json
{
  "extends": [
    "github>gfmio/config-renovate:presets/base",
    "github>gfmio/config-renovate:presets/security",
    "github>gfmio/config-renovate:presets/automerge",
    "github>gfmio/config-renovate:presets/schedule",
    "github>gfmio/config-renovate:presets/javascript/frontend",
    "github>gfmio/config-renovate:presets/javascript/tooling/typescript"
  ]
}
```

## Next Steps

1. Choose the appropriate example from `examples/` directory
2. Copy it to your project as `renovate.json`
3. Customize by adding/removing preset extends as needed
4. Test with Renovate's dry-run mode if available
