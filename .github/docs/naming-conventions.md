# Naming Conventions

This document defines the standardized naming conventions for all presets in @gfmio/config-renovate.

## File Naming

### General Rules

- **Format**: lowercase with hyphens: `my-preset.json`
- **Character set**: `a-z`, `0-9`, hyphen (`-`)
- **No underscores**: Use hyphens, not underscores
- **No spaces**: Use hyphens for word separation

### Layer-Specific Conventions

#### Layer 1: Base Presets

Located in `presets/`:

- `base.json` - Foundation preset
- `security.json` - Security configuration
- `automerge.json` - Automerge strategy
- `schedule.json` - Update scheduling

**Pattern**: Single word describing the preset's purpose

#### Layer 2: Ecosystem Base

Located in `presets/<language>/`:

- `base.json` - Always named `base.json` for every ecosystem

**Pattern**: Always `base.json`

#### Layer 3: Project Type Presets

Located in `presets/<language>/`:

**Standard Names** (use these consistently):

- `library.json` - For libraries/packages published to package registries
- `app.json` - For applications, services, and end-user programs
- `cli.json` - For command-line interface applications (optional, can use `app.json`)

**Specialized Names** (ecosystem-specific):

- `frontend.json` - Frontend frameworks (JavaScript ecosystem)
- `monorepo.json` - Monorepo configuration
- `jupyter.json` - Jupyter notebooks (Python ecosystem)
- `ml.json` - Machine learning projects (Python ecosystem)
- `wasm.json` - WebAssembly projects (Rust ecosystem)
- `ios.json` - iOS/macOS applications (Swift ecosystem)
- `android.json` - Android applications (Kotlin ecosystem)

**Historical Names** (being standardized):

- `node.json` → Should be `app.json` (JavaScript)
- `binary.json` → Should be `app.json` (Rust)

#### Layer 4: Tooling Presets

Located in `presets/<language>/tooling/`:

- `<tool-name>.json` - Lowercase tool name with hyphens if multi-word
- Examples: `typescript.json`, `pytest.json`, `spring-boot.json`

**Pattern**: Tool name in lowercase, hyphens for multi-word names

#### Full Presets

Located in `presets/full/`:

- `<language>-<type>.json`
- Examples: `typescript-library.json`, `python-app.json`, `rust-cli.json`

**Pattern**: `{language}-{project-type}`

Where:

- `{language}`: `typescript`, `python`, `rust`, `go`, `swift`, `kotlin`, `nix`
- `{project-type}`: `library`, `app`, `cli`, `web-service`, `frontend`, etc.

#### CI/CD Presets

Located in `presets/ci/`:

- `<platform>.json` - Platform name in lowercase with hyphens
- Examples: `github-actions.json`, `gitlab-ci.json`

**Pattern**: Platform name in lowercase with hyphens

## Group Names

Group names appear in PR titles and should be user-friendly.

### Rules

- **Format**: Title Case with spaces
- **Be specific**: Describe what's being grouped
- **Include context**: Make it clear what the group contains

### Examples

**Good**:

- `"React"` - Clear and recognizable
- `"TypeScript"` - Full tool name
- `"Spring Boot"` - Complete framework name
- `"Python dev dependencies"` - Descriptive with context
- `"Rust toolchain"` - Indicates what's grouped

**Bad**:

- `"TS"` - Too abbreviated
- `"react"` - Lowercase (not title case)
- `"deps"` - Too vague
- `"stuff"` - Meaningless

### Language-Specific Patterns

#### JavaScript/TypeScript

- `"TypeScript"` - not "TS" or "typescript"
- `"React"` - core React packages
- `"ESLint"` - not "eslint" or "Eslint"
- `"Prettier"` - not "prettier"

#### Python

- `"pytest"` - lowercase (following Python convention)
- `"Python dev dependencies"` - descriptive
- `"PyTorch"` - proper capitalization
- `"Jupyter"` - not "jupyter"

#### Rust

- `"Rust toolchain"` - for rust, cargo, rustfmt, clippy
- `"Tokio"` - proper capitalization
- `"Serde"` - proper capitalization

#### Go

- `"Go"` - not "GO" or "go"
- `"Gin"` - proper capitalization
- `"gRPC"` - proper capitalization (following official style)

#### Swift

- `"Swift"` - not "swift"
- `"SwiftUI"` - proper capitalization
- `"Vapor"` - proper capitalization

#### Kotlin/Java

- `"Kotlin"` - not "kotlin"
- `"Spring Boot"` - two words, both capitalized
- `"Ktor"` - proper capitalization

#### Nix

- `"nixpkgs"` - lowercase (following Nix convention)
- `"NixOS"` - proper capitalization
- `"nix-darwin"` - lowercase with hyphen (following project name)

## Description Fields

Every preset and packageRule must have a clear description.

### Preset Descriptions

**Format**: Sentence case, clear and concise, explains the preset's purpose

**Examples**:

```json
{
  "description": "Base Python configuration for pip/poetry/pipenv/pdm/uv projects"
}
```

```json
{
  "description": "TypeScript library projects with semantic commits and version ranges"
}
```

**Rules**:

- Start with capital letter
- No period at the end
- Be specific about what the preset does
- Mention key technologies or tools if relevant

### PackageRule Descriptions

**Format**: Sentence case, explains **why** the rule exists, not just what it does

**Good**:

```json
{
  "description": "Group React packages to prevent version mismatches between react and react-dom"
}
```

```json
{
  "description": "Pin build tools for reproducible build artifacts"
}
```

**Bad**:

```json
{
  "description": "React packages"
}
```

```json
{
  "description": "Pins dependencies"
}
```

**Rules**:

- Explain the rationale
- Be specific about the impact
- Use active voice
- No period at the end

## Topic Names (Semantic Commits)

Topic names appear in commit messages: `chore(deps): update {topic}`

### Rules

- Use the group name or package name
- Title case for frameworks/libraries
- Lowercase for generic terms

### Examples

```json
{
  "groupName": "TypeScript",
  "commitMessageTopic": "TypeScript"
}
```

```json
{
  "groupName": "dependencies",
  "commitMessageTopic": "dependencies"
}
```

## Directory Structure

### Standard Structure

```
presets/
├── base.json
├── security.json
├── automerge.json
├── schedule.json
├── ci/
│   └── <platform>.json
├── <language>/
│   ├── base.json
│   ├── library.json
│   ├── app.json
│   ├── <specialized>.json
│   └── tooling/
│       └── <tool>.json
└── full/
    └── <language>-<type>.json

examples/
└── <language>-<type>.json
```

## Standardization Roadmap

### Current Inconsistencies

1. **JavaScript uses `node.json`** instead of `app.json`
   - Status: Historical naming
   - Plan: Keep for compatibility, document as legacy
   - Recommendation: New projects use `app.json` pattern

2. **Rust uses `binary.json`** instead of `app.json`
   - Status: Reflects Rust terminology (binary vs library crate)
   - Plan: Keep for semantic accuracy
   - Justification: "binary" is the correct Rust term

### Naming Rationale

#### Why `app.json` instead of `application.json`?

- **Brevity**: Shorter is better for file names
- **Consistency**: Matches `library.json` length pattern
- **Common usage**: "app" is widely understood

#### Why `library.json` not `lib.json`?

- **Clarity**: "library" is unambiguous
- **Not too long**: Still reasonably short
- **Standard term**: Used across all ecosystems

#### Why `cli.json` separate from `app.json`?

- **Optional specialization**: CLI apps have specific needs (argument parsing, etc.)
- **Can use `app.json`**: If no CLI-specific configuration needed
- **Clear intent**: Explicitly marks CLI tools

## Language-Specific Considerations

### JavaScript/TypeScript

**Ecosystem characteristics**:

- Multiple package managers (npm, yarn, pnpm, Bun)
- Strong monorepo culture
- Frontend vs backend split

**Naming decisions**:

- `node.json` historically used for Node.js apps
- `frontend.json` for React/Vue/Svelte apps
- `monorepo.json` for workspace projects

### Python

**Ecosystem characteristics**:

- Multiple package managers (pip, poetry, pipenv, pdm, uv)
- Strong ML/data science presence
- Notebook-based workflows

**Naming decisions**:

- `jupyter.json` for notebook projects
- `ml.json` for machine learning
- Lowercase tool names following Python conventions

### Rust

**Ecosystem characteristics**:

- Library crates vs binary crates terminology
- Strong type system focus
- WASM compilation target

**Naming decisions**:

- `binary.json` preserves Rust terminology
- `wasm.json` for WebAssembly projects
- Tool names follow crate names

### Go

**Ecosystem characteristics**:

- Simple, opinionated tooling
- Standard library focus
- Minimal external frameworks

**Naming decisions**:

- Simple `app.json` and `library.json`
- Framework-specific tooling presets

### Swift

**Ecosystem characteristics**:

- iOS/macOS platform split
- SwiftUI vs UIKit
- Apple ecosystem focus

**Naming decisions**:

- `ios.json` for platform-specific configuration
- Tool names follow Swift Package Manager conventions

### Kotlin/Java

**Ecosystem characteristics**:

- Android vs backend split
- Gradle/Maven build systems
- JVM ecosystem

**Naming decisions**:

- `android.json` for Android-specific
- Framework names (Spring, Ktor) capitalized

### Nix

**Ecosystem characteristics**:

- Flakes-based configuration
- NixOS system management
- Reproducible builds focus

**Naming decisions**:

- Follow Nix community conventions (`nixpkgs`, `nix-darwin`)
- `nixos.json` for system configuration

## Validation

All naming conventions are enforced through:

1. **Code review**: Manual check in PRs
2. **Documentation**: This guide
3. **Examples**: Consistent patterns in examples/
4. **CI**: JSON schema validation (where applicable)

## Questions?

If you're unsure about naming:

1. Check existing presets in the same ecosystem
2. Follow the patterns in this document
3. Ask in the PR or issue discussion
4. Default to clarity over brevity

## Changelog

### Initial Version (2025-11)

- Documented existing naming patterns
- Identified standardization opportunities
- Created rationale for naming decisions
- Established rules for new presets
