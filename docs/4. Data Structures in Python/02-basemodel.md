---
title: Getting Started with Pydantic
---

To get started let's create a new project called structures `poetry new structures`, and introduce `pydantic` as a way to provide run-time type validation. We can then install `pydantic` using Poetry.

```console
$ poetry add mypy pydantic
Creating virtualenv
Using version ^0.910 for mypy
Using version ^1.8.2 for pydantic

Updating dependencies
Resolving dependencies... (3.6s)

Writing lock file

Package operations: 13 installs, 0 updates, 0 removals

  • Installing pyparsing (3.0.6)
  • Installing attrs (21.2.0)
  • Installing more-itertools (8.12.0)
  • Installing mypy-extensions (0.4.3)
  • Installing packaging (21.3)
  • Installing pluggy (0.13.1)
  • Installing py (1.11.0)
  • Installing toml (0.10.2)
  • Installing typing-extensions (4.0.0)
  • Installing wcwidth (0.2.5)
  • Installing mypy (0.910)
  • Installing pydantic (1.8.2)
  • Installing pytest (5.4.3)
```

We can also include the [pydantic mypy plugin](https://pydantic-docs.helpmanual.io/mypy_plugin/). We can do this by adding the following to `pyproject.toml`

Filename:`pyproject.toml`

```toml
(..snip..)

[mypy]
plugins = pydantic.mypy
```

# Defining and Instantiating Data Objects

There are many patterns for packaging related variables in Python, including the used of dictionaries or named tuples. If the input data is can be well-structured, Pydantic offers `BaseModel`, as a way to define these structures and enable the pieces of data to be grouped together. More specifically Pydantic offers additional support to provide run-time type validation to Python.

```py
from pydantic import BaseModel

class User(BaseModel):
    active: bool
    username: str
    email: str
    sign_in_count: int
```

We can create an _instance_ of this data model by specifying concrete values for each of the fields. An instance is created by provided the name of the data model, and providing the key, value pairs as parameters and their corresponding values. For example, we can declare a particular user based on above as follows:

```py
user1 = User(
    email="someone@example.com",
    username="somerusername123",
    active=True,
    sign_in_count=1
)
```

As this is just a Python object, we can change the value of an attribute and they are mutable as we expect:

```py
user1 = User(
    email="someone@example.com",
    username="somerusername123",
    active=True,
    sign_in_count=1
)

user1.email = "anotheremail@example.com"
```

When using `BaseModel` data model objects can be compared, and sorted, whereas using Python objects, this is not possible with writing additional code.

Filename:`structures/__main__.py`

```py
from pydantic import BaseModel


class User(BaseModel):
    active: bool
    username: str
    email: str
    sign_in_count: int


class UserModel(object):
    def __init__(self, active: bool, username: str, email: str, sign_in_count: int):
        self.active = active
        self.username = username
        self.email = email
        self.sign_in_count = sign_in_count


user1 = User(
    email="someone@example.com",
    username="somerusername123",
    active=True,
    sign_in_count=1,
)

user2 = User(
    email="someone@example.com",
    username="somerusername123",
    active=True,
    sign_in_count=1,
)

print(f"user1 and user2 - are they equal? {user1 == user2}")

user1 = UserModel(
    email="someone@example.com",
    username="somerusername123",
    active=True,
    sign_in_count=1,
)

user2 = UserModel(
    email="someone@example.com",
    username="somerusername123",
    active=True,
    sign_in_count=1,
)

print(f"user1 and user2 - are they equal? {user1 == user2}")

```

If we run the above code, we can see when using the appropriate dataclass, the objects can be compared, whereas when using vanilla Python objects

```console
$ poetry run python -m structures
user1 and user2 - are they equal? True
user1 and user2 - are they equal? False
```

Another big difference in using Pydantic is runtime validation. If as a user the incorrect data type is provided, the code will error out at runtime.

Filename:`structures/__main__.py`

```py
from pydantic import BaseModel


class User(BaseModel):
    active: bool
    username: str
    email: str
    sign_in_count: int

user1 = User(
    email="someone@example.com",
    username="somerusername123",
    active=True,
    sign_in_count="one",
)

print(user1)
```

Then when you run the resulting code will raise an error:

```console
$ poetry run python -m structures
Traceback (most recent call last):
  (..snip..)
  File "~/projects/structures/structures/__main__.py", line 10, in <module>
    user1 = User(
  File "pydantic/main.py", line 406, in pydantic.main.BaseModel.__init__
pydantic.error_wrappers.ValidationError: 1 validation error for User
sign_in_count
  value is not a valid integer (type=type_error.integer)
```

With minimal effect we can provide pythonic code that provides runtime validation!
