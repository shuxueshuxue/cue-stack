# Release Guide

This guide explains how to release packages from the CueStack monorepo.

## Version Management Strategy

**Independent package versions** with **unified release tags**.

- Each package maintains its own version number
- Releases are triggered by `release-*` tags
- Only packages with version changes are published

## Release Process

### 1. Update Package Versions

Update version numbers in the packages you want to release:

**For cue-console:**
```bash
# Edit cue-console/package.json
"version": "0.1.25"
```

**For cue-command:**
```bash
# Edit cue-command/package.json
"version": "0.1.17"
```

**For cue-mcp:**
```bash
# Edit BOTH files:
# cue-mcp/pyproject.toml
version = "0.1.13"

# cue-mcp/cuemcp/__init__.py
__version__ = "0.1.13"
```

### 2. Commit Version Changes

```bash
git add .
git commit -m "chore: bump versions for release

- cue-console: 0.1.25
- cue-command: 0.1.17
- cue-mcp: 0.1.13
"
```

### 3. Create Release Tag

Tag format: `release-YYYYMMDD.N` where N is the release number for that day.

```bash
git tag release-20260125.1
git push origin main
git push origin release-20260125.1
```

### 4. GitHub Actions Auto-Publish

The workflow will:
1. Detect which packages have version changes
2. Publish only changed packages to npm/PyPI
3. Create a GitHub Release with package information

### 5. Write Release Notes

After the release is created, edit the GitHub Release to add:

```markdown
## Features
- console: Added new UI feature X
- command: Support for new protocol Y

## Fixes
- mcp: Fixed connection issue Z

## Published Packages
- ✅ `cue-console@0.1.25`
- ✅ `cueme@0.1.17`
- ✅ `cuemcp@0.1.13`
```

## Version Bump Guidelines

### When to bump versions

- **Patch (0.0.X)**: Bug fixes, documentation updates
- **Minor (0.X.0)**: New features, non-breaking changes
- **Major (X.0.0)**: Breaking changes

### Independent versioning

Packages can be released independently:
- Only bump versions for packages that changed
- Unchanged packages will be skipped by CI

## Troubleshooting

### GitHub Actions fails

Check:
1. `NPM_TOKEN` secret is set in repository settings
2. `PYPI_TOKEN` secret is set in repository settings
3. Version numbers were actually changed in the files

### Version detection not working

The workflow compares version files between the current tag and the previous `release-*` tag. Ensure:
- Version changes are committed before tagging
- Tag follows the `release-*` format

## Local Testing

Test package builds locally before releasing:

```bash
# Test console build
pnpm --filter cue-console build

# Test command
pnpm --filter cueme prepare

# Test mcp build
uv build cue-mcp
```
