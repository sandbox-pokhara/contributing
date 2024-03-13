# Git Guidelines

## Repository Name

Use kebab case. Example: `example-repo`

## Commit Message

We follow conventional commits (https://www.conventionalcommits.org/en/v1.0.0/) guidelines for commit messages.

### Commit Descrption

The commit descrption must follow the following guidelines:

- English
- Present tense
- Imperative
- Lowercase

### Commit Types

| Commit Type | Title                    | Description                                                                                                 |
| ----------- | ------------------------ | ----------------------------------------------------------------------------------------------------------- |
| `feat`      | Features                 | A new feature                                                                                               |
| `fix`       | Bug Fixes                | A bug Fix                                                                                                   |
| `docs`      | Documentation            | Documentation only changes                                                                                  |
| `style`     | Styles                   | Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)      |
| `refactor`  | Code Refactoring         | A code change that neither fixes a bug nor adds a feature                                                   |
| `perf`      | Performance Improvements | A code change that improves performance                                                                     |
| `test`      | Tests                    | Adding missing tests or correcting existing tests                                                           |
| `build`     | Builds                   | Changes that affect the build system or external dependencies (example scopes: gulp, broccoli, npm)         |
| `ci`        | Continuous Integrations  | Changes to our CI configuration files and scripts (example scopes: Travis, Circle, BrowserStack, SauceLabs) |
| `chore`     | Chores                   | Other changes that don't modify src or test files                                                           |
| `revert`    | Reverts                  | Reverts a previous commit                                                                                   |

### Examples

- feat: add example feature
- fix: fix a example issue
- docs: add documentation for example feature
- style: add missing new line
- style: change code formatter to black
- style: sort imports in example module
- refactor: improve an example feature
- refactor: rename tool.py to utils.py
- refactor: remove example feature
- refactor: remove utils.py
- pref: improve the performance of example algorithm
- test: add test case for example module
- ci: add deploy github workflow
- chore: bump version to 1.0.8
- chore: add example file to gitignore
- chore: add example package to requirements
