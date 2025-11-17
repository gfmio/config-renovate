# Repository Structure

## Overview

```
config-renovate/
├── README.md                      # Main documentation
├── CONTRIBUTING.md                # Guide for adding new languages
├── STRUCTURE.md                   # This file - repository structure overview
├── CLAUDE.md                      # AI assistant instructions
│
├── default.json                   # Simple default preset (base only)
├── renovate.json                  # Renovate config for this repo (dogfooding!)
├── renovate.current.json          # Original monolithic config (reference)
├── package.json                   # NPM package with renovate-config field for npm-based presets
│
├── presets/                       # All reusable presets
│   ├── base.json                  # Foundation: dashboard, labels, config migration
│   ├── security.json              # Vulnerability alerts & security patches
│   ├── automerge.json             # Automerge strategy (minor/patch yes, major no)
│   ├── schedule.json              # Weekly schedule + lock file maintenance
│   │
│   ├── ci/                        # CI/CD platform presets
│   │   └── github-actions.json    # GitHub Actions automerge
│   │
│   ├── javascript/                # JavaScript/TypeScript ecosystem
│   │   ├── base.json              # Common JS/TS settings
│   │   ├── library.json           # Library projects
│   │   ├── node.json              # Node.js apps/CLIs/services
│   │   ├── bun.json               # Bun runtime
│   │   ├── frontend.json          # React/Vue/Svelte/Next.js/Vite
│   │   ├── monorepo.json          # Monorepo workspaces
│   │   ├── cloudflare-workers.json # Cloudflare Workers
│   │   │
│   │   └── tooling/               # Tool-specific grouping
│   │       ├── typescript.json    # TypeScript & @types
│   │       ├── biome.json         # Biome toolchain
│   │       ├── eslint.json        # ESLint packages
│   │       ├── prettier.json      # Prettier packages
│   │       ├── vitest.json        # Vitest testing
│   │       └── vitepress.json     # VitePress docs
│   │
│   └── python/                    # Python ecosystem
│       ├── base.json              # Common Python settings
│       ├── library.json           # PyPI libraries
│       ├── app.json               # Applications/CLIs/services
│       ├── jupyter.json           # Jupyter notebooks
│       ├── ml.json                # ML/AI/LLM projects
│       │
│       └── tooling/               # Tool-specific grouping
│           ├── pytest.json        # pytest testing
│           ├── black.json         # Black formatter
│           ├── ruff.json          # Ruff linter/formatter
│           ├── mypy.json          # mypy type checker
│           ├── poetry.json        # Poetry package manager
│           ├── uv.json            # uv package manager
│           └── sphinx.json        # Sphinx docs
│
└── examples/                      # Complete example configurations
    ├── typescript-library.json    # TS library with Biome + Vitest + tsup
    ├── node-app.json              # Node.js app with ESLint + Prettier
    ├── react-frontend.json        # React app with Vite
    ├── bun-library.json           # Bun library
    ├── cloudflare-worker.json     # Cloudflare Worker with Wrangler
    ├── monorepo.json              # JS/TS monorepo
    ├── python-library.json        # Python library with Poetry
    ├── python-app.json            # Python app with uv
    ├── python-jupyter.json        # Jupyter notebooks
    └── python-ml.json             # ML/AI project
```

## Preset Categories

### 1. Base Presets (Root Level)

These are universal and can be used with any language:

- **base.json**: Foundation for all configs
- **security.json**: Security-focused rules
- **automerge.json**: Automerge behavior
- **schedule.json**: Update timing

### 2. Language Ecosystems

Language-specific presets organized by ecosystem:

- **javascript/**: Complete JS/TS ecosystem support
- **python/**: _(planned)_ Python ecosystem
- **rust/**: _(planned)_ Rust ecosystem
- **go/**: _(planned)_ Go ecosystem

### 3. Project Types

Within each language, presets for different project types:

- **base.json**: Common settings for the language
- **library.json**: Packages/libraries
- **app.json** / **node.json**: Applications
- **monorepo.json**: Monorepos

### 4. Tooling

Optional tool-specific presets for granular control:

- **tooling/typescript.json**: TypeScript grouping
- **tooling/biome.json**: Biome grouping
- **tooling/eslint.json**: ESLint grouping
- etc.

### 5. CI/CD

Platform-specific CI/CD configurations:

- **ci/github-actions.json**: GitHub Actions
- **ci/gitlab-ci.json**: _(planned)_ GitLab CI
- **ci/docker.json**: _(planned)_ Docker images

## Usage Patterns

### Minimal Setup

Just the basics:

```json
{
  "extends": ["github>gfmio/config-renovate"]
}
```

### Typical Setup

Base + language + project type:

```json
{
  "extends": [
    "github>gfmio/config-renovate:presets/base",
    "github>gfmio/config-renovate:presets/security",
    "github>gfmio/config-renovate:presets/automerge",
    "github>gfmio/config-renovate:presets/javascript/library"
  ]
}
```

### Comprehensive Setup

Everything including tooling:

```json
{
  "extends": [
    "github>gfmio/config-renovate:presets/base",
    "github>gfmio/config-renovate:presets/security",
    "github>gfmio/config-renovate:presets/automerge",
    "github>gfmio/config-renovate:presets/schedule",
    "github>gfmio/config-renovate:presets/javascript/library",
    "github>gfmio/config-renovate:presets/javascript/tooling/typescript",
    "github>gfmio/config-renovate:presets/javascript/tooling/biome",
    "github>gfmio/config-renovate:presets/ci/github-actions"
  ]
}
```

## Design Philosophy

### Composition Over Configuration

Build your config by composing small, focused presets rather than maintaining a large monolithic configuration.

### Opt-in Complexity

Start simple and add more presets as needed:

1. Base preset
2. Add security
3. Add automerge
4. Add scheduling
5. Add tooling-specific rules

### Clear Separation of Concerns

Each preset has a single, well-defined purpose:

- **What** it configures (base settings, security, etc.)
- **When** updates run (schedule)
- **How** updates are handled (automerge)
- **Which** packages are grouped (tooling)

### Easy to Extend

Adding new languages or tools follows a consistent pattern documented in CONTRIBUTING.md.

## Supported Languages

Currently supported ecosystems:

- ✅ **JavaScript/TypeScript** - npm, yarn, pnpm, bun
- ✅ **Python** - pip, poetry, pipenv, pdm, uv

## Future Additions

Planned language ecosystems:

- Rust (cargo)
- Go (go modules)
- Swift (SPM)
- Kotlin (gradle, maven)
- Ruby (bundler)
- PHP (composer)

Planned CI/CD:

- GitLab CI
- Docker images
- Terraform modules

## Related Files

- **README.md**: User-facing documentation
- **CONTRIBUTING.md**: Guide for adding new presets
