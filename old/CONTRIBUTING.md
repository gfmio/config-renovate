# Contributing Guide

## Adding New Language Ecosystems

When adding support for a new language or ecosystem (Rust, Python, Go, etc.), follow this structure:

### 1. Create Language Directory

```
presets/
└── <language>/
    ├── base.json           # Common settings for this language
    ├── library.json        # Library-specific config
    ├── app.json            # Application-specific config
    └── tooling/            # Tool-specific presets
        ├── tool1.json
        └── tool2.json
```

### 2. Base Preset Template

Every language should have a `base.json` that includes common settings:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "description": "Base <Language> configuration for <package manager> projects",
  "extends": [
    "group:recommended",
    "workarounds:all"
  ],
  "packageRules": [
    {
      "description": "Language-specific rules",
      "matchManagers": ["<manager-name>"]
    }
  ]
}
```

### 3. Project Type Presets

Create presets for different project types within the ecosystem:

- **library.json**: For libraries/packages meant to be consumed by others
- **app.json** / **cli.json** / **service.json**: For applications
- **monorepo.json**: For monorepo-specific configuration

### 4. Tooling Presets

Create individual presets for popular tools in the ecosystem:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "description": "<Tool> package grouping",
  "packageRules": [
    {
      "description": "Group all <Tool> packages",
      "groupName": "<Tool>",
      "matchPackageNames": ["/^pattern/"]
    }
  ]
}
```

### 5. Create Examples

Add at least one example showing typical usage:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "description": "Example: <Description>",
  "extends": [
    "github>gfmio/config-renovate:presets/base",
    "github>gfmio/config-renovate:presets/security",
    "github>gfmio/config-renovate:presets/<language>/base",
    "github>gfmio/config-renovate:presets/<language>/library"
  ]
}
```

### 6. Update README

Add a new section to README.md documenting the new presets:

```markdown
### <Language> Ecosystem

| Preset | Description |
|--------|-------------|
| `presets/<language>/base` | Common settings for <language> projects |
| `presets/<language>/library` | Library-specific configuration |
```

## Example: Adding Python Support

Here's a concrete example for adding Python:

### Directory Structure

```
presets/
└── python/
    ├── base.json
    ├── library.json
    ├── app.json
    ├── jupyter.json
    └── tooling/
        ├── pytest.json
        ├── black.json
        └── mypy.json
```

### base.json

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "description": "Base Python configuration for pip/poetry/pipenv projects",
  "extends": [
    "group:recommended",
    "workarounds:all"
  ],
  "packageRules": [
    {
      "description": "Group dev dependencies",
      "matchDepTypes": ["dev-dependencies"],
      "groupName": "Python dev dependencies"
    }
  ]
}
```

### library.json

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "description": "Configuration for Python library projects",
  "extends": [
    ":semanticCommits",
    ":separateMajorReleases"
  ],
  "packageRules": [
    {
      "description": "Pin build tools",
      "matchPackageNames": ["build", "setuptools", "wheel"],
      "rangeStrategy": "pin"
    }
  ]
}
```

### Example

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "description": "Example: Python library with pytest and black",
  "extends": [
    "github>gfmio/config-renovate:presets/base",
    "github>gfmio/config-renovate:presets/security",
    "github>gfmio/config-renovate:presets/automerge",
    "github>gfmio/config-renovate:presets/python/base",
    "github>gfmio/config-renovate:presets/python/library",
    "github>gfmio/config-renovate:presets/python/tooling/pytest",
    "github>gfmio/config-renovate:presets/python/tooling/black"
  ]
}
```

## Best Practices

### 1. Keep Presets Focused

Each preset should have a single, clear purpose. Don't mix concerns.

### 2. Use Descriptive Names

File names should clearly indicate what the preset does.

### 3. Add Descriptions

Every preset should have a `description` field explaining its purpose.

### 4. Document Package Rules

Add `description` fields to packageRules explaining why they exist.

### 5. Test Before Committing

Use Renovate's config validation or test in a real repository.

### 6. Follow Existing Patterns

Look at the JavaScript ecosystem presets for structure and style guidance.

### 7. Consider Composition

Think about how presets will be composed together. Avoid conflicts.

### 8. Update Documentation

Keep README.md and MIGRATION.md up to date with new presets.

## Language-Specific Considerations

### Rust

- Cargo.toml and Cargo.lock handling
- Workspace member grouping
- Security advisories from RustSec

### Go

- go.mod and go.sum handling
- Major version in import paths (v2, v3, etc.)
- Stdlib updates

### Python

- Multiple package managers (pip, poetry, pipenv, pdm)
- requirements.txt vs pyproject.toml
- Version pinning for applications vs ranges for libraries

### Swift

- Swift Package Manager
- Platform-specific dependencies (iOS, macOS)
- Xcode version considerations

### Kotlin

- Gradle vs Maven
- Android-specific dependencies
- Kotlin version coordination

## Testing

Before submitting changes:

1. Validate JSON syntax
2. Check schema compliance
3. Test in a real repository if possible
4. Verify preset composition works as expected

## Questions?

Feel free to open an issue for discussion before creating large additions.
