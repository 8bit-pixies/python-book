---
title: Data Types
---

Python is a dynamically typed language, though we can still apply type hints to our variables as shown in the previous section.

# Simple Types

Here are examples of some common built-in types:

| Type     | Description                                           |
| -------- | ----------------------------------------------------- |
| `int`    | integer                                               |
| `float`  | floating point number                                 |
| `bool`   | boolean value (subclass of int)                       |
| `str`    | string (unicode in Python 3)                          |
| `bytes`  | 8-bit string                                          |
| `object` | an arbitrary object (object is the common base class) |

Within the standard library there is not the concept of precision within Python, though this is typeically included and extended within the `numpy` libraries and other scientific computing frameworks.

Similar to Rust, `mypy` can infer what type we wanted to provide and provide checks to ensure it adheres to our expectation. For example the code below would fail, even without type hints when running `mypy`.

Filename: `variables/__main__.py`

```py
s = 1
s = "hello"
```

```console
$ poetry run mypy variables
variables/__main__.py:2: error: Incompatible types in assignment (expression has type "str", variable has type "int")
Found 1 error in 1 file (checked 2 source files)
```

Though we can modify this to take `Any` hint which will allow this!

Filename: `variables/__main__.py`

```py
from typing import Any

s: Any = 1
s = "hello"
```

```console
$ poetry run mypy variables
Success: no issues found in 2 source files
```

However as it is a static checker, it does not necessarily assert whether the actual outcome is what we expect. For example, just because we provide a type hint as `int`, it offers no guarentees that the variable in the Python program reflects that type!

Filename: `variables/__main__.py`

```py
s: float = 1
print(f"Variable is of type: {type(s)}")
```

```console
$ poetry run mypy variables
Success: no issues found in 2 source files

$ poetry run python -m variables
Variable is of type: <class 'int'>
```

# Compound Types

Compound types can group multiple values into one type. Python has a few key ones, around `Dict`, `List`, `Set`, `Tuple` types, which also have generic variation in the form `Iterable`, `Sequence`, `Mapping`. These generally operate in the way you might expect.

## The Tuple Type

A tuple is a general way of grouping together a number of values with a variety of types into one compound type. Tuples have a fixed length: once declared, they cannot grow or shrink in size.

We create a tuple by writing a comma-separated list of values inside parentheses. Each position in the tuple has a type, and the types of the different values in the tuple don’t have to be the same. We’ve added optional type annotations in this example:

Filename: `variables/__main__.py`

```py
from typing import Tuple

tup: Tuple[int, float, str] = (1, 2.5, "Hello, world!")

# we can use pattern matching to deconstruct the tuple
x, y, z = tup
print(f"The value of y is: {y}")
```

## The List Type

Another way in Python to have a collection of multiple values is to use a _list_, which is known in other programming languages as an _array_. In Python, a list can also be appropriately typed, we can mix types together using `Union`

Filename: `variables/__main__.py`

```py
from typing import List, Union

a: List[Union[float, str]] = [1.0, 2.5, "Hello, world!"]
```

In Python, lists are 0-indexed and can be accessed as so:

```py
from typing import List

a: List[int] = [1, 2, 3, 4, 5]
print(f"The first element is {a[0]}")
print(f"The second last element is {a[-2]}")
```

## The Dict Type

In Python, a built-in type is _dict_ or dictionary, which is known in other programming languages as a _hash table_. In Python, dictionaries can be typed as well, so that all keys and their corresponding values hold the same type. Dictionary values can be accessed by their keys

Filename: `variables/__main__.py`

```py
from typing import Dict

a: Dict[str, int] = {"hello": 0, "world": 25}
my_key: str = "hello"
print(f"The value for key: {my_key} is: {a[my_key]}")
```

What happens when you try accessing a key which does not exist?

Filename: `variables/__main__.py`

```py
from typing import Dict

a: Dict[str, int] = {"hello": 0, "world": 25}
my_key: str = "invalid"
print(f"The value for key: {my_key} is: {a[my_key]}")
# KeyError: 'invalid'
```

Python also provides a convenient `get` method to access values in a dictionary with the optional _value_ when the key is not present.

Filename: `variables/__main__.py`

```py
from typing import Dict

a: Dict[str, int] = {"hello": 0, "world": 25}
my_key: str = "invalid"
print(f"The value for key: {my_key} is: {a.get(my_key, -1)}")
```

This demonstrates very broadly basic data types in Python, and how we can use `mypy` for static type checking.
