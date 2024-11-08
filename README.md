# DOTAI - iOS Development Assistant

This project is a helper tool for the improving the productivity, qualtity, performance, and maintainability of iOS development. This project spesifically prepared for Coding AI Agents like Cursor AI or GitHub Copilot. Every command is designed to be used with the `@` prefix.

## Current Commands

- [@learn.md](commands/learn.md) - Learns new things based on the session. When user asks to learn something, this command will be called. In this way, the AI agent can learn new things from the session. And will remember them for future sessions.
- [@codex.md](commands/codex.md) - Review codex. This command will be called when user asks to review the codex. Not suggested to use this command in the middle of the session. Please use this command at the end of the session. Check [end-session.md](commands/end-session.md) for more information.
- [@documentation.md](commands/documentation.md) - Generate documentation for the project.
- [@split-codex.md](commands/split-codex.md) - Split codex into smaller files.
- [@start-session.md](commands/start-session.md) - Start a new session.

  Example Usage:

  ```
  @start-session.md We will implement a new feature called "Task List". Use `@MVVM-SwiftUI.md` as a architecture guide. Start with splitting this project phases. And create a new feature folder.
  ```

- [@end-session.md](commands/end-session.md) - End session.

  Example Usage:

  ```
  @end-session.md
  ```

## Available Architecture Guides

### SwiftUI

- [@MVVM-SwiftUI.md](architectures/MVVM-SwiftUI.md) - MVVM architecture guide for SwiftUI.
- [@VIPER-SwiftUI.md](architectures/VIPER-SwiftUI.md) - VIPER architecture guide for SwiftUI.

### UIKit

- [@MVVM-UIKit.md](architectures/MVVM-UIKit.md) - MVVM architecture guide for UIKit.
- [@VIPER-UIKit.md](architectures/VIPER-UIKit.md) - VIPER architecture guide for UIKit.

## Folder Structure

### Architecture Guides

You can create a new architecture guide file for your project. This will help the AI agent to understand the architecture and follow the guidelines.

### Codex

Don't use this folder. This folder is only for the AI agent. Agent will create a new file when learn something or fail to do something.

### Commands

You can create a new command file for your project. Be specific and use technical terms. Explain the command in detail. Give examples. Always enhance the command with new features.

## Libraries (Third Party)

This project is currently not using any third party libraries. But if you want to use, you can create a new file in this folder. Give examples. Explain the library in detail.

    Example:
        - [@Alamofire.md](libraries/Alamofire.md) - Alamofire library guide.
        - [@Firebase.md](libraries/Firebase.md) - Firebase library guide.

## Guidelines

You can create a guideline file for your project. This will help the AI agent to understand the project and follow the guidelines.

- [documentation.md](guidelines/documentation.md) - Documentation guidelines.

## Coding Instructions

Don't use this folder. This folder is only for the AI agent. Agent will create a new file when get a new coding instruction.

## Status

Don't use this folder. This folder is used to store the status of the project. Agent will create a new file when start a new session. And update the file when end a session.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Author

[@ohk](https://github.com/ohk)

## Contributing

We welcome contributions to improve the project. Please read the [CONTRIBUTING](CONTRIBUTING.md) guide for details.

## Changelog

See the [CHANGELOG](CHANGELOG.md) file for details.
