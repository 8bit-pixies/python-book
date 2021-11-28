---
title: Guessing Game
---

Let's drive right into it and begin by building a guessing game. We’ll implement a classic beginner programming problem: a guessing game. Here’s how it works: the program will generate a random integer between 1 and 100. It will then prompt the player to enter a guess. After a guess is entered, the program will indicate whether the guess is too low or too high. If the guess is correct, the game will print a congratulatory message and exit.

# Setting Up a New Project

To setup a new project we'll navigate to the _projects_ directory and make a new project using Poetry like so:

```console
$ poetry new guessing_game
$ cd guessing_game
$ echo 'print("Hello, world!")' > guessing_game/__main__.py
```

Just like in Chapter 1, we'll start by printing "Hello, world!" by adding a new file `guessing_game/__main__.py`.

Filename: `guessing_game/__main__.py`

```py
print("Hello, world!")
```

Now we can run this via `poetry run` as a module:

```console
$ poetry run python -m guessing_game
Hello, world!
```

We can use this rapidly iterate on a project. Let's modify `guessing_game/__main__.py` some more.

# Processing a guess

The first thing we need to achieve in our game is to ask for user input and process that input. To start, we'll allow the player to input a guess.

Filename: `guessing_game/__main__.py`

```py
print("Guess the number!")
guess = input("Please input your guess: ")

print(f"You guessed: {guess}")
```

We now introduce the `input` function which enables input. Notice that the resulting variable stored in `guess` is now a `string`. When printing out strings in python, you can make use of f-strings, which makes it easier for format strings.

# Storing Values with Variables

Similar to many other dynamic programming languages, python does not distinguish between mutable and immutable types. For example:

```py
guess = "hello"
guess = 10
```

Is entirely allowable code, even though not only did the variable change, but the type changed as well! We'll go through validation strategies in a later chapter, for now, we can simply try parsing the output.

Filename: `guessing_game/__main__.py`

```py
print("Guess the number!")
guess = int(input("Please input your guess: "))

print(f"You guessed: {guess}")
```

This will work as intended for integer inputs

```console
$ poetry run python -m guessing_game
Guess the number!
Please input your guess: 10
You guessed: 10
```

Now if you input anything that isn't an integer it will raise a `ValueError`:

```console
$ poetry run python -m guessing_game
Guess the number!
Please input your guess: not a number
Please input your guess: not a number
Traceback (most recent call last):
  File "/bin/python/runpy.py", line 193, in _run_module_as_main
    "__main__", mod_spec)
  File "/bin/python/runpy.py", line 85, in _run_code
    exec(code, run_globals)
  File "file:///projects/guessing_game/guessing_game/__main__.py", line 2, in <module>
    guess = int(input("Please input your guess: "))
ValueError: invalid literal for int() with base 10: 'not a number'
```

# Generating a Secret Number

The next step is to generate a secret number to guess. This number needs to be randomly generated everytime the program is run, so we'll use a random number generator between 1 and 100. As the Python standard libraries include `random`, we can use this to generate random numbers.

Filename: `guessing_game/__main__.py`

```py
import random

print("Guess the number!")
secret_number = random.randint(1, 100)

print(f"The secret number is: {secret_number}")
guess = int(input("Please input your guess: "))
print(f"You guessed: {guess}")
```

First we add the line: `import random`. This enables the random module to be used within our project. We then can add the code to generate the number, which is conveniently proivided as `random.randint(1, 100)`, alternatively we could use `random.choice(range(1, 101))` to request a number between 1 and 100.

> Of course, its not feasible to just try "guessing" what methods or functions to use from a python module. Within Python, by convention docstrings are typically used. To view docstrings for `rand` for example, you can try entering `print(rand.__doc__)` to read the package level documentation for `rand` and `print(random.randint.__doc__)` for the documentation for the `randint` function.

For now we are also printing the secret number for testing purposes - in the final version we would remove it. Otherwise it would not be much of a game! Let's try running the programming a few times.

```console
$ poetry run python -m guessing_game
Guess the number!
The secret number is: 64
Please input your guess: 4
You guessed: 4

$ poetry run python -m guessing_game
Guess the number!
The secret number is: 49
Please input your guess: 10
You guessed: 10
```

You should get different random numbers which are between 1 and 100.

# Comparing the Guess to the Secret Number

To compare guesses in Python, we will make use of `if` and `else` statements.

Filename: `guessing_game/__main__.py`

```py
import random

print("Guess the number!")
secret_number = random.randint(1, 100)

guess = int(input("Please input your guess: "))

if guess < secret_number:
    print("Too small!")
elif guess > secret_number:
    print("Too big!")
else:
    print("Just right!")
```

> Note: at this point in time you would realise whitespace is special in Python! Rather than using curly braces, Python uses whitespace to define the "scope" for control flow operations.

In Python 3.X, there isn't an inbuilt `cmp` operator where this can be achieved through pattern matching without additional overhead.

Rather than having one guess, we can include a `while` loop as well

Filename: `guessing_game/__main__.py`

```py
import random

print("Guess the number!")
secret_number = random.randint(1, 100)

while (guess := int(input("Please input your guess: "))) != secret_number:
    if guess < secret_number:
        print("Too small!")
    elif guess > secret_number:
        print("Too big!")
else:
    print("Just right!")
```

This is an example of the walrus operator `:=`, if you are running a version lower than Python 3.8, you may need to split up the code to resemble the below:

```py
guess = int(input("Please input your guess: "))
while True:
    # add a way for comparison and guess to be updated.
```

We can also ensure Poetry picks up on these nuances, to explore this further, we should first change the expected Python version to reflect this:

Filename: `pyproject.toml`

```toml
[tool.poetry.dependencies]
python = "^3.8"
```

This would indicate at the version of Python is at least Python 3.8, then we should update Poetry so it is aware that our configuration has changed via `poetry lock`. Then your output should resemble the below:

```console
$ poetry lock
Updating dependencies
Resolving dependencies... (1.1s)

Writing lock file
```

Now when you use Poetry, a virtual environment with the correct Python versioning will be referenced.

The other nuance within our Python code is the inclusion of `while..else`. The nuance when there is `else` is if the while loop terminates normally, the `else` clause will be triggered, but if it is terminated abnormally it will not, for example if `break` statement is encounted.

We can observe this in the following simple example:

```py
counter = 0

while counter <= 2:
    counter += 1
    print(f"Counter: {counter}")
else:
    print("Else statement!")

# else does not trigger here!
counter = 0

while counter < 3:
    if counter == 2:
        break
    counter += 1
    print(f"Counter: {counter}")
else:
    print("Else statement!")
```

# Summary

We've built the guessing game. Congratulations!

We've introduced some Python specific ideas around `while..else` and the walrus operator `:=`. Next we'll explore common programming concepts within Python.
