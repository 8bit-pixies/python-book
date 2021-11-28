# Hello, Poetry!

Writing clean Python code isn't just about writing Python scripts. At some point your code needs to be distributed and deployed. Often we need to describe and manage the libraries which our code requires.

Unlike Rust, there isn't an idiomatic pattern like _cargo_ to construct packages. In this book, we use _poetry_ as an opinionated way to construct packages, as it is an approach inspired by _cargo_.
First check if _pip_ is installed through

```console
$ pip --version
```

If you see a version number, then that's it! Otherwise you'll need to install pip depending on how you have installed python.

Now you can install _poetry_ as follows

```console
$ pip install poetry
```

# Creating a Project with Poetry

We can create a project with _poetry_.

```console
$ poetry new hello_poetry
$ cd hello_poetry
```

Inside this directory you can see all the files required to build a python package. First lets have a look at `hello_poetry/__init__.py`

Filename: `hello_poetry/__init__.py`

```py
__version__ = '0.1.0'
```

Poetry has generated a package for us, it is up to us to add an some code. To accomplish this create the file `hello_poetry/__main__.py`

Filename: `hello_poetry/__main__.py`

```py
print("Hello, world!")
```

Now we can run this via `poetry run python -m hello_poetry` which results in this:

```console
$ poetry run python -m hello_poetry
Hello, world!
```

What happend here? Poetry under the hood does several things:

1.  It creates a virtual environment based on our package definition.
2.  It starts up `python` and runs the module as a script via the `-m` flag.

There a few additional command line features in poetry including `poetry install`, `poetry build`, and extensions to `poetry run` which we can configure as part of our `pyproject.toml`.

<!-- to do info -->

# Summary

In this chapter we've:

- Installed Python
- Written a "Hello, World!" program using `python` directly
- Created a new python package using the conventions of `poetry`

Now we can start writing a more substantial program to get used to reading Python code.
