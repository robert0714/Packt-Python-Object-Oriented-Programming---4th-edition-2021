# Python 3 Object-Oriented Programming 4th ed.

Chapter 2, Objects in Python.

## Introducing Type Hints
![fig_1.png](./fig_1.png)
```python
>>> type("Hello, world!")
<class 'str'>
>>> type(42)
<class 'int'>

>>> a_string_variable = "Hello, world!"
>>> type(a_string_variable)
<class 'str'>
>>> a_string_variable = 42
>>> type(a_string_variable)
<class 'int'>

```

### Type Checking

```python
>>> def odd(n):
...     return n % 2 != 0

>>> odd(3)
True
>>> odd(4)
False

>>> odd("Hello, world!")
Traceback (most recent call last):
...
TypeError: not all arguments converted during string formatting

```

```python
>>> "a=%d" % 113
'a=113'
>>> 355 % 113
16

```
[The **mypy** tool](https://mypy.readthedocs.io/en/stable/getting_started.html) is commonly used to check the hints for consistency. It's not built into Python, and requires a separate download and install. We'll talk about virtual environments and installation of tools later in this chapter, in the *Third-party libraries* section. For now, you can use `python -m pip install mypy` or `conda install mypy` if you're using **the conda tool**.

Mypy supports reading configuration settings from a file with the following precedence order:
```
1. ./mypy.ini
2. ./.mypy.ini
3. ./pyproject.toml
4. ./setup.cfg
5. $XDG_CONFIG_HOME/mypy/config
6. ~/.config/mypy/config
7. ~/.mypy.ini
```
example:
  * https://mypy.readthedocs.io/en/stable/config_file.html
  * https://github.com/python/mypy/raw/master/mypy_self_check.ini
  * https://github.com/python/mypy/raw/master/mypy_bootstrap.ini

Let's say we had a file, `bad_hints.py`, in a `src` directory, with these two functions and a few lines to call the [`main()` function](../src/bad_hints.py?plain=1#L10-L19):
```python
def odd(n: int) -> bool:
    return n % 2 != 0
def main():
    print(odd("Hello, world!"))
if __name__ == "__main__":
    main()
```
When we run the `mypy` command at the OS's terminal prompt:
```bash
% mypy --strict src/bad_hints.py
```
The **mypy** tool is going to spot a bunch of potential problems, including at least these:
```bash
src/bad_hints.py:12: error: Function is missing a return type annotation
src/bad_hints.py:12: note: Use "-> None" if function does not return a value
src/bad_hints.py:13: error: Argument 1 to "odd" has incompatible type "str"; expected "int"
```
Python Enhancement Proposal (PEP) 585 covers some new language features to make type hints a bit simpler.
## Creating Python classes
We might think there isn't much we can do with this most basic class, but it does allow us to instantiate objects of that class. We can load the class into the Python 3 interpreter, so we can interactively play with it. To do this, save the class definition mentioned earlier in a file named `first_class.py` and then run the `python -i first_class.py` command. The `-i` argument tells Python to *run the code and then drop to the interactive interpreter*. The following interpreter session demonstrates a basic interaction with this class:
```python
>>> class MyFirstClass:
...     pass

>>> a = MyFirstClass()
>>> b = MyFirstClass()
>>> print(a)
<__main__.MyFirstClass object at ...>
>>> print(b)
<__main__.MyFirstClass object at ...>

>>> a is b
False
  
```

### Adding attributes

```python
>>> class Point: 
...     pass 
 
>>> p1 = Point() 
>>> p2 = Point() 
 
>>> p1.x = 5 
>>> p1.y = 4 
 
>>> p2.x = 3 
>>> p2.y = 6 
 
>>> print(p1.x, p1.y)
5 4
>>> print(p2.x, p2.y) 
3 6

```

### Making it do something

```python

>>> class Point: 
...     def reset(self): 
...         self.x = 0 
...         self.y = 0 
... 
 
>>> p = Point() 
>>> p.reset() 
>>> print(p.x, p.y) 
0 0

```

### Talking to yourself

```python
>>> p = Point() 
>>> Point.reset(p) 
>>> print(p.x, p.y) 
0 0

```


```python
>>> class Point:
...     def reset():
...         pass
...
>>> p = Point()
>>> p.reset()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: reset() takes 0 positional arguments but 1 was given

```

### More arguments

```python
>>> import math

>>> class Point:
...     def move(self, x, y):
...         self.x = x
...         self.y = y
...     def reset(self):
...         self.move(0, 0)
...     def calculate_distance(self, other_point):
...         return math.sqrt(
...              (self.x - other_point.x) ** 2
...              + (self.y - other_point.y) ** 2
...         )
... 

How to use this class:

>>> point1 = Point()
>>> point2 = Point()

>>> point1.reset()
>>> point2.move(5, 0)
>>> print(point2.calculate_distance(point1))
5.0

>>> assert point2.calculate_distance(point1) == point1.calculate_distance(
...    point2
... )

point1.move(3, 4)
print(point1.calculate_distance(point2))
4.47213595499958
print(point1.calculate_distance(point1))
5.0

```

### Initializing the object

```python
>>> import math

>>> class Point:
...     def move(self, x, y):
...         self.x = x
...         self.y = y
...     def reset(self):
...         self.move(0, 0)
...     def calculate_distance(self, other_point):
...         return math.hypot(self.x-other_point.x, self.y-other_point.y)
... 

>>> point = Point()
>>> point.x = 5
>>> print(point.x)
5
>>> print(point.y)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Point' object has no attribute 'y'

```

```python
>>> class Point: 
...     def __init__(self, x, y): 
...         self.move(x, y) 
...     def move(self, x, y): 
...         self.x = x 
...         self.y = y  
...     def reset(self): 
...         self.move(0, 0) 
... 

Constructing a Point 

>>> point = Point(3, 5) 
>>> print(point.x, point.y) 
3 5

```

### Explaining Yourself

## Modules and Packages

```python
>>> import point_1
>>> p = point_1.Point(3, 4)
>>> print(f"{p.x=}, {p.y=}")
p.x=3, p.y=4

```

```python
from point_1 import Point
>>> p = Point(3, 4)
>>> print(f"{p.x=}, {p.y=}")
p.x=3, p.y=4

```


```python
>>> from point_1 import Point as PT
>>> p = PT(3, 4)
>>> print(f"{p.x=}, {p.y=}")
p.x=3, p.y=4

```

```python
>>> from point_1 import *

```

### Organizing modules

### Absolute imports

```python
>>> import ecommerce.products
>>> product = ecommerce.products.Product("name1") 

```

```python
>>> from ecommerce.products import Product 
>>> product = Product("name2") 

```


```python
>>> from ecommerce import products 
>>> product = products.Product("name3") 

```

### Relative imports

```python
>>> import ecommerce.products

```

## Organizing module content

## Who can access my data?

## Third-party libraries
### mypy
```bash
python -m pip install mypy  # on basic python
conda install mypy # on Anaconda or miniconda
```
### virtual environmen
Python ships with a tool called venv, a utility that gives you a Python installation called a virtual environment in your working directory. When you activate this environment, commands related to Python will work with your virtual environment's Python instead of the system Python. So, when you run pip or python, it won't touch the system Python at all. Here's how to use it:
```bash
cd project_directory
python -m venv .env
source .env/bin/activate    # on Linux or macOS
.env/Scripts/activate.bat   # on Windows
```
(For other OSes, see https://docs.python.org/3/library/venv.html, which has all the variations required to activate the environment.)