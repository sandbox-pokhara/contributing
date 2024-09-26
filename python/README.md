# Python Guidelines

## Strict Typing

Strict typing using pyright is compulsory. Add pyright section in `pyproject.toml`

```toml
[tool.pyright]
typeCheckingMode = "strict"
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

## Docstring

```py
"""
A description of the function

@param1: value
@param2: value
@param3: value
@param4: value
"""
```

Example:

```py
class User:
    def __init__(self, username:str, password:str):
        """
        A class for a user

        @username: The name of the user
        @password: A strong password for the user
        """
        self.username = username
        self.password = password
```
