# Contributing to Roblox Update Tracker

First off, thank you for considering contributing to Roblox Update Tracker! 🎉

## How Can I Contribute?

### Reporting Bugs

Before creating bug reports, please check the existing issues to avoid duplicates. When you create a bug report, include as many details as possible:

- **Use a clear and descriptive title**
- **Describe the exact steps to reproduce the problem**
- **Provide specific examples** (version numbers, queries, etc.)
- **Include error messages or logs**
- **Describe the expected vs actual behavior**

### Suggesting Enhancements

Enhancement suggestions are tracked as GitHub issues. When creating an enhancement suggestion:

- **Use a clear and descriptive title**
- **Provide a detailed description** of the suggested enhancement
- **Explain why this enhancement would be useful**
- **List examples** of how the feature would be used

### Pull Requests

1. Fork the repo and create your branch from `main`
2. If you've added code, test it with multiple Roblox versions
3. Update documentation (`README.md`, `SKILL.md`) if needed
4. Ensure your code follows the existing style
5. Write a clear commit message describing your changes

## Development Guidelines

### File Structure

```
roblox-update-tracker/
├── SKILL.md                    # Main skill logic
├── api-keywords/               # API declarations
│   ├── luau-language.md
│   ├── api-classes.md
│   └── api-enums.md
├── version-history.md          # Historical versions
├── config.example.json         # Config template
└── README.md
```

### Code Style

- Use clear, descriptive variable names
- Comment complex logic sections
- Follow existing patterns in `SKILL.md`
- Keep functions focused and modular

### Testing

Before submitting a PR, test your changes with:
- At least 2 different Roblox versions
- Both "quick check" and "full analysis" modes
- Various configuration options

### API Keywords Maintenance

When updating `api-keywords/*.md`:
- Keep alphabetical order
- Verify against official Roblox API documentation
- Include version information in comments
- Remove deprecated APIs with version notes

### Documentation

- Update `README.md` for user-facing changes
- Update `SKILL.md` for logic/workflow changes
- Add entries to `CHANGELOG.md` following Keep a Changelog format
- Include code examples for new features

## Community

- Be respectful and welcoming
- Help others in issues and discussions
- Share your Roblox update analysis insights
- Suggest improvements to documentation

## Questions?

Feel free to open a GitHub issue with the `question` label, or reach out via:
- OpenClaw Community Discord
- GitHub Discussions

Thank you for your contributions! 🙏
