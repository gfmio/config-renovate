# Contributing to @gfmio/config-renovate

Thank you for your interest in contributing! This guide will help you add new presets, improve existing ones, and maintain the quality of this configuration library.

## Table of Contents

- [Getting Started](#getting-started)
- [Development Setup](#development-setup)
- [Architecture Overview](#architecture-overview)
- [Adding New Language Ecosystems](#adding-new-language-ecosystems)
- [Adding Tooling Presets](#adding-tooling-presets)
- [Creating Full Presets](#creating-full-presets)
- [Adding Examples](#adding-examples)
- [Testing and Validation](#testing-and-validation)
- [Documentation](#documentation)
- [Pull Request Process](#pull-request-process)
- [Best Practices](#best-practices)

## Getting Started

### Prerequisites

- [Bun](https://bun.sh/) - Package manager and runtime
- Git

### Development Setup

1. Fork and clone the repository:

```bash
git clone https://github.com/YOUR_USERNAME/config-renovate.git
cd config-renovate
```

2. Install dependencies:

```bash
bun install
```

3. Validate existing configuration:

```bash
bun run validate
```

This runs:

- JSON syntax validation across all `.json` files
- Renovate configuration validation with `--strict` mode
- Biome formatting and linting checks

## Architecture Overview

### Four-Layer Preset Architecture

Presets are organized in a hierarchical structure:

```
Layer 1: Base Presets
├── base.json
├── security.json
├── automerge.json
└── schedule.json

Layer 2: Language Ecosystem Base
├── javascript/base.json
├── python/base.json
├── rust/base.json
├── go/base.json
├── swift/base.json
├── kotlin/base.json
└── nix/base.json

Layer 3: Project Type Specialization
├── javascript/library.json
├── javascript/node.json
├── python/library.json
├── python/app.json
└── ...

Layer 4: Tooling Presets
├── javascript/tooling/typescript.json
├── python/tooling/pytest.json
├── rust/tooling/tokio.json
└── ...
```

### Directory Structure

```
presets/
├── base.json                    # Foundation preset
├── security.json                # Security-focused rules
├── automerge.json              # Automerge strategy
├── schedule.json               # Update scheduling
├── ci/
│   └── github-actions.json     # CI/CD presets
├── <language>/
│   ├── base.json               # Language base config
│   ├── library.json            # Library project type
│   ├── app.json                # Application project type
│   └── tooling/
│       └── <tool>.json         # Tool-specific grouping
└── full/
    └── <language>-<type>.json  # Complete ready-to-use presets

examples/
└── <language>-<type>.json      # Usage examples

docs/                           # User documentation
.github/docs/                   # Developer documentation
```

## Adding New Language Ecosystems

### Step 1: Create Base Preset

Create `presets/<language>/base.json`:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "description": "Base <Language> configuration for <package-manager> projects",
  "extends": [
    "group:recommended",
    "workarounds:all"
  ],
  "packageRules": [
    {
      "description": "Group <language> version updates",
      "matchManagers": ["<manager-name>"],
      "matchPackageNames": ["<language-package-name>"],
      "groupName": "<Language> version",
      "separateMajorMinor": true
    },
    {
      "description": "Group dev dependencies",
      "matchManagers": ["<manager-name>"],
      "matchDepTypes": ["dev-dependencies"],
      "groupName": "<Language> dev dependencies"
    }
  ]
}
```

**Manager Names:**

- npm (JavaScript/TypeScript)
- pip, poetry, pipenv, pdm (Python)
- cargo (Rust)
- gomod (Go)
- swift (Swift Package Manager)
- gradle, maven (Kotlin/Java)
- nix (Nix)

### Step 2: Create Project Type Presets

#### Library Preset

Create `presets/<language>/library.json`:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "description": "<Language> library projects - uses version ranges for maximum compatibility",
  "extends": [
    ":semanticCommits",
    ":separateMajorReleases"
  ],
  "rangeStrategy": "bump",
  "packageRules": [
    {
      "description": "Pin build tools for reproducibility",
      "matchPackageNames": ["<build-tool-1>", "<build-tool-2>"],
      "rangeStrategy": "pin"
    }
  ]
}
```

**Key Principle:** Libraries should use version ranges to avoid breaking downstream consumers.

#### Application Preset

Create `presets/<language>/app.json`:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "description": "<Language> application projects - pins dependencies for reproducibility",
  "rangeStrategy": "pin",
  "lockFileMaintenance": {
    "enabled": true,
    "schedule": ["before 6am on the first day of the month"]
  }
}
```

**Key Principle:** Applications should pin exact versions for reproducible builds.

### Step 3: Create Tooling Presets

Create `presets/<language>/tooling/<tool>.json` for popular tools:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "description": "<Tool> package grouping",
  "packageRules": [
    {
      "description": "Group all <Tool> packages",
      "groupName": "<Tool>",
      "matchPackageNames": ["<tool-package>"],
      "matchPackagePatterns": ["^<tool-prefix>"]
    }
  ]
}
```

**Common Grouping Patterns:**

1. **Framework Families:**

```json
{
  "matchPackageNames": ["react", "react-dom"],
  "matchPackagePatterns": ["^@react/", "^react-"]
}
```

2. **Monorepo Packages:**

```json
{
  "matchSourceUrls": ["https://github.com/<org>/<repo>"]
}
```

3. **Type Definitions:**

```json
{
  "matchPackageNames": ["typescript"],
  "matchPackagePatterns": ["^@types/"]
}
```

### Step 4: Create Full Preset

Create `presets/full/<language>-<type>.json`:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "description": "Complete configuration for <Language> <type> projects with <tools>",
  "extends": [
    "github>gfmio/config-renovate:presets/base",
    "github>gfmio/config-renovate:presets/security",
    "github>gfmio/config-renovate:presets/automerge",
    "github>gfmio/config-renovate:presets/schedule",
    "github>gfmio/config-renovate:presets/<language>/base",
    "github>gfmio/config-renovate:presets/<language>/<type>",
    "github>gfmio/config-renovate:presets/<language>/tooling/<tool1>",
    "github>gfmio/config-renovate:presets/<language>/tooling/<tool2>"
  ]
}
```

### Step 5: Create Example

Create `examples/<language>-<type>.json`:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "description": "Example: <Language> <type> project with <tools>",
  "extends": [
    "github>gfmio/config-renovate:presets/full/<language>-<type>"
  ]
}
```

For custom usage examples, add project-specific overrides:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "description": "Example: <Language> <type> with custom grouping",
  "extends": [
    "github>gfmio/config-renovate:presets/full/<language>-<type>"
  ],
  "packageRules": [
    {
      "groupName": "Custom framework",
      "matchPackageNames": ["framework-core", "framework-utils"]
    }
  ]
}
```

### Step 6: Update Documentation

1. Add ecosystem section to `README.md`
2. Create user guide in `docs/<language>.md`
3. Update developer docs in `.github/docs/presets.md`

## Adding Tooling Presets

To add support for a new tool within an existing ecosystem:

### 1. Research the Tool

Identify:

- Package name(s) and patterns
- Related packages (plugins, types, etc.)
- Monorepo structure (if applicable)
- Update frequency and stability

### 2. Create Tooling Preset

Create `presets/<language>/tooling/<tool>.json`:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "description": "<Tool> package grouping for <Language> projects",
  "packageRules": [
    {
      "description": "Group all <Tool> packages",
      "groupName": "<Tool>",
      "matchManagers": ["<manager>"],
      "matchPackageNames": [
        "<tool-core>",
        "<tool-cli>"
      ],
      "matchPackagePatterns": [
        "^<tool-prefix>",
        "^@<tool-scope>/"
      ]
    }
  ]
}
```

### 3. Add to Appropriate Full Presets

Update relevant full presets to include the new tooling preset.

### 4. Create Example (Optional)

If the tool is widely used, create an example configuration.

## Creating Full Presets

Full presets combine multiple layers. Follow this template:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "description": "Complete configuration for <specific use case>",
  "extends": [
    "github>gfmio/config-renovate:presets/base",
    "github>gfmio/config-renovate:presets/security",
    "github>gfmio/config-renovate:presets/automerge",
    "github>gfmio/config-renovate:presets/schedule",
    "github>gfmio/config-renovate:presets/<language>/base",
    "github>gfmio/config-renovate:presets/<language>/<type>",
    "github>gfmio/config-renovate:presets/<language>/tooling/<tool1>",
    "github>gfmio/config-renovate:presets/<language>/tooling/<tool2>",
    "github>gfmio/config-renovate:presets/ci/<ci-platform>"
  ]
}
```

**Ordering:**

1. Base layer presets (base, security, automerge, schedule)
2. Language ecosystem base
3. Project type preset
4. Tooling presets (alphabetically)
5. CI/CD presets

## Adding Examples

Examples demonstrate real-world usage patterns:

### Simple Example

Shows direct use of a full preset:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "description": "Example: Simple <language> <type> project",
  "extends": [
    "github>gfmio/config-renovate:presets/full/<language>-<type>"
  ]
}
```

### Advanced Example

Shows customization patterns:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "description": "Example: <language> <type> with custom configuration",
  "extends": [
    "github>gfmio/config-renovate:presets/full/<language>-<type>"
  ],
  "schedule": ["every weekend"],
  "packageRules": [
    {
      "groupName": "Internal packages",
      "matchPackageNames": ["@company/*"]
    },
    {
      "matchPackageNames": ["critical-dependency"],
      "automerge": false,
      "minimumReleaseAge": "7 days"
    }
  ]
}
```

## Testing and Validation

### Before Committing

Always run validation:

```bash
bun run validate
```

This executes:

1. **JSON Syntax Validation:**

```bash
bun run validate:json
```

2. **Renovate Configuration Validation:**

```bash
bun run validate:renovate
```

3. **Biome Formatting and Linting:**

```bash
bun run check
```

### Fix Formatting Issues

```bash
bun run format
```

### Manual Testing

Test presets in a real repository:

1. Create a test repository with the target language/ecosystem
2. Add your preset to `renovate.json`:

```json
{
  "extends": ["local>gfmio/config-renovate:presets/<your-preset>"]
}
```

3. Run Renovate locally:

```bash
npx renovate --dry-run
```

4. Review the planned updates

### Integration Testing

For major changes, test against real repositories:

```bash
# Test with actual Renovate run
RENOVATE_TOKEN=<github-token> npx renovate --dry-run <your-test-repo>
```

## Documentation

### JSON Comments

Every preset must include:

1. **Schema reference:**

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json"
}
```

2. **Preset description:**

```json
{
  "description": "Clear, concise description of what this preset does"
}
```

3. **Package rule descriptions:**

```json
{
  "packageRules": [
    {
      "description": "Explain why this rule exists",
      "groupName": "..."
    }
  ]
}
```

### User Documentation

When adding a new ecosystem or major feature:

1. Update `README.md` with overview and quick start
2. Create detailed guide in `docs/<topic>.md`
3. Add examples demonstrating usage

### Developer Documentation

Document architectural decisions in `.github/docs/`:

- `architecture.md` - System design and patterns
- `presets.md` - Preset inventory and specifications
- `testing.md` - Testing approaches

## Pull Request Process

### 1. Create a Branch

```bash
git checkout -b feat/add-ruby-ecosystem
```

Use conventional commit prefixes:

- `feat/` - New features (presets, ecosystems)
- `fix/` - Bug fixes
- `docs/` - Documentation updates
- `chore/` - Maintenance tasks

### 2. Make Changes

Follow the appropriate guide above for your type of contribution.

### 3. Validate

```bash
bun run validate
```

Ensure all checks pass.

### 4. Commit

Use conventional commits format:

```bash
git commit -m "feat(rust): add rust ecosystem presets

- Add base, library, and binary presets
- Add tokio, serde, clap tooling presets
- Create rust-library and rust-cli full presets
- Add usage examples and documentation"
```

**Commit Message Format:**

```
<type>(<scope>): <subject>

<body>
```

**Types:**

- `feat` - New feature
- `fix` - Bug fix
- `docs` - Documentation
- `chore` - Maintenance
- `refactor` - Code refactoring
- `test` - Testing

**Scopes:**

- Language names: `javascript`, `python`, `rust`, `go`, `swift`, `kotlin`, `nix`
- Feature areas: `security`, `automerge`, `schedule`, `ci`
- Infrastructure: `build`, `ci`, `deps`

### 5. Push and Create PR

```bash
git push origin feat/add-ruby-ecosystem
```

Create a pull request with:

- Clear title and description
- Reference to any related issues
- Examples of the new functionality
- Test results

### 6. CI Validation

GitHub Actions will run:

- JSON syntax validation
- Renovate config validation
- Biome formatting/linting
- Commitlint

Ensure all checks pass.

## Best Practices

### Preset Design Principles

#### 1. Single Responsibility

Each preset should have one clear purpose:

**Good:**

```json
{
  "description": "Group all ESLint packages"
}
```

**Bad:**

```json
{
  "description": "Group ESLint, Prettier, and configure automerge"
}
```

#### 2. Composability

Design presets to be combined:

```json
{
  "extends": [
    "github>gfmio/config-renovate:presets/base",
    "github>gfmio/config-renovate:presets/<language>/library",
    "github>gfmio/config-renovate:presets/<language>/tooling/<tool>"
  ]
}
```

#### 3. No Conflicts

Avoid settings that conflict with other presets at the same layer.

#### 4. Clear Descriptions

Every preset and rule needs a description:

```json
{
  "description": "Group all React packages to avoid version mismatches",
  "packageRules": [
    {
      "description": "React core packages must be updated together",
      "groupName": "React",
      "matchPackageNames": ["react", "react-dom"]
    }
  ]
}
```

### Versioning Strategies

#### Libraries: Use Version Ranges

```json
{
  "rangeStrategy": "bump"
}
```

**Rationale:** Libraries consumed by others should specify compatible ranges, not exact versions.

#### Applications: Pin Versions

```json
{
  "rangeStrategy": "pin",
  "lockFileMaintenance": {
    "enabled": true
  }
}
```

**Rationale:** Applications need reproducible builds.

#### Build Tools: Always Pin

```json
{
  "matchDepTypes": ["build"],
  "rangeStrategy": "pin"
}
```

**Rationale:** Build tools affect output artifacts and should be pinned.

### Package Grouping Guidelines

#### Group Related Packages

Packages that should update together:

```json
{
  "groupName": "Framework",
  "matchPackageNames": ["framework-core", "framework-router", "framework-state"]
}
```

#### Separate Major Updates

For critical packages:

```json
{
  "separateMajorMinor": true,
  "separateMinorPatch": false
}
```

#### Use Stability Days for Unstable Packages

```json
{
  "matchPackageNames": ["bleeding-edge-lib"],
  "minimumReleaseAge": "3 days"
}
```

### Security Considerations

#### Always Allow Security Updates

Don't restrict security patches with schedules:

```json
{
  "matchUpdateTypes": ["patch"],
  "matchCurrentVersion": "!/^0/",
  "schedule": ["at any time"]
}
```

#### Automerge Stable Security Patches

```json
{
  "matchDepTypes": ["dependencies"],
  "matchUpdateTypes": ["patch"],
  "matchCurrentVersion": "!/^0/",
  "automerge": true
}
```

### Naming Conventions

All presets must follow standardized naming conventions. See [Naming Conventions Guide](.github/docs/naming-conventions.md) for complete details.

#### Quick Reference

**Preset Files:**

- Lowercase with hyphens: `spring-boot.json`
- Base: Always `base.json`
- Project types: `library.json`, `app.json`, `cli.json` (standardized)
- Tooling: `tooling/<tool-name>.json`
- Full: `<language>-<type>.json`

**Group Names:**

- Title Case: `"Spring Boot"`, `"TypeScript"`, `"React"`
- Be specific and user-friendly
- Follow ecosystem conventions (e.g., `"pytest"` lowercase in Python)

**Descriptions:**

- Sentence case, no ending period
- Explain **why**, not just **what**
- Be specific about impact

**Examples:**

Good:

```json
{
  "description": "Group React packages to prevent version mismatches between react and react-dom"
}
```

Bad:

```json
{
  "description": "react packages"
}
```

**Note**: Some ecosystems have specialized naming (e.g., Rust uses `binary.json` reflecting crate terminology). See the naming conventions guide for rationale

## Language-Specific Considerations

### JavaScript/TypeScript

- Use `@types/*` patterns for type definitions
- Group monorepo packages (React, Babel, etc.)
- Separate TypeScript major updates
- Pin build tools (tsup, vite, webpack)

### Python

- Handle multiple package managers (pip, poetry, pipenv, pdm, uv)
- Pin dependencies for apps, ranges for libraries
- Group test/lint/format tools separately
- Consider Python version updates carefully

### Rust

- Group Rust toolchain updates
- Use semver ranges for libraries
- Pin for binaries
- Group related crate families (tokio, serde)

### Go

- Respect Go module versioning (v2+)
- Use compatible version ranges
- Group related modules

### Swift

- Consider platform requirements (iOS, macOS)
- Handle Swift version updates carefully
- Group related packages (Vapor, Alamofire ecosystems)

### Kotlin/Java

- Coordinate Kotlin version updates
- Group Gradle/Maven versions
- Separate Android-specific dependencies
- Handle multi-module projects

### Nix

- Group flake.lock updates
- Add stability days for nixpkgs major updates
- Handle NixOS vs nix-darwin differences
- Group home-manager separately

## Questions?

- Open an issue for discussion before large additions
- Check existing presets for patterns and examples
- Consult [Renovate documentation](https://docs.renovatebot.com)

Thank you for contributing to @gfmio/config-renovate!
