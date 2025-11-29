# Blamodex Dev Agents

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Laravel](https://img.shields.io/badge/Laravel-12-red.svg)](https://laravel.com)
[![Packagist](https://img.shields.io/badge/Packagist-Ready-blue.svg)](https://packagist.org/)

A set of Markdown-based "agent manuals" for using LLMs (like Claude Code, GitHub Copilot, or ChatGPT) as structured development assistants.

These agents provide a systematic workflow for building, testing, debugging, and maintaining code with AI assistance.

## 🎯 Designed For

- **Laravel 12** (PHP 8.2+)
- **React / TypeScript** (Vite)
- **PSR-12** coding standards
- **Test-first / test-friendly** workflows

## 📦 Installation

### Option 1: Composer (Recommended)

```bash
composer require --dev blamodex/laravel-ai-agents
```

**Optional: Create a symlink to access agents from your project root**

```bash
ln -s vendor/blamodex/laravel-ai-agents/agents agents
```

This allows you to reference agents as `/agents/DevAgent.md` instead of `/vendor/blamodex/laravel-ai-agents/agents/DevAgent.md`.

### Option 2: Manual Installation

Copy the `agents/` folder into the root of your project:

```bash
mkdir -p agents
cp -r vendor/blamodex/laravel-ai-agents/agents/* ./agents/
```

### Option 3: Git Submodule

```bash
git submodule add https://github.com/blamodex/laravel-ai-agents.git agents
```

## Included Agents

All agent definitions live under `./agents/`:

- `DevAgent.md` – multi-file feature and refactor agent
- `BugAgent.md` – diagnostic and debugging agent
- `TestAgent.md` – test design and generation agent
- `CleanupAgent.md` – non-behavioral cleanup and documentation agent
- `WORKFLOW.md` – how to combine them in a Request → Agent → Test → Refine loop

## How to Use (Claude Code / VS Code)

1. **Install the package** (see Installation above)
2. **Reference agents in your prompts**:

   ```
   Use DevAgent.
   Follow the instructions in /agents/DevAgent.md.
   Goal: Implement rate limiting on all /api/* routes in a safe, incremental way.
   ```

3. **Follow the workflow** in `agents/WORKFLOW.md`:
   - Start with **DevAgent** (plan + first step)
   - Use **TestAgent** to generate tests if needed
   - Use **BugAgent** when tests fail or errors appear
   - Use **CleanupAgent** at the end for polish and documentation

## 📚 Documentation

- **[Workflow Guide](agents/WORKFLOW.md)** - Complete agent workflow loop
- **[Laravel Usage Examples](examples/laravel-usage.md)** - Concrete Laravel examples
- **[React Usage Examples](examples/react-usage.md)** - React + TypeScript examples
- **[Contributing](CONTRIBUTING.md)** - How to contribute
- **[Changelog](CHANGELOG.md)** - Version history

## Recommended Project Conventions

These agents assume (and work best with):

- Laravel:
  - Feature tests in `tests/Feature`
  - Unit tests in `tests/Unit`
  - Prefer factories and seeders over ad-hoc model creation
- Frontend:
  - React + TypeScript under something like `resources/js`
  - Jest + React Testing Library for component tests
- Tooling:
  - Composer scripts such as:

    ```
    composer test
    composer test:coverage
    composer lint
    composer lint:fix
    composer analyze
    ```

You can adapt the Markdown files to match your own project conventions.

## Customizing

Feel free to fork or copy these agent files and customize them for your project:

- Adjust command names (`composer test` → `npm test`, etc.)
- Modify directory structure expectations
- Add project-specific conventions
- Create new specialized agents (DeployAgent, DocAgent, etc.)

## 🤝 Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for details.

## 📄 License

MIT License - see [LICENSE](LICENSE) for details.

## 🙏 Acknowledgments

Built for teams using AI-assisted development workflows with tools like:
- [Claude Code (Anthropic)](https://www.anthropic.com/)
- [GitHub Copilot](https://github.com/features/copilot)
- [ChatGPT](https://openai.com/chatgpt)

## 🔗 Links

- [Report a Bug](https://github.com/blamodex/laravel-ai-agents/issues)
- [Request a Feature](https://github.com/blamodex/laravel-ai-agents/issues)
- [View Changelog](CHANGELOG.md)