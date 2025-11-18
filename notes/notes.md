# Internals

## Packages

Package - a Python module which can contain submodules or recursively,
subpackages.

More:

- https://docs.python.org/3/tutorial/modules.html#packages
-
-

## Modules

Module - an object that serves as an organizational unit of Python code. Modules
have a namespace containing arbitrary Python objects. Modules are loaded into
Python by the process of _importing_.

More:

- https://docs.python.org/3/tutorial/modules.html
- https://docs.python.org/3/reference/datamodel.html#modules
- https://docs.python.org/3/reference/import.html

### `__main__`

More:

- https://docs.python.org/3/reference/import.html#special-considerations-for-main
- https://docs.python.org/3/reference/datamodel.html#module.__name__
- https://docs.python.org/3/tutorial/modules.html#executing-modules-as-scripts
- https://docs.python.org/3/library/__main__.html

## Classes

### Encapsulation

```python
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

    @property
    def x(self):
        return self._x

    @x.setter
    def x(self, value):
        self._x = self._validate(value)

    @property
    def y(self):
        return self._y

    @y.setter
    def y(self, value):
        self._y = self._validate(value)

    @staticmethod
    def _validate(value):
        if not isinstance(value, int | float):
            raise ValueError("number expected")
        return value


if __name__ == '__main__':
    point = Point(12, 5)
    print(point.x)
    print(point.y)
    point.y = "10"
```

### `super()`

More:

- https://docs.python.org/3/library/functions.html#super
- https://docs.python.org/3/reference/datamodel.html#object.__init__

### Special attributes (`__dict__`, `__module__`, etc.)

More:

- https://docs.python.org/3/library/stdtypes.html#special-attributes
- https://docs.python.org/3/reference/datamodel.html#special-writable-attributes
- https://docs.python.org/3/reference/datamodel.html#instance-methods
- https://docs.python.org/3/reference/datamodel.html#special-attributes

### Special methods a.k.a magic methods (`__init__`, `__str__`, etc.)

More:

- https://docs.python.org/3/reference/datamodel.html#special-methods

## Functions

More:

- https://docs.python.org/3/reference/compound_stmts.html#function-definitions
- https://docs.python.org/3/tutorial/controlflow.html#defining-functions

### Positional and keyword arguments

More:

- https://docs.python.org/3/tutorial/controlflow.html#recap

### Anonymous functions a.k.a `lambda`

More:

- https://docs.python.org/3/tutorial/controlflow.html#lambda-expressions

## Scopes

More:

- https://docs.python.org/3/reference/executionmodel.html#resolution-of-names
- https://docs.python.org/3/tutorial/classes.html#python-scopes-and-namespaces

## Code loading

Helper:

```python
import sys; print(f"{__name__ = }\n{sys.path = }")
```

For `-c <command>`:

```bash
$ python -c "import sys; print(f\"{__name__ = }\n{sys.path = }\")"
```

For `-m <module-name>`:

```bash
$ python -m module
$ python -m regular_package
$ python -m namespace_package
```

> If `<module-name>` is regular package, `__init__.py` is executed as well.

For `<script>`:

```bash
$ python module
$ python regular_package
$ python namespace_package
```

> If `<script>` is regular package, `__init__.py` is not executed.

More:

- https://docs.python.org/3/library/sys_path_init.html
- https://docs.python.org/3/library/sys.html#sys.path

## OOP vs functional

```python
class Pet:
    def __init__(self, name):
        self.name = name


class Dog(Pet):
    def speak(self):
        print(f"{self.name} says whoof!")


class Cat(Pet):
    def speak(self):
        print(f"{self.name} says mhoew!")


dog = Dog("Roxana")
dog.speak()
Dog.speak(dog)  # Call dog.speak() is exactly equivalent to Dog.speak(dog).
Cat.speak(dog)  # It works as well, but dog says mhoew.
```

More:

- https://docs.python.org/3/tutorial/classes.html#method-objects

# Software packaging

Packages are the idiomatic way to package software in `Python`.

# Interactive shell

More:

- https://docs.python.org/3.13/tutorial/interpreter.html#using-the-python-interpreter
- https://docs.python.org/3/using/cmdline.html#command-line-and-environment

# Style guide

More:

- https://peps.python.org/pep-0008/

## Google

Every major open-source project has its own style guide: a set of conventions
(sometimes arbitrary) about how to write code for that project. It is much
easier to understand a large codebase when all the code in it is in a consistent
style.

More:

- https://google.github.io/styleguide/pyguide.html

# Typespecs

# Docs

Write `docstrings` for all public modules, functions, classes, and methods.
`Docstrings` are not necessary for non-public methods, but you should have a
comment that describes what the method does. This comment should appear after
the `def` line.

More:

- https://peps.python.org/pep-0257/
- https://realpython.com/how-to-write-docstrings-in-python/
- https://google.github.io/styleguide/pyguide.html#381-docstrings

# Debugging

# Logging

# Testing

# Truthy and falsy values

More:

- https://docs.python.org/3/library/stdtypes.html#truth-value-testing

# Data persistance

More:

- https://docs.python.org/3/library/shelve.html
- https://docs.python.org/3/library/pickle.html

# Exceptions

More:

- https://docs.python.org/3/tutorial/errors.html
- https://docs.python.org/3/library/exceptions.html
- https://docs.python.org/3/reference/executionmodel.html#exceptions
- https://docs.python.org/3/reference/compound_stmts.html#the-try-statement

# Iterators

> `__iter__()` and `__next__()` methods do not have to come form the same class:

```python
class Fib:
    def __init__(self, nn):
        self.__n = nn
        self.__i = 0
        self.__p1 = self.__p2 = 1

    def __next__(self):
        self.__i += 1
        if self.__i > self.__n:
            raise StopIteration
        if self.__i in [1, 2]:
            return 1
        ret = self.__p1 + self.__p2
        self.__p1, self.__p2 = self.__p2, ret
        return ret


class Closure:
    def __init__(self, n):
        self.__iter = Fib(n)

    def __iter__(self):
        return self.__iter


if __name__ == '__main__':
    for i in Closure(8):
        print(i)
```

More:

- https://docs.python.org/3/tutorial/classes.html#iterators

# Generators

More:

- https://docs.python.org/3/tutorial/classes.html#generators

# Comprehensions

More:

- https://docs.python.org/3/reference/expressions.html#displays-for-lists-sets-and-dictionaries
- https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions

# Reading and writing files

> It is important to use encoding with text files.

More:

- https://docs.python.org/3/tutorial/inputoutput.html#reading-and-writing-files

# SQL

Using `SQLAlchemy`.

Best practices:

```python
import logging

import sqlalchemy as sa


# Set constants.
DRIVERNAME = "sqlite+pysqlite"
DATABASE = "todo.db"

CREATE_TABLE = """
CREATE TABLE IF NOT EXISTS tasks (
id INTEGER PRIMARY KEY,
name TEXT NOT NULL,
priority INTEGER NOT NULL
)
;
"""

INSERT_TASKS = """
INSERT INTO tasks (name, priority)
VALUES (:name, :priority)
;
"""

TASKS = [
    {"name": "My first task", "priority": 1},
    {"name": "My second task", "priority": 5},
    {"name": "My third task", "priority": 10},
]

# Enable logging.
logging.basicConfig()
logging.getLogger("sqlalchemy").setLevel(logging.INFO)

# Creating URLs programmatically.
url = sa.URL.create(
    drivername=DRIVERNAME,
    database=DATABASE,
)

# Establishing connectivity - the engine.
engine = sa.create_engine(
    url=url,
)

# Getting a connection using context manager.
with engine.connect() as conn:
    conn.execute(
        statement=sa.text(text=CREATE_TABLE),
    )
    conn.execute(
        statement=sa.text(text=INSERT_TASKS),
        parameters=TASKS,
    )
    conn.commit()
```

# Miscellaneous

## `dir` helper

Without arguments, return the list of names in the current local scope. With an
argument, attempt to return a list of valid attributes for that object.

More:

- https://docs.python.org/3/tutorial/modules.html#the-dir-function
- https://docs.python.org/3/library/functions.html#dir

## `help` helper

Invoke the built-in help system.

More:

- https://docs.python.org/3/library/functions.html#help

## `type` helper

With one argument, return the type of an object. The return value is a type
object and generally the same object as returned by `object.__class__`.

More:

- https://docs.python.org/3/library/functions.html#type

## `id` helper

Return the “identity” of an object. This is an integer which is guaranteed to be
unique and constant for this object during its lifetime.

More:

- https://docs.python.org/3/library/functions.html#id

## Shortcut operators / augmented assignment statements (e.g. `+=`)

More:

- https://docs.python.org/3/reference/simple_stmts.html#augmented-assignment-statements

## `break` and `continue` statements

More:

- https://docs.python.org/3.13/tutorial/controlflow.html#break-and-continue-statements
- https://docs.python.org/3.13/reference/simple_stmts.html#the-break-statement
- https://docs.python.org/3.13/reference/simple_stmts.html#the-continue-statement

## `else` clauses on loops

More:

- https://docs.python.org/3.13/tutorial/controlflow.html#else-clauses-on-loops

## Boolean (`and, or, not`) vs bitwise operations (`&, |, ^`)

More:

- https://docs.python.org/3/library/stdtypes.html#boolean-type-bool
- https://docs.python.org/3/library/stdtypes.html#boolean-operations-and-or-not
- https://docs.python.org/3/reference/expressions.html#boolean-operations
- https://docs.python.org/3/library/stdtypes.html#bitwise-operations-on-integer-types
- https://docs.python.org/3/reference/expressions.html#binary-bitwise-operations

## Swapping values / multiply assignment (`x, y = y, x`)

More:

- https://docs.python.org/3/reference/simple_stmts.html#assignment-statements
- https://docs.python.org/3/tutorial/introduction.html#first-steps-towards-programming

## Leading underscores (`_`)

> `help()` only shows names without leading underscores.

More:

- https://docs.python.org/3/tutorial/classes.html#private-variables
- https://docs.python.org/3/reference/lexical_analysis.html#reserved-classes-of-identifiers
- https://docs.python.org/3/reference/expressions.html#index-5

## `__dict__` vs `dir()`

`__dict__` - dictionary

`dir()` - directory

More:

- https://docs.python.org/3/reference/datamodel.html#object.__dict__
- https://docs.python.org/3/library/functions.html#dir

# Acronyms

- PEP - Python Enhancement Proposals

# Dictionary
