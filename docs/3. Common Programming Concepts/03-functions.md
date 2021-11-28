---
title: Functions
---

# Functions

Liek many other programming languages, functions are an integral part of Python programming. In Python, functions start with the `def` keyword which allows you to declare new functions.

Python code uses _snake case_ as the convention for function and variable names. In snake case, all letters are lowercase and underscores separate words. Here’s a program that contains an example function definition:

Filename: `variables/__main__.py`

```py
def say_hello():
    print("Hello!")

say_hello()
```

## Function Parameters

Functions can also be defined to have _parameters_, which are special variables that are part of a function's signature. When a function has parameters, you can provide it with concrete values for those parameters. We can add a parameter to our function `say_hello` to let it say hello to someone.

Filename: `variables/__main__.py`

```py
def say_hello(x: str):
    print(f"Hello {x}!")

say_hello("world")
# Hello world!
```

As with type hints, in Python these are optional, we can also use mypy in the way you would expect

Filename: `variables/__main__.py`

```py
def say_hello(x: str):
    print(f"Hello {x}!")

say_hello(123)
```

```console
poetry run mypy variables
variables/__main__.py:4: error: Argument 1 to "say_hello" has incompatible type "int"; expected "str"
Found 1 error in 1 file (checked 2 source files)
```

Functions can also take multiple parameters which are separated by a comma, and we can provide type hints for the output of a function using `->`. To output the return of a function you would need to use the `return` keyword

Filename: `variables/__main__.py`

```py
def five() -> int:
    return 5

print(five())
```

```console
$ poetry run python -m variables
5
```

We can combine the two ideas to have a function with a function signature and also a return type

Filename: `variables/__main__.py`

```py
def add_one(x: int) -> int:
    return x + 1

print(add_one(5))
```

```console
$ poetry run python -m variables
6
```
