# Installation Guide

This guide provides detailed installation instructions for cc-sessions-drupal.

## Prerequisites

### Required

1. **cc-sessions v0.3.0 or higher**
   ```bash
   # Check if installed
   cc-sessions --version

   # Install if needed
   pipx install cc-sessions
   # or
   npx cc-sessions
   ```

2. **Drupal 10 or 11 project**
   - With Composer-based structure
   - Web root typically at `web/` or `docroot/`

3. **PHP 8.1 or higher**
   ```bash
   php --version
   ```

4. **Composer**
   ```bash
   composer --version
   ```

### Recommended

1. **Drush 12+**
   ```bash
   drush --version
   ```

2. **Development environment** (choose one):
   - **ddev** (recommended)
   - **Lando**
   - **Docker**
   - **Native LAMP/LEMP stack**

3. **PHP CodeSniffer with Drupal standards**
   ```bash
   composer require --dev drupal/coder
   composer require --dev dealerdirect/phpcodesniffer-composer-installer
   ```

4. **Behat** (for functional testing)
   ```bash
   composer require --dev drupal/drupal-extension:^4.2
   ```

## Installation Methods

### Method 1: Quick Install (Recommended)

**For Python users:**
```bash
# Navigate to your Drupal project
cd /path/to/your/drupal/project

# Install cc-sessions-drupal
pipx install cc-sessions-drupal

# Verify installation
ls sessions/templates/task-drupal/
```

**For JavaScript/npm users:**
```bash
# Navigate to your Drupal project
cd /path/to/your/drupal/project

# Install cc-sessions-drupal
npx cc-sessions-drupal

# Verify installation
ls sessions/templates/task-drupal/
```

### Method 2: From Source

```bash
# Clone the repository
git clone https://github.com/gkastanis/cc-sessions-drupal.git /tmp/cc-sessions-drupal

# Navigate to your Drupal project
cd /path/to/your/drupal/project

# Run installation script
bash /tmp/cc-sessions-drupal/install.sh
```

### Method 3: Development Installation

If you're developing or customizing cc-sessions-drupal:

```bash
# Clone to your preferred location
git clone https://github.com/gkastanis/cc-sessions-drupal.git ~/projects/cc-sessions-drupal

# Navigate to your Drupal project
cd /path/to/your/drupal/project

# Set environment variable
export CLAUDE_PROJECT_DIR=$(pwd)

# Run installation
bash ~/projects/cc-sessions-drupal/install.sh
```

## Post-Installation

### 1. Verify Installation

Check that all components were installed:

```bash
# Check templates (5 files)
ls -la sessions/templates/task-drupal/
# Expected: module-feature.md, theme-component.md, content-architecture.md,
#           migration.md, config-management.md

# Check commands (5 files)
ls -la sessions/commands/drupal/
# Expected: phpcs.md, security.md, config-export.md, cache-clear.md, behat.md

# Check protocols (1 file)
ls sessions/protocols/drupal-quality-gate.md

# Check agents (2 files)
ls sessions/agents/drupal-*.md

# Check state
cat sessions/sessions-state.json | grep -A 10 '"drupal"'

# Check config
cat sessions/sessions-config.json | grep -A 15 '"drupal"'
```

### 2. Configure Drupal Settings

Edit `sessions/sessions-config.json` to match your environment:

```json
{
  "drupal": {
    "version": "11",
    "phpcs_path": "./vendor/bin/phpcs",
    "phpcs_standard": "Drupal,DrupalPractice",
    "config_export_mode": "warn",
    "behat_prompt": true,
    "behat_command": "ddev robo behat",
    "drush_command": "ddev drush",
    "quality_gates": {
      "phpcs": true,
      "security": true,
      "config_check": true,
      "behat": false
    }
  }
}
```

**Customization options:**

**For Drupal 10:**
```json
"version": "10"
```

**For Lando:**
```json
"drush_command": "lando drush",
"behat_command": "lando behat"
```

**For native install (no container):**
```json
"drush_command": "./vendor/bin/drush",
"behat_command": "./vendor/bin/behat"
```

**Custom PHPCS location:**
```json
"phpcs_path": "/usr/local/bin/phpcs"
```

### 3. Test Installation

Create a test task to verify everything works:

```bash
# In Claude Code, say:
mek: @drupal-m-test-installation

# Claude should load the module-feature template
# Verify you see the Drupal-specific structure
```

Test a slash command:

```bash
# In Claude Code, type:
/drupal/phpcs

# Should execute phpcs check
```

## Environment-Specific Setup

### ddev

**Recommended configuration:**
```json
{
  "drupal": {
    "drush_command": "ddev drush",
    "behat_command": "ddev robo behat",
    "phpcs_path": "./vendor/bin/phpcs"
  }
}
```

**Test commands:**
```bash
ddev drush status
ddev exec ./vendor/bin/phpcs --version
```

### Lando

**Recommended configuration:**
```json
{
  "drupal": {
    "drush_command": "lando drush",
    "behat_command": "lando behat",
    "phpcs_path": "./vendor/bin/phpcs"
  }
}
```

**Test commands:**
```bash
lando drush status
lando php ./vendor/bin/phpcs --version
```

### Docker Compose

**Recommended configuration:**
```json
{
  "drupal": {
    "drush_command": "docker-compose exec php drush",
    "behat_command": "docker-compose exec php vendor/bin/behat",
    "phpcs_path": "./vendor/bin/phpcs"
  }
}
```

### Native Install

**Recommended configuration:**
```json
{
  "drupal": {
    "drush_command": "./vendor/bin/drush",
    "behat_command": "./vendor/bin/behat",
    "phpcs_path": "./vendor/bin/phpcs"
  }
}
```

## Upgrading

### From Git (Development)

```bash
cd ~/projects/cc-sessions-drupal
git pull origin main
cd /path/to/your/drupal/project
bash ~/projects/cc-sessions-drupal/install.sh
```

### From PyPI

```bash
pipx upgrade cc-sessions-drupal
```

### From npm

```bash
npx cc-sessions-drupal@latest
```

## Uninstalling

To remove cc-sessions-drupal:

```bash
# Remove files
rm -rf sessions/templates/task-drupal/
rm -rf sessions/commands/drupal/
rm sessions/protocols/drupal-quality-gate.md
rm sessions/agents/drupal-architect.md
rm sessions/agents/drupal-security-review.md
rm -rf sessions/extensions/drupal/

# Remove configuration (manual edit)
# Edit sessions/sessions-config.json and remove "drupal" section

# Remove state (manual edit)
# Edit sessions/sessions-state.json and remove "drupal" section
```

## Troubleshooting Installation

### Error: cc-sessions not found

**Problem:** cc-sessions is not installed

**Solution:**
```bash
# Install cc-sessions first
pipx install cc-sessions
# or
npx cc-sessions

# Then install cc-sessions-drupal
pipx install cc-sessions-drupal
```

### Error: sessions directory not found

**Problem:** Not in a project with cc-sessions initialized

**Solution:**
```bash
# Initialize cc-sessions first
cc-sessions-install

# Then install cc-sessions-drupal
pipx install cc-sessions-drupal
```

### Error: Permission denied

**Problem:** Insufficient permissions to write to sessions directory

**Solution:**
```bash
# Check directory permissions
ls -la sessions/

# Fix if needed (be careful with permissions)
chmod u+w sessions/
chmod u+w sessions/templates/
chmod u+w sessions/commands/
```

### Error: Python/Node version too old

**Problem:** Python < 3.8 or Node < 14

**Solution:**
```bash
# Check versions
python3 --version
node --version

# Upgrade as needed (method depends on OS)
```

### Drupal config not added

**Problem:** Installation completed but no Drupal section in config

**Solution:**
```bash
# Manually add to sessions/sessions-config.json
{
  "drupal": {
    "version": "11",
    "phpcs_path": "./vendor/bin/phpcs",
    "phpcs_standard": "Drupal,DrupalPractice",
    "config_export_mode": "warn",
    "behat_prompt": true,
    "behat_command": "ddev robo behat",
    "drush_command": "ddev drush",
    "quality_gates": {
      "phpcs": true,
      "security": true,
      "config_check": true,
      "behat": false
    }
  }
}
```

## Next Steps

After successful installation:

1. **Configure for your environment** - Update drush/behat commands
2. **Create your first Drupal task** - `mek: @drupal-m-test-feature`
3. **Test quality gates** - Complete a task and observe automatic checks
4. **Try slash commands** - Run `/drupal/phpcs`
5. **Explore templates** - Review `sessions/templates/task-drupal/`

## Getting Help

- **Documentation:** [README.md](README.md)
- **Issues:** [GitHub Issues](https://github.com/gkastanis/cc-sessions-drupal/issues)
- **Discussions:** [GitHub Discussions](https://github.com/gkastanis/cc-sessions-drupal/discussions)
