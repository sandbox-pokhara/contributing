# Python Guidelines

## Strict Typing

Strict typing using pyright is compulsory. Add pyright section in `pyproject.toml`

```toml
[tool.pyright]
typeCheckingMode = "strict"
exclude = ["**/venv"]
```

## Fomatting

Preferred formatter for python is `black`.

```toml
[tool.black]
line-length = 79
preview = true
```

## Import Sorting

Preferred import sort tool for python is `isort`. We use single line for imports because it reduces the line length and makes the code pretty.

```toml
[tool.isort]
line_length = 79
force_single_line = true
```
