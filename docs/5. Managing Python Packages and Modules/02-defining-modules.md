---
title: Defining Modules in Python
---

# Defining Modules in Python

In this section, we'll talk about modules in Python, specifically the usage of the `__init__.py` file. 

_Modules_ let us organize code within a package into groups for readability and easy reuse.

As an example let's write a package that provides the functionality of a restaurant. We’ll define the signatures of functions but leave their bodies empty to concentrate on the organization of the code, rather than actually implement a restaurant in code.

In the restaurant industry, some parts of a restaurant are referred to as _front of house_ and others as _back of house_. Front of house is where customers are; this is where hosts seat customers, servers take orders and payment, and bartenders make drinks. Back of house is where the chefs and cooks work in the kitchen, dishwashers clean up, and managers do administrative work.

To structure our crate in the same way that a real restaurant works, we can organize the functions into nested modules. Create a new library named _restaurant_ by running `poetry new restaurant`,

We can now create the `front_of_house` folder with `hosting.py` and `serving.py`, which should resemble the following:

```
restaurant
├── __init__.py
└── front_of_house
    ├── __init__.py
    ├── hosting.py
    └── serving.py
```

This tree shows how the modules can nest inside one another, note that we require the `__init__.py` file to inform the location of a module. 

To test that this works, let's add some dummy functions to `hosting.py` and `serving.py` that resembles the below:

Filename:`restaurant/front_of_house/hosting.py`
```py
def add_to_waitlist():
    pass

def seat_at_table():
    pass
```

Filename:`restaurant/front_of_house/serving.py`
```py
def take_order():
    pass

def serve_order():
    pass

def take_payment():
    pass
```

We can install our package by running `poetry install`

```console
$ poetry install
Installing dependencies from lock file

(..snip..)

Installing the current project: restaurant (0.1.0)
```

To demonstrate how we can interact with our package, we can spawn a new shell using `poetry shell`

```console
$ poetry shell
Spawning shell within (..snip..)
$ python
>>> from restaurant.front_of_house.hosting import add_to_waitlist
>>> exit()
$ exit
```

To exit the shell itself, simply use `exit`. 