# Distribution Guide

This guide explains how to build and publish cc-sessions-drupal packages to PyPI and npm.

## Prerequisites

### For Python Distribution (PyPI)

1. **Python build tools**
   ```bash
   pip install build twine
   ```

2. **PyPI account**
   - Register at https://pypi.org/account/register/
   - Set up API token at https://pypi.org/manage/account/token/

3. **Configure credentials**
   ```bash
   # Create ~/.pypirc
   cat > ~/.pypirc << 'EOF'
   [pypi]
   username = __token__
   password = pypi-YOUR-API-TOKEN-HERE
   EOF
   chmod 600 ~/.pypirc
   ```

### For JavaScript Distribution (npm)

1. **npm account**
   - Register at https://www.npmjs.com/signup

2. **Login to npm**
   ```bash
   npm login
   ```

## Pre-Release Checklist

Before publishing a new version:

- [ ] All tests pass
- [ ] Documentation is up to date
- [ ] CHANGELOG.md updated with new version
- [ ] Version bumped in both `pyproject.toml` and `package.json`
- [ ] Git tag created for version
- [ ] Installation tested locally

## Version Numbering

Follow [Semantic Versioning](https://semver.org/):

- **MAJOR** (X.0.0) - Breaking changes
- **MINOR** (0.X.0) - New features, backward compatible
- **PATCH** (0.0.X) - Bug fixes, backward compatible

## Step-by-Step Release Process

### 1. Update Version Numbers

**Update pyproject.toml:**
```toml
[project]
name = "cc-sessions-drupal"
version = "0.2.0"  # <-- Update this
```

**Update package.json:**
```json
{
  "name": "cc-sessions-drupal",
  "version": "0.2.0",  # <-- Update this
  ...
}
```

### 2. Update CHANGELOG.md

Add new version section:

```markdown
## [0.2.0] - 2025-11-XX

### Added
- New feature 1
- New feature 2

### Changed
- Modified behavior 1

### Fixed
- Bug fix 1

[0.2.0]: https://github.com/gkastanis/cc-sessions-drupal/compare/v0.1.0...v0.2.0
```

### 3. Commit Version Changes

```bash
git add pyproject.toml package.json CHANGELOG.md
git commit -m "chore: bump version to 0.2.0"
```

### 4. Create Git Tag

```bash
git tag -a v0.2.0 -m "Release version 0.2.0"
```

### 5. Build Python Package

```bash
# Clean previous builds
rm -rf dist/ build/ *.egg-info

# Build package
python -m build

# Verify contents
tar -tzf dist/cc-sessions-drupal-0.2.0.tar.gz | head -20
```

**Expected package contents:**
```
cc-sessions-drupal-0.2.0/
├── pyproject.toml
├── README.md
├── LICENSE
├── CHANGELOG.md
├── MANIFEST.in
├── install.sh
├── cc_sessions_drupal/
│   ├── __init__.py
│   ├── python/
│   ├── hooks/
│   └── scripts/
├── javascript/
├── templates/
├── protocols/
├── agents/
├── commands/
└── docs/
```

### 6. Test Python Package Locally

```bash
# Install in test environment
cd /tmp
python -m venv test-env
source test-env/bin/activate
pip install /path/to/cc-sessions-drupal/dist/cc-sessions-drupal-0.2.0.tar.gz

# Test installation
cd /path/to/test/drupal/project
cc-sessions-drupal

# Verify files installed
ls sessions/templates/task-drupal/
```

### 7. Publish to PyPI

**Test on TestPyPI first (recommended):**
```bash
# Upload to TestPyPI
twine upload --repository testpypi dist/*

# Test installation from TestPyPI
pip install --index-url https://test.pypi.org/simple/ cc-sessions-drupal
```

**Publish to production PyPI:**
```bash
twine upload dist/*
```

### 8. Test npm Package

```bash
# Test installation locally
cd /path/to/test/drupal/project
node /path/to/cc-sessions-drupal/javascript/install.js

# Verify it works
ls sessions/templates/task-drupal/
```

### 9. Publish to npm

```bash
# Make sure you're in the package root
cd /path/to/cc-sessions-drupal

# Dry run (see what would be published)
npm publish --dry-run

# Publish to npm
npm publish
```

### 10. Push to GitHub

```bash
# Push commits
git push origin main

# Push tags
git push origin v0.2.0
```

### 11. Create GitHub Release

1. Go to https://github.com/gkastanis/cc-sessions-drupal/releases/new
2. Select tag: `v0.2.0`
3. Release title: `v0.2.0`
4. Description: Copy from CHANGELOG.md
5. Attach dist files (optional)
6. Publish release

## Post-Release

### Verify Installation

**Test Python package:**
```bash
pipx install cc-sessions-drupal
# or
pip install cc-sessions-drupal
```

**Test npm package:**
```bash
npx cc-sessions-drupal
```

### Announce Release

- Update repository README badges if needed
- Post in discussions/announcements
- Notify users who requested features

## Troubleshooting

### PyPI Upload Fails with Authentication Error

**Problem:** `403 Forbidden` or authentication error

**Solution:**
```bash
# Regenerate API token on PyPI
# Update ~/.pypirc with new token
# Retry upload
```

### npm Publish Fails with Permission Error

**Problem:** `EPERM` or `npm ERR! 403 Forbidden`

**Solution:**
```bash
# Check you're logged in
npm whoami

# Login again if needed
npm login

# Check package name isn't taken
npm view cc-sessions-drupal

# Retry publish
```

### Package Contains Wrong Files

**Problem:** Missing files or extra files in package

**Solution for Python:**
```bash
# Check MANIFEST.in
# Rebuild package
python -m build --no-isolation

# Inspect tarball
tar -tzf dist/cc-sessions-drupal-*.tar.gz
```

**Solution for npm:**
```bash
# Check package.json "files" field
# Use npm pack to see what would be published
npm pack
tar -tzf cc-sessions-drupal-*.tgz
```

### Installation Script Fails After Publishing

**Problem:** Users report installation errors

**Solution:**
```bash
# Test in clean environment
cd /tmp
mkdir test-project && cd test-project

# Install from published package
pipx install cc-sessions-drupal
# or
npx cc-sessions-drupal

# Debug and create patch release if needed
```

## Rollback a Release

### Remove from PyPI

**Note:** You cannot re-upload the same version number to PyPI once deleted.

```bash
# Delete release (requires maintainer permissions)
# Use PyPI web interface: https://pypi.org/manage/project/cc-sessions-drupal/releases/

# Or use twine
pip install twine[keyring]
twine delete cc-sessions-drupal==0.2.0
```

### Unpublish from npm

**Within 72 hours:**
```bash
npm unpublish cc-sessions-drupal@0.2.0
```

**After 72 hours:**
- Cannot unpublish
- Publish a patch version with fixes
- Deprecate bad version:
  ```bash
  npm deprecate cc-sessions-drupal@0.2.0 "This version has issues, use 0.2.1 instead"
  ```

## Automated Release (Future Enhancement)

Consider setting up GitHub Actions for automated releases:

```yaml
# .github/workflows/publish.yml
name: Publish Package

on:
  push:
    tags:
      - 'v*'

jobs:
  publish-pypi:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
        with:
          python-version: '3.x'
      - name: Build and publish
        env:
          TWINE_USERNAME: __token__
          TWINE_PASSWORD: ${{ secrets.PYPI_API_TOKEN }}
        run: |
          pip install build twine
          python -m build
          twine upload dist/*

  publish-npm:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '16'
          registry-url: 'https://registry.npmjs.org'
      - name: Publish
        env:
          NODE_AUTH_TOKEN: ${{ secrets.NPM_TOKEN }}
        run: npm publish
```

## Beta/Pre-Release Versions

### Publishing Beta Versions

**Update version with pre-release suffix:**
```json
"version": "0.2.0-beta.1"
```

**Publish as beta:**
```bash
# PyPI
twine upload dist/*

# npm
npm publish --tag beta
```

**Users can install beta:**
```bash
# Python
pip install cc-sessions-drupal==0.2.0b1

# npm
npm install cc-sessions-drupal@beta
```

## Release Cadence

**Recommended schedule:**
- **Patch releases** - As needed for bug fixes
- **Minor releases** - Every 4-6 weeks for new features
- **Major releases** - Only when breaking changes necessary

## Versioning Examples

```
0.1.0 → 0.1.1   # Bug fix (patch)
0.1.1 → 0.2.0   # New feature (minor)
0.2.0 → 1.0.0   # Breaking change (major)
1.0.0 → 1.0.1   # Bug fix
1.0.1 → 1.1.0   # New feature
1.1.0 → 2.0.0   # Breaking change
```

## Support Policy

**Current major version:**
- Active development
- New features
- Bug fixes
- Security updates

**Previous major version:**
- Critical bug fixes only
- Security updates
- 6 months after new major release

**Older versions:**
- No active support
- Community contributions accepted

## Questions?

- Create an issue: https://github.com/gkastanis/cc-sessions-drupal/issues
- Discuss: https://github.com/gkastanis/cc-sessions-drupal/discussions
