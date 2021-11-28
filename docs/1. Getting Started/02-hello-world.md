---
title: Hello, World!
---

# Hello, World!

Now that you've installed Python, let's write our first Python program; printing `Hello, World!` to the screen.

> Note: in this book we assume familiarity with command line. We make no assumption on the tooling, or how you may edit your code. There are many great integrated development environments (IDE) that have excellent Python support.

# Creating a Project Directory

Let's start by making a directory to store our Python code. It doesn't really matter where Python code resides, but for working through this book, we recommend making a _projects_ directory in your home directory and keeping all your projects there.

Open a terminal and enter the following commands to make a projects directory and a directory for the “Hello, world!” project within the projects directory.

For Linux, macOS, and PowerShell on Windows, enter this:

```console
$ mkdir ~/projects
$ cd ~/projects
$ mkdir hello_world
$ cd hello_world
```

For Windows CMD, enter this:

```console
> mkdir "%USERPROFILE%\projects"
> cd /d "%USERPROFILE%\projects"
> mkdir hello_world
> cd hello_world
```

# Writing and Running a Python Program

Now, let's make a source file and call it _main.py_. Python files end with the extension _.py_. It is convention if you use more than one word in the filename, then an underscore is used to separate them. For example, it is preferable to use _hello_world.py_ rather than _helloworld.py_.

Open _main.py_ file you just created and enter the following code

Filename: `main.py`

```py
print("Hello, world!")
```

Save the file and go back to your terminal to run the file:

```console
$ python main.py
Hello, world!
```

Regardless of your operating system, the string `Hello, world!` should print to the terminal.

If you got this to print, then you've officially written your first Python program!
