# Contributing to Blamodex Dev Agents

Thank you for considering contributing to Blamodex Dev Agents! This document provides guidelines for contributions.

## How to Contribute

### Reporting Issues

- Check existing issues before creating a new one
- Provide clear, detailed descriptions
- Include relevant context (Laravel version, AI tool used, etc.)

### Suggesting Enhancements

- Use the issue tracker to suggest new agents or improvements
- Explain the use case and expected behavior
- Provide examples if possible

### Pull Requests

1. **Fork the repository**
2. **Create a feature branch** (`git checkout -b feature/amazing-feature`)
3. **Follow the existing structure**:
   - Agent files go in `agents/`
   - Examples go in `examples/`
   - Keep Markdown formatting consistent
4. **Test your changes** with real AI assistants (Claude, etc.)
5. **Commit your changes** (`git commit -m 'Add amazing feature'`)
6. **Push to the branch** (`git push origin feature/amazing-feature`)
7. **Open a Pull Request**

## Agent File Guidelines

When creating or modifying agent files:

### Structure
- Use clear headings with emoji markers (🧠, 🧭, 📝, etc.)
- Include these sections:
  - **Role & Context**
  - **Purpose**
  - **Operating Principles**
  - **Output Format**
  - **Example Invocation**
  - **What NOT to do**

### Content
- Be explicit and directive (these are instruction manuals for AI)
- Provide concrete examples
- Emphasize safety and incremental changes
- Reference project conventions (PSR-12, Laravel structure, etc.)

### Formatting
- Use standard Markdown
- Keep lines under 80-100 characters where reasonable
- Use code blocks with proper syntax highlighting
- Use lists for clarity

## Documentation Guidelines

- Keep examples realistic and complete
- Test examples in actual projects before submitting
- Update the main README.md if adding new agents
- Update CHANGELOG.md with your changes

## Code of Conduct

- Be respectful and inclusive
- Focus on constructive feedback
- Help others learn and improve

## Questions?

Open an issue with the `question` label or reach out to the maintainers.

## License

By contributing, you agree that your contributions will be licensed under the MIT License.
