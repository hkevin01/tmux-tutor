# Contributing to Tmux Mastery Lab

Thank you for your interest in contributing to Tmux Mastery Lab! This document provides guidelines and instructions for contributing.

## 🤝 How to Contribute

### Reporting Bugs

1. Check if the bug has already been reported in [Issues](https://github.com/yourusername/tmux-tutor/issues)
2. If not, create a new issue using the Bug Report template
3. Provide detailed information:
   - OS and version
   - Tmux version
   - Shell type and version
   - Steps to reproduce
   - Expected vs actual behavior
   - Relevant logs or screenshots

### Suggesting Features

1. Check [existing feature requests](https://github.com/yourusername/tmux-tutor/issues?q=is%3Aissue+label%3Aenhancement)
2. Create a new issue using the Feature Request template
3. Clearly describe:
   - The problem it solves
   - Your proposed solution
   - Alternative approaches considered
   - Willingness to implement

### Submitting Pull Requests

1. **Fork the repository**
2. **Create a feature branch**: `git checkout -b feature/your-feature-name`
3. **Make your changes** following our coding standards
4. **Write or update tests**
5. **Update documentation** as needed
6. **Commit your changes** using conventional commits
7. **Push to your fork**: `git push origin feature/your-feature-name`
8. **Open a Pull Request** using the PR template

## 📝 Coding Standards

### Bash Scripts

```bash
# Use strict mode
#!/usr/bin/env bash
set -euo pipefail

# Function naming: snake_case
function my_function() {
  local variable_name="value"
  
  # Error handling
  if ! command_that_might_fail; then
    echo "ERROR: Descriptive error message" >&2
    return 1
  fi
  
  # Logging with timestamps
  echo "[$(date +'%Y-%m-%d %H:%M:%S')] INFO: Operation completed"
}

# Use shellcheck
# shellcheck disable=SC2086 # Only when absolutely necessary with explanation
```

**Naming Conventions**:
- Functions: `snake_case`
- Local variables: `snake_case`
- Environment variables: `UPPER_SNAKE_CASE`
- Constants: `UPPER_SNAKE_CASE`

### Python Code

```python
#!/usr/bin/env python3
"""Module docstring describing purpose.

This module provides...
"""

from typing import Optional, List
import logging

# Configure logging
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)


class MyClass:
    """Class docstring following Google style.
    
    Attributes:
        attribute_name: Description of attribute.
    """
    
    def __init__(self, param: str) -> None:
        """Initialize the class.
        
        Args:
            param: Description of parameter.
        """
        self.attribute_name = param
    
    def my_method(self, arg: int) -> Optional[str]:
        """Method docstring.
        
        Args:
            arg: Description of argument.
            
        Returns:
            Description of return value.
            
        Raises:
            ValueError: When arg is invalid.
        """
        try:
            # Implementation
            logger.info(f"Processing {arg}")
            return str(arg)
        except Exception as e:
            logger.error(f"Error: {e}")
            raise
```

**Naming Conventions**:
- Classes: `PascalCase`
- Functions/methods: `snake_case`
- Variables: `snake_case`
- Constants: `UPPER_SNAKE_CASE`
- Private members: `_leading_underscore`

### YAML Configuration

```yaml
# Use consistent indentation (2 spaces)
session:
  name: "my-session"
  windows:
    - index: 1
      name: "editor"
      panes:
        - index: 1
          command: "vim"
          path: "/workspace"
```

## 🧪 Testing

### Running Tests

```bash
# Run all tests
./scripts/run_tests.sh

# Run specific test file
pytest tests/unit/test_layout_manager.py

# Run with coverage
pytest --cov=src tests/
```

### Writing Tests

```bash
# Bash tests (using bats or shunit2)
#!/usr/bin/env bats

@test "save_layout creates valid YAML" {
  source scripts/utils/save_layout.sh
  
  # Setup
  tmux new-session -d -s test-session
  
  # Execute
  save_layout "test-session" "/tmp/test.yml"
  
  # Assert
  [ -f "/tmp/test.yml" ]
  grep -q "session: \"test-session\"" "/tmp/test.yml"
  
  # Cleanup
  tmux kill-session -t test-session
  rm -f /tmp/test.yml
}
```

```python
# Python tests (using pytest)
import pytest
from src.utils.layout_manager import LayoutManager

def test_save_layout_creates_file(tmp_path):
    """Test that save_layout creates a valid file."""
    # Arrange
    manager = LayoutManager()
    output_file = tmp_path / "layout.yml"
    
    # Act
    result = manager.save_layout("test-session", str(output_file))
    
    # Assert
    assert result is True
    assert output_file.exists()
    assert "session:" in output_file.read_text()
```

## 📚 Documentation

### Code Comments

```bash
# Good: Explains WHY, not WHAT
# Use grep -v to exclude comment lines because they don't affect configuration
grep -v "^#" config.conf

# Bad: Explains obvious WHAT
# Loop through files
for file in *.txt; do
```

### Function Documentation

Every function must have a header comment:

```bash
#######################################
# Saves the current tmux session layout to a YAML file.
#
# This function captures window and pane configurations including
# layout strings, working directories, and running commands.
#
# Globals:
#   TMUX_MASTERY_DIR
# Arguments:
#   $1 - Session name to save
#   $2 - Output YAML file path
# Returns:
#   0 on success, non-zero on error
# Example:
#   save_layout "my-session" "/tmp/layout.yml"
#######################################
function save_layout() {
  local session_name="$1"
  local output_file="$2"
  # Implementation...
}
```

## 🔄 Git Workflow

### Commit Messages

We follow [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

**Types**:
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting, no logic change)
- `refactor`: Code refactoring
- `test`: Adding or updating tests
- `chore`: Maintenance tasks

**Examples**:
```
feat(labs): add advanced hook exercises

Added 5 new exercises covering tmux hooks including session-created
and window-linked events.

Closes #42
```

```
fix(install): handle missing ~/.bashrc gracefully

The installer now checks if ~/.bashrc exists before attempting to
append to it. Falls back to creating the file if needed.

Fixes #38
```

### Branch Naming

- Feature branches: `feature/short-description`
- Bug fixes: `fix/issue-number-description`
- Documentation: `docs/what-changed`
- Refactoring: `refactor/what-changed`

## 🎯 Pull Request Process

1. **Update documentation** for any changed functionality
2. **Add tests** for new features or bug fixes
3. **Run tests locally** and ensure they pass
4. **Update CHANGELOG.md** with your changes
5. **Fill out the PR template** completely
6. **Request review** from maintainers
7. **Address feedback** promptly and professionally
8. **Squash commits** if requested before merge

### PR Checklist

Before submitting, ensure:

- [ ] Code follows project style guidelines
- [ ] All tests pass locally
- [ ] New code has appropriate test coverage
- [ ] Documentation is updated
- [ ] Commit messages follow conventional commits
- [ ] CHANGELOG.md is updated
- [ ] No merge conflicts with main branch
- [ ] Shellcheck passes for bash scripts
- [ ] Pylint/black passes for Python code

## 🐛 Debugging Tips

### Tmux Debugging

```bash
# Enable tmux command logging
tmux set -g @debug on

# Check tmux version
tmux -V

# List all options
tmux show-options -g

# Verify keybinding
tmux list-keys | grep "your-key"
```

### Script Debugging

```bash
# Enable bash debugging
bash -x script.sh

# Or within script
set -x  # Enable
set +x  # Disable

# Check syntax without execution
bash -n script.sh
```

## 📞 Getting Help

- **Questions**: Open a [Discussion](https://github.com/yourusername/tmux-tutor/discussions)
- **Chat**: Join our community channel (link TBD)
- **Documentation**: Check the [docs/](docs/) folder

## 🎖️ Recognition

Contributors will be:
- Listed in CONTRIBUTORS.md
- Mentioned in release notes
- Given credit in documentation

## 📜 Code of Conduct

This project adheres to a Code of Conduct. By participating, you are expected to:

- Be respectful and inclusive
- Accept constructive criticism
- Focus on what's best for the community
- Show empathy towards others

## 📄 License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

Thank you for contributing to Tmux Mastery Lab! 🚀
