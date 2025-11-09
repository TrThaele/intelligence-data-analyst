# Contributing to CloudDora Data Insights Platform

Thank you for your interest in contributing to CloudDora! This document provides guidelines and instructions for contributing.

## 🎯 Ways to Contribute

- 🐛 Report bugs
- 💡 Suggest new features
- 📝 Improve documentation
- 🔧 Submit bug fixes
- ✨ Add new features
- 🧪 Write tests
- 🎨 Improve UI/UX

## 🚀 Getting Started

### 1. Fork the Repository
Click the "Fork" button at the top right of the repository page.

### 2. Clone Your Fork
```bash
git clone https://github.com/T Thaele/clouddora-platform.git
cd clouddora-platform
```

### 3. Add Upstream Remote
```bash
git remote add upstream https://github.com/clouddora/clouddora-platform.git
```

### 4. Create a Branch
```bash
git checkout -b feature/your-feature-name
# or
git checkout -b fix/your-bug-fix
```

## 📝 Development Guidelines

### Code Style

**Python**
- Follow [PEP 8](https://www.python.org/dev/peps/pep-0008/) style guide
- Use meaningful variable and function names
- Add docstrings to functions and classes
- Maximum line length: 100 characters

**JavaScript**
- Use ES6+ features
- Use `const` and `let`, avoid `var`
- Use semicolons
- Prefer arrow functions for callbacks

**HTML/CSS**
- Use semantic HTML5 elements
- Follow BEM naming convention for CSS classes
- Keep CSS modular and reusable

### Commit Messages

Follow the [Conventional Commits](https://www.conventionalcommits.org/) specification:

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting, etc.)
- `refactor`: Code refactoring
- `test`: Adding or updating tests
- `chore`: Maintenance tasks

**Examples:**
```
feat(api): add rate limiting to upload endpoint

fix(ui): resolve file upload progress bar issue

docs(readme): update deployment instructions
```

### Testing

- Write unit tests for new functions
- Ensure all tests pass before submitting PR
- Aim for >80% code coverage

```bash
# Run tests
python -m pytest tests/

# With coverage
python -m pytest --cov=lambda tests/
```

## 🔄 Pull Request Process

### 1. Update Your Fork
```bash
git fetch upstream
git rebase upstream/main
```

### 2. Make Your Changes
- Write clean, readable code
- Add tests for new features
- Update documentation if needed
- Follow the code style guidelines

### 3. Test Your Changes
```bash
# Run all tests
python -m pytest tests/

# Test locally with SAM (if available)
sam local invoke UploadHandler --event events/upload-event.json
```

### 4. Commit Your Changes
```bash
git add .
git commit -m "feat: add your feature description"
```

### 5. Push to Your Fork
```bash
git push origin feature/your-feature-name
```

### 6. Create Pull Request
- Go to the original repository
- Click "New Pull Request"
- Select your fork and branch
- Fill out the PR template
- Link any related issues

### PR Checklist
- [ ] Code follows project style guidelines
- [ ] Tests added/updated and passing
- [ ] Documentation updated
- [ ] Commit messages follow conventions
- [ ] No merge conflicts
- [ ] PR description is clear and complete

## 🐛 Bug Reports

When reporting bugs, please include:

1. **Description**: Clear description of the bug
2. **Steps to Reproduce**: Detailed steps to reproduce
3. **Expected Behavior**: What should happen
4. **Actual Behavior**: What actually happens
5. **Environment**: 
   - AWS Region
   - Python version
   - Browser (if UI bug)
6. **Screenshots**: If applicable
7. **Logs**: Relevant error logs or CloudWatch logs

**Use this template:**
```markdown
## Bug Description
[Clear description]

## Steps to Reproduce
1. Step one
2. Step two
3. Step three

## Expected Behavior
[What should happen]

## Actual Behavior
[What actually happens]

## Environment
- AWS Region: eu-central-1
- Python: 3.11
- Browser: Chrome 120

## Screenshots
[If applicable]

## Logs
```
[Paste relevant logs]
```
```

## 💡 Feature Requests

When suggesting features, please include:

1. **Problem Statement**: What problem does this solve?
2. **Proposed Solution**: How should it work?
3. **Alternatives**: Other solutions you've considered
4. **Additional Context**: Any other relevant information

## 📚 Documentation

Improvements to documentation are always welcome!

- Fix typos or unclear explanations
- Add examples
- Improve getting started guide
- Add troubleshooting tips
- Create tutorials

## 🏗️ Architecture Changes

For significant architectural changes:

1. Open an issue first to discuss
2. Wait for maintainer approval
3. Create a design document if needed
4. Submit PR with implementation

## 🧪 Testing Guidelines

### Unit Tests
- Test individual functions in isolation
- Mock external dependencies (AWS services)
- Use descriptive test names

```python
def test_upload_handler_validates_file_size():
    """Test that upload handler rejects files over 10MB"""
    # Test implementation
```

### Integration Tests
- Test Lambda functions end-to-end
- Use LocalStack for local AWS testing
- Test API Gateway endpoints

### Manual Testing
1. Deploy to test environment
2. Test via UI
3. Check CloudWatch logs
4. Verify data in S3/DynamoDB

## 📋 Code Review Process

All PRs require review before merging:

1. **Automated Checks**: Must pass all CI/CD checks
2. **Code Review**: At least one maintainer approval
3. **Testing**: All tests must pass
4. **Documentation**: Docs updated if needed

### Review Timeline
- Small fixes: 1-2 days
- Features: 3-5 days
- Major changes: 1-2 weeks

## 🚫 What NOT to Commit

- AWS credentials or API keys
- Large binary files
- Generated files (*.pyc, node_modules/, etc.)
- Personal configuration files
- Secrets or sensitive data

## 🎓 Learning Resources

New to the technologies used? Check these out:

- [AWS Lambda](https://docs.aws.amazon.com/lambda/)
- [Amazon Bedrock](https://docs.aws.amazon.com/bedrock/)
- [API Gateway](https://docs.aws.amazon.com/apigateway/)
- [DynamoDB](https://docs.aws.amazon.com/dynamodb/)
- [Serverless Architectures](https://aws.amazon.com/serverless/)

## 🤝 Community

- Be respectful and inclusive
- Help others learn
- Give constructive feedback
- Assume good intentions

## 📞 Questions?

- Open an issue for questions
- Tag maintainers with @mention
- Join our community discussions

## 🙏 Recognition

Contributors will be:
- Listed in CONTRIBUTORS.md
- Mentioned in release notes
- Credited in documentation

Thank you for contributing to CloudDora! 🎉
