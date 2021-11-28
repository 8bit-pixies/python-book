---
title: Comments and Docstrings
---

# Comments

All programmers strive to make their code easy to understand, but sometimes extra explanation is warranted. In these cases, programmers leave notes, or comments, in their source code that the compiler will ignore but people reading the source code may find useful.

Here’s a simple comment:

```py
# hello, world
```

In Python, the idiomatic comment style starts a comment with a single hash character `#`, and the comment continues until the end of the line. Comments can be placed at the end of lines containing code:

Filename: `variables/__main__.py`

```py
lucky_number = 7  # I'm feeling lucky today
```

Comments can be used for much more, including type checking in `mypy`, as shown below

Filename: `variables/__main__.py`

```py
lucky_number = 7  # type: int
```

Multi-line comments can be represented by triple quotations, though by convention these are typically used as `docstrings` and documentation within a Python file.

Filename: `variables/__main__.py`

```py
def say_hello(x: str):
    """
    Says hello

    Parameters
    ----------
    x : str
        Says hello to {x}
    """
    print(f"Hello {x}")

print(say_hello.__doc__)
```

```console
$ poetry run python -m variables

    Says hello

    Parameters
    ----------
    x : str
        Says hello to {x}
```

These can also appear at the module level at the top of a file! To demonstrate this we will alter `__init__.py` file instead

Filename: `variables/__init__.py`

```py
"""
This is a docstring!
"""

__version__ = '0.1.0'
```

Filename: `variables/__main__.py`

```py
import variables;

print(variables.__doc__)
```

```console
$ poetry run python -m variables

This is a docstring!
```

Docstrings actually have a very lengthy [PEP guideline](https://www.python.org/dev/peps/pep-0257/) which is worth considering as part of Python development.
