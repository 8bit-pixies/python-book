---
title: Control Flow
---

# Control Flow

Deciding whether or not to run some code depending on if a condition is true and deciding to run some code repeatedly while a condition is true are basic building blocks in most programming languages. The most common constructs that let you control the flow of execution of Python code are `if` expressions and loops.

## `if` Expressions

An if expression allows you to branch your code depending on conditions. You provide a condition and then state, “If this condition is met, run this block of code. If the condition is not met, do not run this block of code.”

Create a new project called `branches` in your projects directory to explore the if expression. In the `branches/__main.py__` file, input the following:

Filename: `branches/__main__.py`

```py
number = 3

if number < 5:
    print("condition was true")
else:
    print("condition was false")
```

All `if` expressions start with the keyword `if`, which is followed by a condition. In this case, the condition checks whether or not the variable `number` has a value less than 5. The block of code we want to execute if the condition is true is placed within the same indented code block.

Optionally, we can also include an `else` expression, which we chose to do here, to give the program an alternative block of code to execute should the condition evaluate to false. If you don’t provide an `else` expression and the condition is false, the program will just skip the `if` block and move on to the next bit of code.

Let's see this in action

```console
$ poetry run python -m branches
condition was true
```

If we change the value of `number` to a value that makes a condition false, let's see what happens

```py
number = 7
```

Run the program again and look at the output

```console
$ poetry run python -m branches
condition was false
```

In Python, the conditions need not be boolean! Python only requires "truthy" or "falsy" values, this is performed by Python casting things via `bool` automatically.

| Type    | Truthy                   | Falsy   |
| ------- | ------------------------ | ------- |
| `int`   | Any non-zero             | `0`     |
| `float` | Any non-zero             | `0.0`   |
| `bool`  | `true`                   | `false` |
| `str`   | Any non-empty string     | `""`    |
| `list`  | Any non-empty list       | `[]`    |
| `tuple` | Any non-empty tuple      | `()`    |
| `set`   | Any non-empty set        | `set()` |
| `dict`  | Any non-empty dictionary | `{}`    |

We can observe this in the following simple example

```py
number = 3

if number:
    print(f"The number: {number}, is non-zero")
```

## Handling Multiple Conditions with `elif`

You can have multiple conditions by combining `if` and `else` in an `elif` expression. For example:

Filename: `branches/__main__.py`

```py
number = 6

if number % 4 == 0:
    print("number is divisible by 4")
elif number % 3 == 0:
    print("number is divisible by 3")
elif number % 2 == 0:
    print("number is divisible by 2")
else:
    print("number is not divisible by 4, 3, or 2")
```

This program has four possible paths it can take. After running it, you should see the following output:

```console
$ poetry run python -m branches
number is divisible by 3
```

When this program executes, it checks each `if` expression in turn and executes the first body for which the condition holds true. Note that even though 6 is divisible by 2, we don’t see the output `number is divisible by 2`, nor do we see the `number is not divisible by 4, 3, or 2` text from the `else` block. That’s because Python only executes the block for the first true condition, and once it finds one, it doesn’t even check the rest.

Python also provides support for a ternary operator, which is shown below

Filename: `branches/__main__.py`

```py
condition = True
number = 5 if condition else 6

print(f"The value of number is: {number}")
```

Notice that `mypy` can still be applied in this setting as we might expect.

Filename: `branches/__main__.py`

```py
condition: bool = True
number: int = 5 if condition else 6

print(f"The value of number is: {number}")
```

```console
$ poetry run mypy branches
Success: no issues found in 2 source files
```

## Repetition with Loops

It’s often useful to execute a block of code more than once. For this task, Rust provides several loops. A loop runs through the code inside the loop body to the end and then starts immediately back at the beginning. To experiment with loops, let’s make a new project called loops.

Python has two kinds of loops: `for`, `while`. Let's have a look at each one.

## Repeating Code with `while True:`

If we use the statement `while True:`, it tells Python to execute the code block over and over again forever or until you tell it to stop. Loops can also use two special keywords, being `continue` which tells the program to skip over reamining code and proceed to the next iteration, and `break` which is to cease execution of the loop code block and continue. Having loops within loops would only affect the innermost loop.

In Python, there does not exist a mechanism to break out of multiple loops at once, instead various design patterns can be used to accomplish this instead.

Filename: `loops/__main__.py`

```py
count: int = 0

while True:
    if count == 5:
        break
    count += 1
    print(f"count = {count}")
```

```console
$ poetry run python -m loops
count = 1
count = 2
count = 3
count = 4
count = 5
```

## Conditional Loops with `while`

A better pattern for managing loops is to enable conditional loops, we saw a variation of this in the guessing game, which was used in conjunction with the walrus operator. We can use a program which loops three times, counting down each time and printing a message before exiting.

Filename:`loops/__main__.py`

```py
number = 3
while number != 0:
    print(f"{number}!")
    number -= 1
print("LIFTOFF!!!")
```

```console
$ poetry run python -m loops
3!
2!
1!
LIFTOFF!!!
```

Notice that logically this is equivalent to the `while...else` pattern which was introduced in the guessing game shown below

Filename:`loops/__main__.py`

```py
number = 3
while number != 0:
    print(f"{number}!")
    number -= 1
else:
    print("LIFTOFF!!!")
```

## Looping Through a Collection with `for`

Another way is to use the `for` loop which can be used over a collection.

Filename:`loops/__main__.py`

```py
from typing import List

a: List[int] = [10, 20, 30, 40, 50]
index: int = 0

while index < 5:
    print(f"The value at index: {index} is: {a[index]}")
    index += 1
```

Here the code counts up through the elements in the array starting from index `0` until it reaches the final index in the array.

```console
$ poetry run  python -m loops
The value at index: 0 is: 10
The value at index: 1 is: 20
The value at index: 2 is: 30
The value at index: 3 is: 40
The value at index: 4 is: 50
```

This approach is problematic, as we have to know the test condition or the index value. A more concise alternative is to use a `for` loop

Filename:`loops/__main__.py`

```py
from typing import List

a: List[int] = [10, 20, 30, 40, 50]

for element in a:
    print(f"The value is {element}")
```

If we need to keep the index, we use `enumerate` with the `for` loop

Filename:`loops/__main__.py`

```py
from typing import List

a: List[int] = [10, 20, 30, 40, 50]

for index, element in enumerate(a):
    print(f"The value at index: {index} is: {element}")
```

Not only is this code more concise and produces the same output as the example using `while` loop, it is safer, as we have eliminated the chance of bugs that result of going beyond the end of the list.

We can use `for` loops on collections which are not lists as well. For example over dictionaries.

Filename:`loops/__main__.py`

```py
from typing import Dict

a: Dict[str, int] = {"a": 10, "b": 20}

for key in a:
    print(f"At key {key}, we have value {a[key]}")
```

Or a more readable version

Filename:`loops/__main__.py`

```py
from typing import Dict

a: Dict[str, int] = {"a": 10, "b": 20}

for key, value in a.items():
    print(f"At key {key}, we have value {a[key]}")
```

We can also refactor our countdown example to leverage Python's built-in `range` function

Filename:`loops/__main__.py`

```py
for count in range(3, 0, -1):
    print(f"{count}!")
print("LIFTOFF!!!")
```

We can also use `else` in the `for..else` pattern

Filename:`loops/__main__.py`

```py
for count in range(3, 0, -1):
    print(f"{count}!")
else:
    print("LIFTOFF!!!")
```

Or even the reverse list slice operation

Filename:`loops/__main__.py`

```py
for count in range(1, 4)[::-1]:
    print(f"{count}!")
print("LIFTOFF!!!")
```

Depending on context, each one of these variations have their place. In general use the option which makes the code the most readable!

<!-- ### Pythonic Patterns for managing break  -->

# Summary

Here ends a chapter on Python and how it handles variables, functions, comments, and control flow!

Next we'll examine ways for building data structured in a way which provides validation and typing on runtime.

<!-- to do yield patterns to break out of double loops -->
