# Contributing Guidelines

Thank you for your interest in contributing to DOTAI (iOS Development Assistant)! This document provides guidelines for contributing to this AI-agent focused iOS development tool.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How to Contribute](#how-to-contribute)
- [Project Structure](#project-structure)
- [Documentation Guidelines](#documentation-guidelines)
- [Command Creation](#command-creation)
- [Pull Request Process](#pull-request-process)

## Code of Conduct

Please read and follow our [Code of Conduct](CODE_OF_CONDUCT.md). We expect all contributors to adhere to these guidelines to maintain a positive and inclusive community.

## How to Contribute

1. **Fork the Repository**

   - Create your own fork of the project
   - Clone it to your local machine

2. **Create a Branch**

   - Create a new branch for your contribution
   - Use a descriptive name: `feature/your-feature` or `fix/your-fix`

3. **Make Changes**

   - Follow the project structure
   - Create or modify command files using the `@` prefix
   - Update documentation as needed
   - Add architecture guides if applicable

4. **Commit Your Changes**

   - Use clear and descriptive commit messages
   - Follow conventional commit format:

     ```
     type(scope): description

     [optional body]
     [optional footer]
     ```

   - Types: feat, fix, docs, style, refactor, test, chore

## Project Structure

When contributing, respect the following folder structure:

1. **Architecture Guides** (`/architectures`)

   - Create new architecture guide files (e.g., MVVM, VIPER)
   - Follow existing guide formats
   - Include SwiftUI and UIKit variants

2. **Commands** (`/commands`)

   - Create new command files with `@` prefix
   - Include detailed explanations and examples
   - Follow existing command format

3. **Guidelines** (`/guidelines`)

   - Add development and documentation guidelines
   - Focus on iOS-specific best practices

4. **Libraries** (`/libraries`)
   - Document third-party library integration
   - Include implementation examples
   - Provide configuration guidelines

Note: Avoid modifying the `codex`, `coding-instructions`, and `status` folders as they are reserved for AI agent use.

## Documentation Guidelines

- Follow the [documentation guidelines](commands/documentation.md)
- Use markdown format for all documentation
- Include code examples where applicable
- Keep documentation up to date with changes
- Update the [CHANGELOG.md](CHANGELOG.md) for notable changes

## Command Creation

When creating new commands:

1. Use the `@` prefix (e.g., `@new-command.md`)
2. Include clear description and purpose
3. Provide usage examples
4. Document parameters and expected outcomes
5. Place in the appropriate folder structure

Example command structure:

```markdown
# @command-name

## Description

Detailed description of the command's purpose

## Usage

@command-name [parameters]

## Examples

[Include practical examples]

## Parameters

- param1: description
- param2: description

## Notes

Additional information or considerations
```

## Pull Request Process

1. **Before Submitting**

   - Test your changes with AI agents (Cursor AI, GitHub Copilot)
   - Update relevant documentation
   - Review folder structure compliance
   - Update CHANGELOG.md

2. **Submitting**

   - Create a Pull Request to the `main` branch
   - Fill out the PR template completely
   - Link any related issues
   - Describe AI agent compatibility

3. **PR Review**

   - Address review comments
   - Make requested changes
   - Maintain a respectful dialogue

4. **After Merge**
   - Delete your branch
   - Update your fork

## Questions?

If you have questions, feel free to:

- Open an issue
- Contact the maintainers
- Check existing documentation
- Review the command documentation

Thank you for contributing to DOTAI! Your efforts help improve iOS development workflows with AI assistance.
