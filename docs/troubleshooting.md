# Troubleshooting Guide

Common issues and solutions when using @gfmio/config-renovate presets.

## Renovate Not Creating PRs

### Issue: No PRs are being created

**Possible Causes:**

1. **Renovate not enabled** - Check if Renovate App is installed and has access to your repository
2. **Schedule mismatch** - Default schedule is "before 6am on Monday" (UTC)
3. **Rate limits hit** - Check dependency dashboard for rate limit status
4. **All dependencies up to date** - Renovate only creates PRs when updates are available
5. **Onboarding PR not merged** - The initial onboarding PR must be merged first

**Solutions:**

```bash
# Check if Renovate is installed (GitHub)
# Visit: https://github.com/apps/renovate
# Ensure your repository is selected

# Check Renovate logs (for self-hosted)
# Look for errors in the Renovate run logs

# Verify configuration is valid
npx renovate-config-validator
```

**Check schedule**:

```json
{
  "extends": ["@gfmio/config-renovate:presets/full/typescript-library"],
  "schedule": ["at any time"]  // Override to test immediately
}
```

### Issue: Renovate created onboarding PR but no updates after merging

**Possible Causes:**

1. **Schedule not yet reached** - Wait for next scheduled run (Monday morning UTC)
2. **All dependencies already up to date**
3. **Rate limits** - Check `prHourlyLimit` and `prConcurrentLimit`

**Solutions:**

1. Check dependency dashboard issue for pending updates
2. Manually trigger Renovate run (self-hosted) or wait for schedule
3. Review rate limit settings:

```json
{
  "extends": ["@gfmio/config-renovate:presets/full/typescript-library"],
  "prHourlyLimit": 0,  // Disable hourly limit
  "prConcurrentLimit": 0  // Disable concurrent limit
}
```

## Configuration Issues

### Issue: "Preset not found" error

**Example Error:**

```
Cannot find preset @gfmio/config-renovate:presets/full/typescript-library
```

**Solutions:**

1. **For npm presets**: Ensure package is installed

```bash
npm install -D @gfmio/config-renovate
# or
bun add -D @gfmio/config-renovate
```

2. **For GitHub presets**: Use correct reference format

```json
{
  "extends": ["github>gfmio/config-renovate:presets/full/python-library"]
}
```

3. **Check spelling**: Ensure preset path is correct (case-sensitive)

### Issue: Configuration validation fails

**Example Error:**

```
Invalid configuration option
```

**Solutions:**

1. **Validate configuration**:

```bash
npx renovate-config-validator renovate.json
```

2. **Check JSON syntax**:

```bash
# Ensure valid JSON
cat renovate.json | jq .
```

3. **Review schema**: Use `$schema` for IDE validation

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["@gfmio/config-renovate:presets/full/typescript-library"]
}
```

### Issue: Conflicting settings with preset

**Symptom**: Your configuration overrides aren't working

**Cause**: Order of extends matters - last one wins

**Solution**: Put your overrides after the extends:

```json
{
  "extends": ["@gfmio/config-renovate:presets/full/typescript-library"],
  "schedule": ["every weekend"],  // Your override
  "packageRules": [  // Your additional rules
    {
      "matchPackageNames": ["specific-package"],
      "enabled": false
    }
  ]
}
```

## Automerge Issues

### Issue: PRs not automerging

**Possible Causes:**

1. **GitHub auto-merge not enabled** - Check repository settings
2. **Branch protection requirements** - CI must pass
3. **Major update** - Majors require manual approval by design
4. **CI failing** - Automerge only works if all checks pass
5. **Unstable version** - 0.x versions don't automerge

**Solutions:**

1. **Enable GitHub auto-merge**:
   - Settings → General → Pull Requests → Allow auto-merge

2. **Check branch protection**:
   - Settings → Branches → Branch protection rules
   - Ensure "Require status checks to pass" is configured correctly

3. **Verify it's not a major update**:

```json
// Major updates require approval - this is by design
// To automerge majors (not recommended):
{
  "extends": ["@gfmio/config-renovate:presets/full/typescript-library"],
  "packageRules": [
    {
      "matchUpdateTypes": ["major"],
      "dependencyDashboardApproval": false
    }
  ]
}
```

4. **Check CI status**: Ensure all required checks are passing

### Issue: Too many PRs automerging at once

**Solution**: Adjust rate limits

```json
{
  "extends": ["@gfmio/config-renovate:presets/full/typescript-library"],
  "prHourlyLimit": 1,  // Slower rate
  "prConcurrentLimit": 3  // Fewer concurrent PRs
}
```

## Scheduling Issues

### Issue: Updates running at wrong time

**Cause**: Timezone mismatch or schedule syntax error

**Solutions:**

1. **Check timezone**: Default is UTC

```json
{
  "extends": ["@gfmio/config-renovate:presets/full/typescript-library"],
  "timezone": "America/New_York",  // Set your timezone
  "schedule": ["before 6am on Monday"]
}
```

2. **Test schedule syntax**: Use Renovate's schedule testing

3. **Override for testing**:

```json
{
  "extends": ["@gfmio/config-renovate:presets/full/typescript-library"],
  "schedule": ["at any time"]  // Temporarily disable schedule
}
```

### Issue: Updates not running often enough

**Solution**: Adjust schedule

```json
{
  "extends": ["@gfmio/config-renovate:presets/full/typescript-library"],
  "schedule": [
    "before 6am every weekday",
    "every weekend"
  ]
}
```

## Package Grouping Issues

### Issue: Packages that should be grouped are separate

**Cause**: Missing from grouping rule

**Solution**: Add custom grouping

```json
{
  "extends": ["@gfmio/config-renovate:presets/full/typescript-library"],
  "packageRules": [
    {
      "groupName": "My Framework",
      "matchPackageNames": [
        "my-framework-core",
        "my-framework-router",
        "my-framework-state"
      ]
    }
  ]
}
```

### Issue: Too many packages in one group

**Solution**: Split groups

```json
{
  "extends": ["@gfmio/config-renovate:presets/full/typescript-library"],
  "packageRules": [
    {
      "groupName": "React Core",
      "matchPackageNames": ["react", "react-dom"]
    },
    {
      "groupName": "React Testing",
      "matchPackageNames": ["@testing-library/react", "@testing-library/react-hooks"]
    }
  ]
}
```

## Version Range Issues

### Issue: Dependencies pinned when ranges expected (or vice versa)

**Cause**: Using wrong project type preset

**Solution**: Use correct preset

For libraries (use ranges):

```json
{
  "extends": ["github>gfmio/config-renovate:presets/full/python-library"]
}
```

For applications (pin versions):

```json
{
  "extends": ["github>gfmio/config-renovate:presets/full/python-app"]
}
```

### Issue: Want to pin specific packages in library

**Solution**: Add package-specific rule

```json
{
  "extends": ["github>gfmio/config-renovate:presets/full/python-library"],
  "packageRules": [
    {
      "matchPackageNames": ["critical-package"],
      "rangeStrategy": "pin"
    }
  ]
}
```

## Security Update Issues

### Issue: Security updates not automerging

**Possible Causes:**

1. **Package version is 0.x** - Security automerge only for stable versions
2. **CI failing** - Security updates still require passing checks
3. **Custom override blocking** - Check your packageRules

**Solutions:**

1. **Check package version**: 0.x versions don't automerge by design

2. **Verify security preset is included**:

```json
{
  "extends": [
    "github>gfmio/config-renovate:presets/base",
    "github>gfmio/config-renovate:presets/security"  // Ensure this is included
  ]
}
```

3. **Check for conflicting rules**: Review custom packageRules

### Issue: Want to disable security automerge

**Solution**:

```json
{
  "extends": ["github>gfmio/config-renovate:presets/full/python-library"],
  "packageRules": [
    {
      "matchPackageNames": ["*"],
      "matchUpdateTypes": ["patch"],
      "automerge": false
    }
  ]
}
```

## Lock File Issues

### Issue: Lock file not updating

**Possible Causes:**

1. **Lock file maintenance disabled**
2. **Schedule not reached** - Default is first day of month

**Solutions:**

1. **Enable lock file maintenance**:

```json
{
  "extends": ["github>gfmio/config-renovate:presets/full/python-app"],
  "lockFileMaintenance": {
    "enabled": true,
    "schedule": ["before 6am on Monday"]  // Weekly instead of monthly
  }
}
```

2. **Trigger manually**: Via dependency dashboard

## Private Registry Issues

### Issue: Renovate can't access private packages

**Solution**: Configure authentication

See [Renovate's private package documentation](https://docs.renovatebot.com/getting-started/private-packages/).

Example for npm:

```json
{
  "extends": ["@gfmio/config-renovate:presets/full/typescript-library"],
  "npmrc": "@mycompany:registry=https://npm.pkg.github.com/"
}
```

## Performance Issues

### Issue: Too many PR requests overwhelming CI

**Solution**: Reduce rate limits

```json
{
  "extends": ["@gfmio/config-renovate:presets/full/typescript-library"],
  "prHourlyLimit": 1,
  "prConcurrentLimit": 3
}
```

### Issue: Renovate runs taking too long

**Solutions:**

1. **Enable caching** (GitHub Actions):

```yaml
# Already configured in CI if using GitHub Actions
```

2. **Reduce frequency**:

```json
{
  "extends": ["@gfmio/config-renovate:presets/full/typescript-library"],
  "schedule": ["on the first day of the month"]
}
```

3. **Ignore large directories**:

```json
{
  "extends": ["@gfmio/config-renovate:presets/full/typescript-library"],
  "ignorePaths": ["**/node_modules/**", "**/vendor/**"]
}
```

## Debugging Tips

### Enable Debug Logging

For self-hosted Renovate:

```bash
LOG_LEVEL=debug renovate
```

### Check Dependency Dashboard

The dependency dashboard issue shows:

- Pending updates
- Rate limit status
- Errors and warnings
- Updates requiring approval

### Validate Configuration Locally

```bash
# Install Renovate CLI
npm install -g renovate

# Validate configuration
renovate-config-validator

# Dry run
renovate --dry-run
```

### Test in Isolation

Create a minimal test repository with:

- Single dependency
- Your preset configuration
- Observe behavior

## Getting Help

If you're still stuck:

1. **Check Renovate docs**: <https://docs.renovatebot.com>
2. **Search issues**: <https://github.com/gfmio/config-renovate/issues>
3. **Open an issue**: Include:
   - Your `renovate.json`
   - Renovate logs (if available)
   - Expected vs actual behavior
   - Repository link (if public)

## Common Patterns That Work

### Conservative Configuration

For teams wanting manual control:

```json
{
  "extends": ["github>gfmio/config-renovate:presets/full/python-library"],
  "dependencyDashboardApproval": true,  // Require approval for all
  "automerge": false,  // No automerge
  "schedule": ["on the first day of the month"]  // Monthly only
}
```

### Aggressive Configuration

For teams wanting maximum automation:

```json
{
  "extends": ["github>gfmio/config-renovate:presets/full/python-library"],
  "schedule": ["at any time"],  // Always check
  "prHourlyLimit": 10,  // More PRs
  "prConcurrentLimit": 20,  // More concurrent
  "packageRules": [
    {
      "matchUpdateTypes": ["minor", "patch"],
      "automerge": true
    },
    {
      "matchUpdateTypes": ["major"],
      "automerge": false  // Still require approval for majors
    }
  ]
}
```

### Balanced Configuration

For most teams:

```json
{
  "extends": ["github>gfmio/config-renovate:presets/full/python-library"]
  // Use defaults - already balanced
}
```

## FAQ

**Q: Why aren't my overrides working?**
A: Order matters. Put your overrides after `extends` and ensure you're not creating conflicting rules.

**Q: Can I use multiple full presets?**
A: No. Full presets are complete configurations. Use modular presets instead if you need multiple languages.

**Q: How do I disable Renovate for a specific dependency?**
A: Add to `ignoreDeps` or use a packageRule with `enabled: false`.

**Q: Can I test configuration changes safely?**
A: Yes, use `--dry-run` flag with Renovate CLI or create a test repository.

**Q: Why is Renovate ignoring my package.json changes?**
A: Renovate doesn't see manual changes during its run. Merge your changes and wait for next Renovate run.
