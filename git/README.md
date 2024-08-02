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

| Commit Type | Title                    | Modifies Source Code? | Description                                                                                            |
| ----------- | ------------------------ | --------------------- | ------------------------------------------------------------------------------------------------------ |
| `feat`      | Features                 | Yes                   | A new feature                                                                                          |
| `fix`       | Bug Fixes                | Yes                   | A bug fix                                                                                              |
| `refactor`  | Code Refactoring         | Yes                   | A code change that neither fixes a bug nor adds a feature, it changes an existing feature              |
| `style`     | Styles                   | Yes                   | Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc) |
| `perf`      | Performance Improvements | Yes                   | A code change that improves performance                                                                |
| `test`      | Tests                    | Yes                   | Adding missing tests or correcting existing tests                                                      |
| `docs`      | Documentation            | No                    | Documentation only changes                                                                             |
| `chore`     | Chores                   | No                    | Other changes that don't modify src or test files                                                      |

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
- chore: add deploy github workflow
- chore: bump version to 1.0.8
- chore: add example file to gitignore
- chore: add example package to requirements
