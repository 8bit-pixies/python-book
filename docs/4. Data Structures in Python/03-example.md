---
title: An Example Program using Pydantic
---

# An Example Program using Pydantic

To understand how we can leverage these data models, let's write a program that calculates the area of a rectangle. We'll start with variables and gradually refactor the program.

We'll begin by creating a new project with Poetry called _rectangles_ that will take the width and height of a rectangle specified in pixels and calculate the area.

Filename:`rectangles/__main__.py`

```py
def area(width: float, height: float) -> float:
    return width * height

width: float = 30
height: float = 50

print(f"The area of the rectange is {area(width, height)} square pixels.")
```

The `area` function is supposed to calculate the area of one rectangle, but the function we wrote has two parameters. The parameters are related, but that’s not expressed anywhere in our program. It would be more readable and more manageable to group width and height together.

## Refactoring with Pydantic: Adding more Meaning

Filename:`rectangles/__main__.py`

```py
from pydantic import BaseModel


class Rectangle(BaseModel):
    width: float
    height: float


def area(rect: Rectangle) -> float:
    return rect.width * rect.height

rect = Rectangle(width=30, height=50)

print(f"The area of the rectangle is {area(rect)} square pixels.")
print(f"Our Rectangle is a data object defined with:\n\tRectangle({rect})")
```

Here we have defined a data class object named `Rectangle`, which has the width and height defined. We can further extend this example by changing `area` to be a method

## Defining methods in Python

Let's change the `area` function which has `Rectangle` instance as a parameter to be the `area` method as part of the `Rectangle` object itself.

Filename:`rectangles/__main__.py`

```py
from pydantic import BaseModel


class Rectangle(BaseModel):
    width: float
    height: float

    def area(self) -> float:
        return self.width * self.height


rect = Rectangle(width=30, height=50)

print(f"The area of the rectangle is {rect.area()} square pixels.")
print(f"Our Rectangle is a data object defined with:\n\tRectangle({rect})")
```

To define a method, we use whitespace indent a method which allows it to be associated to the `Rectangle` object. We accomplish this by moving the `area` function and change the signature to be `self` in the signature and everywhere else within the the `area` method. Then we can use the _method syntax_ to call the `area` method on our `Rectangle` instance. The method syntax goes after an instance: we add a dot followed by the method name, parentheses, and any arguments.

The main benefit of using methods instead of functions, in addition to using method syntax and not having to repeat the type of `self` in every method’s signature, is for organization.

Next we can add further validation to the `Rectangle` object, which ensures `width` and `height` upon instantiation is always non-zero.

Filename:`rectangles/__main__.py`

```py
from pydantic import BaseModel, validator


class Rectangle(BaseModel):
    width: float
    height: float

    @validator('width', 'height')
    def check_positive_size(cls, v):
        if v <= 0:
            raise ValueError("Width and height of a rectange needs to be a positive number")
        return v

    def area(self) -> float:
        return self.width * self.height


rect = Rectangle(width=30, height=50)

print(f"The area of the rectangle is {rect.area()} square pixels.")
print(f"Our Rectangle is a data object defined with:\n\tRectangle({rect})")
```

You can try changing width or height to be zero or less, which should then raise a `ValueError`, which demonstrates the runtime validation which is enabled as part of Pydantic.

# Methods with Parameters

Let's implement an additional method to the `Rectangle` object, which takes another instance of `Rectangle` and returns `True` if the second `Rectangle` can fit completely within `self`; otherwise it should return `false`.

Filename:`rectangles/__main__.py`

```py
from pydantic import BaseModel, validator
from typing import Any

class Rectangle(BaseModel):
    width: float
    height: float

    @validator('width', 'height')
    def check_positive_size(cls, v):
        if v <= 0:
            raise ValueError("Width and height of a rectange needs to be a positive number")
        return v

    def area(self) -> float:
        return self.width * self.height

    def can_hold(self, rect: Any) -> bool:
        return self.width > rect.width and self.height > rect.height


rect = Rectangle(width=30, height=50)
rect2 = Rectangle(width=20, height=40)

print(f"Can rect hold rect2? {rect.can_hold(rect2)}")
```

Unfortunately in the current versions of Python, type annotations won't work in the way we might expect. That is if the method signature resembles

```py
    def can_hold(self, rect: Rectangle) -> bool:
        return self.width > rect.width and self.height > rect.height
```

Python would complain, to somewhat resolve this I've replaced it with `Any`. It is possible with some refactoring to get around this issue, but it may sacrifice readability.

# Using `classmethod`

Class method can be used if we wish to construct or use methods associated with the object itself. They can be used as a constructor. An example is if we wish to make it easier to create a square from `Rectangle` rather than specifying the same value twice

Filename:`rectanges/__main__.py`

```py
from pydantic import BaseModel, validator


class Rectangle(BaseModel):
    width: float
    height: float

    @validator('width', 'height')
    def check_positive_size(cls, v):
        if v <= 0:
            raise ValueError("Width and height of a rectange needs to be a positive number")
        return v

    @classmethod
    def square(cls, size: float):
        return cls(width=size, height=size)

    def area(self) -> float:
        return self.width * self.height


rect = Rectangle.square(30)

print(f"The area of the rectangle is {rect.area()} square pixels.")
print(f"Our Rectangle is a data object defined with:\n\tRectangle({rect})")
```

As we observe here we can now call the `square` method without `Rectangle` being instantiated.

# Summary

Uisng Pydantic allows you to create meaningful data models for your domain that include run-time validation. This can enable your code to be clear.
