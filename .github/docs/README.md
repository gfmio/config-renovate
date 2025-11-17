# Developer Documentation

Internal documentation for developing and maintaining @gfmio/config-renovate.

## Documentation Structure

### Architecture & Design

- [Architecture Overview](architecture.md) - System architecture and design patterns
- [Preset Specifications](presets.md) - Complete preset inventory and specifications
- [Design Decisions](decisions.md) - Rationale behind key choices

### Development

- [Development Setup](development.md) - Local development environment
- [Testing Guide](testing.md) - Testing and validation procedures
- [Release Process](release.md) - Publishing and versioning

### Maintenance

- [Workflows](workflows.md) - CI/CD pipeline documentation
- [Validation](validation.md) - Configuration validation details
- [Troubleshooting](troubleshooting.md) - Common development issues

## Quick Reference

### Project Structure

```
config-renovate/
├── presets/              # All preset configurations
│   ├── base.json         # Base preset
│   ├── security.json     # Security preset
│   ├── automerge.json    # Automerge preset
│   ├── schedule.json     # Schedule preset
│   ├── ci/               # CI/CD presets
│   ├── <language>/       # Language ecosystems
│   │   ├── base.json
│   │   ├── library.json
│   │   ├── app.json
│   │   └── tooling/
│   └── full/             # Complete presets
├── examples/             # Usage examples
├── docs/                 # User documentation
├── .github/
│   ├── workflows/        # CI/CD workflows
│   ├── docs/             # Developer documentation (this)
│   └── renovate.json     # Dogfooding config
├── package.json          # Package metadata
├── default.json          # Default export
└── README.md             # Main documentation
```

### Key Scripts

```bash
# Validate all configurations
bun run validate

# Validate JSON syntax
bun run validate:json

# Validate Renovate configs
bun run validate:renovate

# Format code
bun run format

# Lint code
bun run lint

# Full check (format + lint)
bun run check
```

### Validation Pipeline

All changes must pass:

1. **JSON Syntax Validation**
   - Validates all `.json` files
   - Uses `glob` to find files
   - Checks for syntax errors

2. **Renovate Config Validation**
   - Runs `renovate-config-validator --strict`
   - Validates all presets and examples
   - Ensures schema compliance

3. **Biome Checks**
   - Formatting validation
   - Linting rules
   - Error-on-warnings enabled

4. **Commitlint**
   - Conventional commit format
   - Enforces semantic commit messages

## Key Concepts

### Preset Layering

Presets are organized in 4 layers:

1. **Base:** Cross-language foundation
2. **Ecosystem:** Language-specific
3. **Project Type:** Library vs app
4. **Tooling:** Package grouping

### Configuration Standards

Every preset must include:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "description": "Clear description of what this preset does",
  "packageRules": [
    {
      "description": "Explain why this rule exists"
    }
  ]
}
```

### Naming Conventions

- Files: lowercase-with-hyphens.json
- Group names: Title Case
- Descriptions: Start with capital, be specific

### Version Strategies

| Project Type | Range Strategy | Rationale |
|-------------|----------------|-----------|
| Library | `bump` | Maintain compatibility |
| Application | `pin` | Ensure reproducibility |
| Build Tools | `pin` | Stable build artifacts |

## Contributing Workflow

1. **Create Feature Branch**

   ```bash
   git checkout -b feat/add-ruby-ecosystem
   ```

2. **Make Changes**
   - Add presets following standards
   - Update documentation
   - Add examples

3. **Validate**

   ```bash
   bun run validate
   ```

4. **Commit**

   ```bash
   git commit -m "feat(ruby): add ruby ecosystem presets"
   ```

5. **Push and Create PR**

   ```bash
   git push origin feat/add-ruby-ecosystem
   ```

6. **CI Validation**
   - All checks must pass
   - Review required

7. **Merge**
   - Squash merge preferred
   - Release-please handles versioning

## Release Process

### Automated via Release-Please

1. **Commits to main** trigger release-please
2. **Release PR created** with changelog and version bump
3. **Merge release PR** creates GitHub release
4. **Publish workflow** automatically publishes to npm

### Version Bumping

Based on conventional commits:

- `feat:` → Minor version bump
- `fix:` → Patch version bump
- `feat!:` or `BREAKING CHANGE:` → Major version bump

### Manual Release (if needed)

```bash
# Update version in package.json
bun version <major|minor|patch>

# Create tag
git tag v<version>

# Push tag
git push origin v<version>
```

## Testing Strategies

### Unit Testing (Validation)

```bash
bun run validate
```

### Integration Testing (Manual)

1. Create test repository
2. Reference preset locally:

   ```json
   {
     "extends": ["local>gfmio/config-renovate:presets/<preset>"]
   }
   ```

3. Run Renovate:

   ```bash
   npx renovate --dry-run
   ```

### Real-World Testing

Test against actual repositories before major releases.

## Dogfooding

This repository uses its own presets via `.github/renovate.json`:

```json
{
  "extends": [
    "local>gfmio/config-renovate:presets/base",
    "local>gfmio/config-renovate:presets/security",
    "local>gfmio/config-renovate:presets/automerge",
    "local>gfmio/config-renovate:presets/schedule",
    "local>gfmio/config-renovate:presets/javascript/base",
    "local>gfmio/config-renovate:presets/javascript/tooling/typescript",
    "local>gfmio/config-renovate:presets/javascript/tooling/biome",
    "local>gfmio/config-renovate:presets/ci/github-actions"
  ]
}
```

Changes to presets are tested on this repository first.

## Maintenance Tasks

### Adding Language Ecosystem

See [CONTRIBUTING.md](../../CONTRIBUTING.md) for detailed guide.

Quick checklist:

- [ ] Create `presets/<language>/base.json`
- [ ] Create `presets/<language>/library.json`
- [ ] Create `presets/<language>/app.json`
- [ ] Create tooling presets
- [ ] Create full presets
- [ ] Add examples
- [ ] Update documentation
- [ ] Validate all configs
- [ ] Test in real repository

### Updating Renovate Version

When Renovate adds new features or deprecates options:

1. Review [Renovate changelog](https://github.com/renovatebot/renovate/releases)
2. Update affected presets
3. Run validation
4. Test with latest Renovate version
5. Update documentation if behavior changes

### Reviewing PRs

Checklist:

- [ ] Follows conventional commit format
- [ ] All validation checks pass
- [ ] Preset includes descriptions
- [ ] Documentation updated
- [ ] Examples added (if applicable)
- [ ] No conflicts with existing presets
- [ ] Testing evidence provided

## Common Tasks

### Add New Tooling Preset

```bash
# Create preset file
touch presets/<language>/tooling/<tool>.json

# Add configuration
# Validate
bun run validate

# Add to appropriate full preset
# Create example (optional)
# Update documentation
```

### Update Security Strategy

1. Modify `presets/security.json`
2. Test impact on existing repos
3. Update documentation
4. Communicate changes to users

### Fix Validation Error

```bash
# Run validation to see error
bun run validate

# Fix the issue in the specific file
# Re-validate
bun run validate

# If Biome formatting issue
bun run format
```

## Resources

### Internal

- [Main README](../../README.md)
- [Contributing Guide](../../CONTRIBUTING.md)
- [User Documentation](../../docs/)

### External

- [Renovate Documentation](https://docs.renovatebot.com)
- [Renovate Schema](https://docs.renovatebot.com/renovate-schema.json)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [Release-Please](https://github.com/googleapis/release-please)

## Support

For development questions:

- Check this documentation
- Review existing presets for patterns
- Open discussion in GitHub Issues
- Consult Renovate docs

## License

MIT - see [LICENSE](../../LICENSE)
