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

**Naming Principle: Specificity Over Generic Terms**

Choose the most specific, informative name for each preset. Prefer:

- **Runtime/environment names** (node, bun, browser) over generic "app"
- **Ecosystem terminology** (binary for Rust) over imposed standards
- **Platform names** (ios, android) over generic terms
- **Domain-specific names** (jupyter, ml) for specialized use cases

**Universal Names:**

- `library.json` - For libraries/packages published to package registries (all ecosystems)

**JavaScript/TypeScript** (high runtime diversity):

- `node.json` - Node.js runtime applications ✅ Specific and meaningful
- `bun.json` - Bun runtime applications ✅ Specific and meaningful
- `frontend.json` - Browser-based applications ✅ Environment-specific
- `monorepo.json` - Monorepo workspace configuration
- `cloudflare-workers.json` - Cloudflare Workers edge functions

**Python** (single primary runtime, domain diversity):

- `app.json` - Python applications ✅ Generic acceptable here
- `jupyter.json` - Jupyter notebooks ✅ Environment-specific
- `ml.json` - Machine learning projects ✅ Domain-specific

**Rust** (ecosystem terminology):

- `binary.json` - Binary crates ✅ Uses Rust's terminology (not generic "app")
- `wasm.json` - WebAssembly target ✅ Target-specific

**Go** (single runtime):

- `app.json` - Go applications ✅ Generic acceptable

**Swift** (platform diversity):

- `app.json` - Generic Swift applications
- `ios.json` - iOS/macOS applications ✅ Platform-specific

**Kotlin/Java** (platform diversity):

- `app.json` - Generic JVM applications
- `android.json` - Android applications ✅ Platform-specific

**Nix** (purpose diversity):

- `flake.json` - Nix flake projects
- `devshell.json` - Development shells
- `nixos.json` - NixOS system configuration ✅ Purpose-specific

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

## Naming Rationale

### Core Philosophy: Specificity Over Uniformity

The naming across ecosystems is **intentionally diverse** because different ecosystems have different needs. We prioritize **meaningful, informative names** over artificial consistency.

### Why Different Patterns?

#### JavaScript: `node.json` vs `app.json`

**Why `node.json` is better:**

- JavaScript has multiple distinct runtimes (Node.js, Bun, Deno, browsers)
- Each runtime has different APIs, constraints, and deployment models
- "node" tells you specifically this is for Node.js server-side execution
- "app" would be meaningless - app for what environment?

**Pattern:** Use runtime/environment names when there's diversity

#### Rust: `binary.json` vs `app.json`

**Why `binary.json` is correct:**

- "Binary crate" is the actual Rust terminology
- Rust developers think in terms of "library crates" vs "binary crates"
- Using ecosystem terminology makes it immediately recognizable
- "app" would feel foreign to Rust developers

**Pattern:** Use ecosystem terminology when it's well-established

#### Python/Go: `app.json` is acceptable

**Why generic `app.json` works here:**

- Python and Go have single primary runtimes
- No meaningful runtime distinction to make
- "Application" is clear enough in these contexts

**Pattern:** Generic terms acceptable when there's no meaningful distinction

### Naming Decision Tree

When naming a project type preset, ask:

1. **Are there multiple runtimes/environments?**
   - YES → Use runtime names (node, bun, browser)
   - NO → Continue to #2

2. **Does the ecosystem have specific terminology?**
   - YES → Use ecosystem terms (binary for Rust)
   - NO → Continue to #3

3. **Are there platform-specific variants?**
   - YES → Use platform names (ios, android, nixos)
   - NO → Continue to #4

4. **Is it domain-specific?**
   - YES → Use domain terms (ml, jupyter)
   - NO → Generic `app.json` is acceptable

### Why NOT `library.json` → `lib.json`?

- **Clarity**: "library" is unambiguous
- **Searchability**: Easier to find and search for
- **Universal**: "library" is understood across all ecosystems
- **Not too long**: Still reasonable length

### Why `frontend.json` not `browser.json`?

- **Developer mental model**: Developers think "frontend" not "browser"
- **Encompasses more**: Includes SSR, build tools, not just browser runtime
- **Common usage**: "Frontend" is the standard term in the industry

## Language-Specific Considerations

### JavaScript/TypeScript

**Ecosystem characteristics**:

- **Multiple runtimes**: Node.js, Bun, Deno, browsers
- Each runtime has distinct APIs and deployment models
- Strong monorepo culture
- Complex frontend tooling ecosystem

**Naming decisions** (runtime-specific):

- `node.json` - Node.js server-side applications ✅ Runtime-specific
- `bun.json` - Bun runtime applications ✅ Runtime-specific
- `frontend.json` - Browser-based applications ✅ Environment-specific
- `monorepo.json` - Workspace projects ✅ Structure-specific
- `cloudflare-workers.json` - Edge runtime ✅ Platform-specific

**Why not `app.json`?** Too vague - doesn't indicate which runtime/environment

### Python

**Ecosystem characteristics**:

- **Single primary runtime** (CPython, with PyPy as edge case)
- Multiple package managers (pip, poetry, pipenv, pdm, uv)
- Strong ML/data science presence
- Notebook-based workflows

**Naming decisions**:

- `app.json` - Python applications ✅ Generic acceptable (single runtime)
- `jupyter.json` - Notebook projects ✅ Environment-specific
- `ml.json` - Machine learning ✅ Domain-specific
- Lowercase tool names following Python conventions (pytest, not PyTest)

**Why `app.json` works here**: No runtime ambiguity unlike JavaScript

### Rust

**Ecosystem characteristics**:

- **Distinct terminology**: "library crates" vs "binary crates"
- Strong type system focus
- Multiple compilation targets (native, WASM)

**Naming decisions** (ecosystem terminology):

- `binary.json` - Binary crates ✅ Correct Rust term (not "app")
- `library.json` - Library crates ✅ Correct Rust term
- `wasm.json` - WebAssembly target ✅ Target-specific
- Tool names follow crate names (tokio, serde, clap)

**Why `binary.json` not `app.json`**: "Binary crate" is how Rust developers think about executables. Using Rust's terminology makes it immediately recognizable and correct.

### Go

**Ecosystem characteristics**:

- **Single runtime** (Go runtime)
- Simple, opinionated tooling
- Standard library focus
- Minimal external frameworks

**Naming decisions**:

- `app.json` - Go applications ✅ Generic acceptable (single runtime)
- `library.json` - Go packages/modules ✅ Clear terminology
- Framework-specific tooling presets (gin, echo, cobra, grpc)

**Why `app.json` works here**: Go has a single runtime, no ambiguity about execution environment

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
