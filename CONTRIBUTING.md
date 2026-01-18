# Contributing to Ralph Wiggum Docker Loop

Thank you for your interest in contributing! This document provides guidelines for contributing to the project.

## How to Contribute

### Reporting Bugs

1. Check existing [issues](https://github.com/jodagreyhame/ralph-wiggum-docker/issues) to avoid duplicates
2. Create a new issue with:
   - Clear, descriptive title
   - Steps to reproduce
   - Expected vs actual behavior
   - Environment details (OS, Docker version, auth mode)
   - Relevant logs from `logs/` directory

### Suggesting Features

1. Open an issue with the `enhancement` label
2. Describe the use case and proposed solution
3. Be open to discussion and alternative approaches

### Pull Requests

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/my-feature`
3. Make your changes
4. Test with the Docker test suite: `./test/test-all.sh`
5. Commit with clear messages (see below)
6. Push and open a PR against `main`

## Development Setup

```bash
# Clone the repo
git clone https://github.com/jodagreyhame/ralph-wiggum-docker.git
cd ralph-wiggum-docker

# Copy environment template
cp env.template .env

# Build Docker image
docker compose build

# Build CLI
bun install && bun run build

# Create a test project
ralph new test-project --preset=minimal

# Run tests
.claude/skills/docker-test/scripts/test-all.sh
```

## Code Style

### General Rules

- Keep files under 300 lines
- Keep functions under 50 lines
- No hardcoded secrets - use environment variables
- No TODO comments - fix it or create an issue
- No mock data - use real implementations
- Fail loudly with clear error messages

### Shell Scripts

- Use `set -euo pipefail` for bash scripts
- Quote variables: `"$VAR"` not `$VAR`
- Use `shellcheck` for linting

### Python

- Follow PEP 8
- Use type hints where practical
- Run `ruff` for linting

### Commit Messages

**IMPORTANT:** This project uses [Conventional Commits](https://www.conventionalcommits.org/) for automated versioning and changelog generation.

```
<type>(<scope>): <description>

[optional body]
```

#### Types (required)

| Type | Description | Version Bump |
|------|-------------|--------------|
| `feat` | New feature | Minor (0.X.0) |
| `fix` | Bug fix | Patch (0.0.X) |
| `docs` | Documentation only | Patch |
| `refactor` | Code change (no feature/fix) | Patch |
| `perf` | Performance improvement | Patch |
| `test` | Adding/updating tests | No release |
| `build` | Build system changes | No release |
| `ci` | CI configuration | No release |
| `chore` | Maintenance tasks | No release |

#### Breaking Changes

Add `!` after type or include `BREAKING CHANGE:` in body for major version bump:

```
feat!: Remove deprecated API endpoint
```

#### Examples

```bash
feat: Add provider fallback support
feat(cli): Add --preset flag to new command
fix: Resolve null pointer in config parser
fix(docker): Mount permissions on Windows
docs: Update Quick Start guide
refactor(lib): Extract logging utilities
```

#### Automated Releases

- PRs are validated for conventional commit format
- Merges to `main` trigger automatic version bumps based on commit types
- Changelog is auto-generated from commit messages

## Project Structure

```
ralph-wiggum-docker-loop/
├── docker/              # Docker infrastructure
│   ├── Dockerfile       # Main image
│   ├── entrypoint.sh    # Container startup
│   ├── ralph.sh         # Main loop script
│   └── cli/             # CLI backend configs
├── template/            # Project template
├── scripts/             # Launcher scripts
├── .claude/skills/      # Claude Code skills
└── .projects/           # User projects (gitignored)
```

## Testing

Before submitting a PR:

1. Run the test suite:
   ```bash
   # Linux/Mac
   ./test/test-all.sh

   # Windows
   .\test\test-all.ps1

   # CLI tests
   ./test/cli/test-cli.sh
   ```

2. Test with at least one auth mode (passthrough recommended)

3. Verify Docker builds successfully: `docker compose build`

## Questions?

Open an issue or start a discussion. We're happy to help!
