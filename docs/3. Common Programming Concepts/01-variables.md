---
title: Variables and Assignment
---

As we begin to explore variables in Python, we'll begin by talking about linting and testing. Although almost everything is permissible in Python, this does not mean we shouldn't include type hints to verify that our code works as intended!

Typing is optional in Python, but using where we can will ensure our code is more precise, and easier for others to read and reason with. At the end of the day, readable programming is for humans not machines.

To get this working, we will introduce `mypy` as a way to provide typing support to Python. To illustrate this, let’s generate a new project called _variables_ in your projects directory by using `poetry new variables`. Next we can install `mypy` using Poetry.

```console
$ poetry add mypy
Creating virtualenv
Using version ^0.910 for mypy

Updating dependencies
Resolving dependencies... (3.5s)

Writing lock file

Package operations: 12 installs, 0 updates, 0 removals
```

Now in the new _variables_ folder, we can once again update `__main__.py`

Filename: `variables/__main__.py`

```py
x: int = 5
print(f"The value of x is: {x}")
x = "hello"
print(f"The value of x is: {x}")
```

What happens if we run this? It will work as expected - which seems a bit underwhelming!

```console
$ poetry run python -m variables
The value of x is: 5
The value of x is: hello
```

Now if we run `mypy` over this, we should get an error instead!

```console
$ poetry run mypy variables
variables/__main__.py:3: error: Incompatible types in assignment (expression has type "str", variable has type "int")
Found 1 error in 1 file (checked 2 source files)
```

This example shows how we can provide static type checking for Python. In Python, there isn't a concept of constants, or separation between immutable or mutable types, instead Python depends on conventions. For example, under [PEP8 guidelines](https://www.python.org/dev/peps/pep-0008/#toc-entry-35) constants are written in capital letters with underscores separating words.
