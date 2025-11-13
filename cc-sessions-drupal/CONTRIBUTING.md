# Contributing to cc-sessions-drupal

Thank you for your interest in contributing! This document provides guidelines and instructions for contributing.

## Code of Conduct

Be respectful and constructive. We're all here to make Drupal development better.

## How to Contribute

### Reporting Issues

**Before creating an issue:**
1. Search existing issues to avoid duplicates
2. Collect relevant information (versions, error messages, steps to reproduce)

**When creating an issue:**
1. Use a clear, descriptive title
2. Provide steps to reproduce
3. Include expected vs actual behavior
4. Add relevant logs or screenshots
5. Specify your environment:
   - cc-sessions version
   - cc-sessions-drupal version
   - Drupal version
   - PHP version
   - Development environment (ddev, Lando, etc.)

### Suggesting Features

**Feature requests should include:**
1. Clear description of the feature
2. Use case and problem it solves
3. How it fits with existing functionality
4. Any implementation ideas

### Contributing Code

#### Development Setup

**1. Fork and clone:**
```bash
git clone https://github.com/YOUR_USERNAME/cc-sessions-drupal.git
cd cc-sessions-drupal
```

**2. Create a feature branch:**
```bash
git checkout -b feature/your-feature-name
# or
git checkout -b fix/your-bugfix-name
```

**3. Set up test environment:**
```bash
# Navigate to a test Drupal project
cd /path/to/test/drupal/project

# Install cc-sessions-drupal from your development directory
export CLAUDE_PROJECT_DIR=$(pwd)
bash /path/to/cc-sessions-drupal/install.sh
```

#### Code Standards

**Python code:**
- Follow PEP 8 style guide
- Use type hints where appropriate
- Add docstrings to functions and classes
- Keep functions focused and single-purpose

**JavaScript code:**
- Follow Node.js best practices
- Use clear variable names
- Add JSDoc comments
- Maintain feature parity with Python implementation

**Documentation:**
- Use clear, concise language
- Include code examples
- Update README.md if adding features
- Add entries to CHANGELOG.md

**Task Templates:**
- Follow existing template structure
- Include Context Manifest section
- Add Quality Gates checklist
- Provide Work Log sections

**Agents:**
- Specify clear responsibilities
- List required tools
- Provide workflow steps
- Include output format examples

#### Making Changes

**1. Keep changes focused:**
- One feature or fix per pull request
- Don't mix refactoring with feature additions
- Break large changes into smaller PRs

**2. Test your changes:**
```bash
# Test installation
bash install.sh

# Test templates
# Create task with new template

# Test slash commands
# Execute command in Claude Code

# Test quality gates
# Complete a task and verify gates run

# Test both languages if changing code
# Python implementation
# JavaScript implementation
```

**3. Update documentation:**
- README.md for user-facing changes
- INSTALL.md for installation changes
- CHANGELOG.md with your changes
- Code comments for complex logic

**4. Commit your changes:**
```bash
git add .
git commit -m "feat: add new feature"
# or
git commit -m "fix: resolve bug"
```

**Commit message format:**
- `feat:` - New feature
- `fix:` - Bug fix
- `docs:` - Documentation changes
- `refactor:` - Code refactoring
- `test:` - Adding tests
- `chore:` - Maintenance tasks

#### Submitting Pull Requests

**1. Push to your fork:**
```bash
git push origin feature/your-feature-name
```

**2. Create pull request:**
- Go to GitHub and create PR
- Use descriptive title
- Reference related issues
- Describe what changed and why
- Include testing steps

**3. PR description template:**
```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Documentation update
- [ ] Refactoring

## Related Issues
Fixes #123

## Testing
Steps to test the changes:
1. ...
2. ...

## Checklist
- [ ] Code follows project style
- [ ] Self-review completed
- [ ] Documentation updated
- [ ] CHANGELOG.md updated
- [ ] Tested with Python implementation
- [ ] Tested with JavaScript implementation
```

## Development Guidelines

### File Organization

```
cc-sessions-drupal/
├── cc_sessions_drupal/     # Python implementation
│   ├── python/             # Python modules
│   ├── hooks/              # Hook scripts
│   └── scripts/            # Utility scripts
├── javascript/             # JavaScript implementation
│   ├── hooks/              # Hook scripts
│   └── scripts/            # Utility scripts
├── templates/              # Task templates
│   └── task-drupal/        # Drupal-specific templates
├── protocols/              # Protocol definitions
├── agents/                 # Specialized agents
├── commands/               # Slash commands
│   └── drupal/             # Drupal commands
└── docs/                   # Documentation
```

### Adding New Templates

**1. Create template file:**
```bash
# In templates/task-drupal/
touch my-new-template.md
```

**2. Follow template structure:**
```markdown
# Task: @{task-name}

## Task Metadata
- **Type**: Description
- **Priority**: {priority}

## Drupal Context
[Drupal-specific information]

## Context Manifest
[Files and dependencies]

## Work Log
[Session tracking]

## Completion Checklist
[Quality gates and requirements]
```

**3. Update task detector:**
```python
# In cc_sessions_drupal/python/task_detector.py
TASK_PATTERNS = {
    DrupalTaskType.NEW_TYPE: r'^@?drupal-x-',
    # ... existing patterns
}
```

**4. Update documentation:**
- Add to README.md under "Task Templates"
- Include naming pattern
- Describe use case

### Adding New Slash Commands

**1. Create command file:**
```bash
# In commands/drupal/
touch my-command.md
```

**2. Command structure:**
```markdown
# /drupal/my-command

Brief description of what this command does.

## Usage

\`\`\`bash
/drupal/my-command [options]
\`\`\`

## Implementation

\`\`\`bash
{drush_command} my-drush-command
\`\`\`

## Output

Expected output description
```

**3. Test command:**
- Type `/drupal/my-command` in Claude Code
- Verify execution
- Check output formatting

### Adding New Agents

**1. Create agent file:**
```bash
# In agents/
touch drupal-new-agent.md
```

**2. Agent structure:**
```markdown
# Drupal New Agent

## Role
Brief description

## Responsibilities
- Task 1
- Task 2

## Available Tools
- Read
- Bash
- etc.

## Workflow
1. Step 1
2. Step 2

## Output Format
Expected deliverables
```

### Maintaining Feature Parity

When modifying code that exists in both Python and JavaScript:

**1. Make changes to both:**
- Update Python version
- Update JavaScript version
- Verify identical behavior

**2. Test both implementations:**
```bash
# Test Python
python3 cc_sessions_drupal/python/task_detector.py

# Test JavaScript
node javascript/task-detector.js
```

## Testing

### Manual Testing Checklist

**Installation:**
- [ ] Install from Python package
- [ ] Install from npm package
- [ ] Install from source
- [ ] Verify all files copied
- [ ] Check configuration added
- [ ] Check state initialized

**Task Templates:**
- [ ] Create task with each template type
- [ ] Verify template structure
- [ ] Check frontmatter populated

**Slash Commands:**
- [ ] Execute each command
- [ ] Verify output
- [ ] Test with arguments
- [ ] Check error handling

**Quality Gates:**
- [ ] Complete task triggers gates
- [ ] PHPCS runs and reports
- [ ] Security scan executes
- [ ] Config check works
- [ ] Behat prompt appears

**Agents:**
- [ ] Invoke each agent
- [ ] Verify tools available
- [ ] Check output format

### Environment Testing

Test with different setups:
- [ ] ddev
- [ ] Lando
- [ ] Docker Compose
- [ ] Native install
- [ ] Drupal 10
- [ ] Drupal 11
- [ ] Python 3.8+
- [ ] Node.js 14+

## Release Process

### Preparing a Release

**1. Update version:**
- `pyproject.toml`
- `package.json`
- `CHANGELOG.md`

**2. Update CHANGELOG:**
```markdown
## [X.Y.Z] - YYYY-MM-DD

### Added
- New features

### Changed
- Modifications

### Fixed
- Bug fixes
```

**3. Create release commit:**
```bash
git commit -m "chore: release v0.X.Y"
git tag v0.X.Y
```

**4. Push release:**
```bash
git push origin main
git push origin v0.X.Y
```

**5. Publish packages:**
```bash
# Python
python -m build
twine upload dist/*

# npm
npm publish
```

**6. Create GitHub release:**
- Use tag v0.X.Y
- Copy CHANGELOG section
- Add release notes

## Getting Help

- **Questions:** [GitHub Discussions](https://github.com/gkastanis/cc-sessions-drupal/discussions)
- **Bugs:** [GitHub Issues](https://github.com/gkastanis/cc-sessions-drupal/issues)
- **Email:** zorz@example.com

## Recognition

Contributors will be recognized in:
- README.md contributors section
- Release notes
- CHANGELOG.md (for significant contributions)

Thank you for contributing to cc-sessions-drupal!
