# Architecture

Understanding the architecture of @gfmio/config-renovate will help you compose presets effectively and understand how they interact.

## Four-Layer Architecture

The presets are organized in a hierarchical structure with four distinct layers:

```
┌─────────────────────────────────────────────────────────┐
│ Layer 1: Base Presets (Cross-Language Foundation)      │
│ - base, security, automerge, schedule                   │
└─────────────────────────────────────────────────────────┘
                           ▼
┌─────────────────────────────────────────────────────────┐
│ Layer 2: Ecosystem Base (Language-Specific)            │
│ - javascript/base, python/base, rust/base, etc.         │
└─────────────────────────────────────────────────────────┘
                           ▼
┌─────────────────────────────────────────────────────────┐
│ Layer 3: Project Type (Use Case Specialization)        │
│ - library, app, cli, frontend, monorepo, etc.           │
└─────────────────────────────────────────────────────────┘
                           ▼
┌─────────────────────────────────────────────────────────┐
│ Layer 4: Tooling (Package Grouping)                    │
│ - typescript, pytest, tokio, gin, spring, etc.          │
└─────────────────────────────────────────────────────────┘
```

### Layer 1: Base Presets

**Purpose:** Provide cross-language foundation settings that work for any project.

**Presets:**

1. **`base.json`**
   - Extends Renovate's `config:recommended`
   - Enables dependency dashboard
   - Adds common labels ("dependencies")
   - Sets UTC timezone
   - Enables config migration
   - Configures rebase strategy

2. **`security.json`**
   - Enables vulnerability alerts
   - Labels security updates
   - Automerges security patches for stable packages (non-0.x)
   - Removes schedule restrictions for security updates

3. **`automerge.json`**
   - Automerges minor and patch updates
   - Requires approval for major updates
   - Uses platform automerge (GitHub auto-merge)
   - Sets squash merge strategy

4. **`schedule.json`**
   - Schedules updates for Monday mornings (before 6am UTC)
   - Monthly lock file maintenance
   - Immediate PR creation

**Key Characteristics:**

- Language-agnostic
- Can be used together or separately
- Provide opinionated defaults
- Easily overridden

### Layer 2: Ecosystem Base

**Purpose:** Provide language/package manager-specific configuration.

**Structure:**

```
presets/
├── javascript/base.json
├── python/base.json
├── rust/base.json
├── go/base.json
├── swift/base.json
├── kotlin/base.json
└── nix/base.json
```

**Common Features:**

- Manager-specific settings (`matchManagers`)
- Language version grouping
- Dev dependency grouping
- Ecosystem-specific workarounds
- Range strategy defaults

**Example: `javascript/base.json`**

```json
{
  "extends": ["group:recommended", "workarounds:all"],
  "rangeStrategy": "bump",
  "packageRules": [
    {
      "description": "Ignore node_modules and tests",
      "matchPaths": ["node_modules/**", "test/**", "tests/**"],
      "enabled": false
    }
  ]
}
```

### Layer 3: Project Type

**Purpose:** Specialize configuration for different project types within an ecosystem.

**Common Project Types:**

1. **Library** (`library.json`)
   - Version ranges for dependencies
   - Semantic commits enabled
   - Separate major releases
   - Pinned build tools

2. **Application** (`app.json`, `cli.json`, `node.json`, `binary.json`)
   - Pinned dependencies
   - Lock file maintenance enabled
   - Reproducible builds

3. **Specialized** (`frontend.json`, `monorepo.json`, `jupyter.json`, `ml.json`)
   - Framework-specific grouping
   - Higher PR limits (monorepos)
   - Domain-specific rules

**Key Decision: Library vs Application**

| Aspect | Library | Application |
|--------|---------|-------------|
| **Dependencies** | Version ranges | Pinned versions |
| **Range Strategy** | `bump` | `pin` |
| **Lock Files** | Optional | Required |
| **Goal** | Compatibility | Reproducibility |
| **Consumers** | Other projects | End users |

### Layer 4: Tooling

**Purpose:** Fine-grained package grouping for specific tools and frameworks.

**Structure:**

```
presets/
└── <language>/
    └── tooling/
        ├── <tool-1>.json
        ├── <tool-2>.json
        └── <tool-3>.json
```

**Examples:**

- `javascript/tooling/typescript.json` - Groups TypeScript + @types packages
- `python/tooling/pytest.json` - Groups pytest and plugins
- `rust/tooling/tokio.json` - Groups Tokio ecosystem
- `kotlin/tooling/spring.json` - Groups Spring Boot, Framework, Security, Data

**Benefits:**

- Reduces PR noise
- Ensures related packages update together
- Prevents version mismatches
- Optional (use only what you need)

## Full Presets

**Purpose:** Provide complete, ready-to-use configurations by composing multiple layers.

**Structure:**

```
presets/full/<language>-<type>.json
```

**Composition Pattern:**

```json
{
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

**Benefits:**

- Batteries-included experience
- Sensible defaults
- Can still be customized
- Reduces configuration boilerplate

## Preset Inheritance

### How Extends Works

Renovate merges presets in order:

```json
{
  "extends": [
    "preset-1",  // Applied first
    "preset-2",  // Overrides preset-1
    "preset-3"   // Overrides preset-1 and preset-2
  ],
  "schedule": ["custom"]  // Overrides all presets
}
```

**Merge Behavior:**

| Configuration Type | Merge Strategy |
|-------------------|----------------|
| Primitive values | Last one wins |
| Arrays | Concatenated |
| Objects | Deep merged |
| packageRules | Concatenated |

**Example:**

```json
// Preset A
{
  "prHourlyLimit": 2,
  "labels": ["dependencies"],
  "packageRules": [{"matchPackageNames": ["react"], "groupName": "React"}]
}

// Preset B extends Preset A
{
  "extends": ["preset-a"],
  "prHourlyLimit": 4,
  "labels": ["updates"],
  "packageRules": [{"matchPackageNames": ["vue"], "groupName": "Vue"}]
}

// Result
{
  "prHourlyLimit": 4,  // Overridden
  "labels": ["dependencies", "updates"],  // Concatenated
  "packageRules": [  // Concatenated
    {"matchPackageNames": ["react"], "groupName": "React"},
    {"matchPackageNames": ["vue"], "groupName": "Vue"}
  ]
}
```

### PackageRules Evaluation

PackageRules are evaluated **in order**. Later rules can override earlier ones for matching packages:

```json
{
  "packageRules": [
    {
      "description": "Automerge all minor updates",
      "matchUpdateTypes": ["minor"],
      "automerge": true
    },
    {
      "description": "But not TypeScript minor updates",
      "matchPackageNames": ["typescript"],
      "matchUpdateTypes": ["minor"],
      "automerge": false
    }
  ]
}
```

## Distribution Strategies

### npm Package

**Reference format:** `@gfmio/config-renovate:presets/<path>`

**Requires:** Package installed in project

**Best for:** JavaScript/TypeScript projects already using npm

**Example:**

```json
{
  "extends": ["@gfmio/config-renovate:presets/full/typescript-library"]
}
```

### GitHub Reference

**Reference format:** `github>gfmio/config-renovate:presets/<path>`

**Requires:** Nothing (fetched from GitHub)

**Best for:** Non-JavaScript projects, examples

**Example:**

```json
{
  "extends": ["github>gfmio/config-renovate:presets/full/python-library"]
}
```

### Local Reference (Development)

**Reference format:** `local>gfmio/config-renovate:presets/<path>`

**Requires:** Local repository

**Best for:** Testing, development, dogfooding

**Example:**

```json
{
  "extends": ["local>gfmio/config-renovate:presets/base"]
}
```

## Configuration Resolution

### Resolution Order

1. **Built-in Renovate presets** (`:semanticCommits`, `config:recommended`)
2. **External presets** (npm, GitHub, local)
3. **Repository configuration** (`renovate.json`)
4. **Package rules** (evaluated in order)

### Example Resolution

```json
{
  "extends": [
    "config:recommended",  // Renovate built-in
    "github>gfmio/config-renovate:presets/base",  // External
    "github>gfmio/config-renovate:presets/python/library"  // External
  ],
  "schedule": ["every weekend"],  // Repository override
  "packageRules": [  // Repository rules appended
    {
      "matchPackageNames": ["custom-package"],
      "enabled": false
    }
  ]
}
```

Final configuration is the merged result of all these sources.

## Design Principles

### 1. Single Responsibility

Each preset has one clear purpose:

- ✅ `security.json` - Security configuration
- ✅ `typescript.json` - TypeScript grouping
- ❌ `security-and-typescript.json` - Too broad

### 2. Composability

Presets are designed to work together:

```json
{
  "extends": [
    "github>gfmio/config-renovate:presets/base",
    "github>gfmio/config-renovate:presets/security",
    "github>gfmio/config-renovate:presets/python/library"
  ]
}
```

### 3. No Conflicts

Presets at the same layer avoid conflicting settings.

### 4. Override-Friendly

All settings can be overridden at the repository level:

```json
{
  "extends": ["@gfmio/config-renovate:presets/full/typescript-library"],
  "automerge": false  // Override automerge setting
}
```

### 5. Layered Defaults

More specific layers override more general ones:

- Base layer: Broad defaults
- Ecosystem layer: Language-specific defaults
- Project type layer: Use-case defaults
- Tooling layer: Tool-specific grouping

## Summary

**Key Takeaways:**

1. **Four Layers:** Base → Ecosystem → Project Type → Tooling
2. **Composition:** Combine presets from different layers
3. **Full Presets:** Pre-composed configurations for common scenarios
4. **Inheritance:** Later presets and repository config override earlier ones
5. **Flexibility:** Use full presets for simplicity, or compose your own for control

**Next Steps:**

- [Learn about preset composition](composition.md)
- [Explore customization options](customization.md)
- [Review preset reference](reference/presets.md)
