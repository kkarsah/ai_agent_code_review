# AI Code Reviewer 🤖

An intelligent GitHub Action that automatically reviews pull requests using advanced pattern analysis to identify security vulnerabilities, performance issues, and code quality problems.

## Features ✨

### 🔍 **Comprehensive Code Analysis**
- **Security Analysis**: Detects SQL injection, XSS vulnerabilities, hardcoded credentials, unsafe code execution
- **Performance Optimization**: Identifies inefficient loops, expensive DOM operations, database query issues
- **Code Quality**: Catches debug statements, TODO comments, long lines, and language-specific anti-patterns

### 🎯 **Smart File Processing**
- **Multi-language Support**: Python, JavaScript/TypeScript, Java, C/C++, PHP, Ruby, Go, Rust, Swift, Kotlin, SQL, YAML, JSON, HTML/CSS, Shell scripts, Terraform, Markdown
- **Intelligent Filtering**: Automatically skips generated files, dependencies, and binary files
- **Large File Handling**: Handles big PRs with pagination and size limits

### 📊 **Rich Reporting**
- **Severity Levels**: Critical errors, warnings, and suggestions
- **Category Breakdown**: Issues grouped by security, performance, style, logic, and maintainability
- **Actionable Feedback**: Clear explanations with specific recommendations

## Quick Setup 🚀

### 1. Create Workflow File

Create `.github/workflows/ai-code-review.yml` in your repository:

```yaml
name: AI Code Reviewer

on:
  pull_request:
    types: [opened, synchronize, reopened]

jobs:
  ai-code-review:
    name: AI Code Reviewer
    runs-on: ubuntu-latest
    
    if: |
      github.event.pull_request.draft == false && 
      !contains(github.event.pull_request.title, '[skip-ai-review]') &&
      github.actor != 'dependabot[bot]'
    
    permissions:
      contents: read
      pull-requests: write
      issues: write
      checks: write
    
    steps:
    - uses: actions/checkout@v4
    - uses: actions/setup-python@v5
      with:
        python-version: '3.12'
        
    # ... (rest of workflow from the main file)
```

### 2. Enable Permissions

Ensure your repository has the following permissions enabled:
- **Contents**: Read access to repository files
- **Pull Requests**: Write access to comment on PRs
- **Issues**: Write access for issue comments
- **Checks**: Write access for check runs

### 3. That's It! 🎉

The action will automatically run on every pull request and provide detailed code review comments.

## Usage Examples 📝

### Security Issues Detected
```
🚨 Security Risk: Use of eval() can lead to code injection vulnerabilities. 
Consider safer alternatives.

🔑 Security: Possible hardcoded credential detected. 
Use environment variables or secure credential storage.
```

### Performance Warnings
```
⚡ Performance: DOM queries in loops are expensive. 
Cache element references outside the loop.

🐼 Performance: iterrows() is slow. 
Consider vectorized operations or apply() methods.
```

### Code Quality Suggestions
```
🐛 Code Quality: Debug statement 'console.log' found. 
Remove before production deployment.

⚖️ Code Quality: Use strict equality (===) instead of loose equality (==).
```

## Configuration Options ⚙️

### Skip Reviews
Add `[skip-ai-review]` to your PR title to skip automated review:
```
feat: add new feature [skip-ai-review]
```

### Supported File Types
The reviewer automatically analyzes these file extensions:
- **Languages**: `.py`, `.js`, `.ts`, `.jsx`, `.tsx`, `.java`, `.c`, `.cpp`, `.cs`, `.php`, `.rb`, `.go`, `.rs`, `.swift`, `.kt`, `.scala`, `.r`
- **Data/Config**: `.sql`, `.yaml`, `.yml`, `.json`, `.xml`, `.html`, `.css`, `.scss`
- **Scripts**: `.sh`, `.bash`, `.ps1`, `.dockerfile`, `.tf`, `.hcl`
- **Documentation**: `.md`

### Automatically Skipped Files
- Generated files (`node_modules/`, `dist/`, `build/`)
- Lock files (`package-lock.json`, `yarn.lock`, `poetry.lock`)
- Minified files (`.min.js`, `.min.css`)
- Large files (>1500 changes)
- Binary files

## Review Categories 📋

| Category | Description | Examples |
|----------|-------------|----------|
| 🔒 **Security** | Potential vulnerabilities | SQL injection, XSS, hardcoded secrets |
| ⚡ **Performance** | Efficiency issues | Slow loops, expensive operations |
| 🎨 **Style** | Code formatting | Long lines, debug statements |
| 🧠 **Logic** | Code correctness | Exception handling, type checking |
| 🔧 **Maintainability** | Code clarity | TODO comments, complex functions |

## Advanced Features 🔧

### Rate Limit Handling
- Automatic retry logic with exponential backoff
- Respects GitHub API rate limits
- Graceful degradation on API errors

### Error Recovery
- Continues review even if individual files fail
- Posts error comments with debugging information
- Comprehensive logging for troubleshooting

### Scalability
- Handles large PRs with pagination
- Processes up to 50 pages of files
- Intelligent file filtering to focus on reviewable code

## Troubleshooting 🔧

### Common Issues

**Permission Denied (403)**
- Ensure your repository has proper permissions configured
- Check that `GITHUB_TOKEN` has required scopes

**No Files Reviewed**
- Check if files have supported extensions
- Verify files aren't in excluded directories
- Look for `[skip-ai-review]` in PR title

**Rate Limited**
- The action automatically handles rate limits
- Large repositories may experience delays

### Debug Information
Check the Actions tab in your repository for detailed logs including:
- Files processed and skipped
- API calls made
- Errors encountered
- Review statistics

## Contributing 🤝

This AI Code Reviewer uses pattern-based analysis to provide immediate feedback without external AI services. It's designed to be:

- **Fast**: No API calls to external services
- **Private**: All analysis happens in your GitHub Actions
- **Reliable**: Works consistently without external dependencies
- **Customizable**: Easy to extend with new patterns

## License 📄

This project is open source and available under standard GitHub Actions terms.

## Support 💬

- 📖 Check the workflow logs for detailed debugging information
- 🐛 Issues are automatically reported in PR comments
- 🔄 Re-run reviews by closing and reopening the PR

---

**💡 Pro Tip**: The AI reviewer learns from common patterns and can catch issues that human reviewers might miss. Use it as a first line of defense, but always apply human judgment for final decisions!
