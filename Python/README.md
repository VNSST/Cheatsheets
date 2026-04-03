# 🐍 Python Cheatsheet

> A comprehensive reference guide covering Python fundamentals through advanced concepts.

---

## Table of Contents

1. [Variables & Data Types](#variables--data-types)
2. [Operators](#operators)
3. [Strings](#strings)
4. [Data Structures](#data-structures)
5. [Control Flow](#control-flow)
6. [Functions](#functions)
7. [OOP](#object-oriented-programming)
8. [File Handling](#file-handling)
9. [Error Handling](#error-handling)
10. [Comprehensions & Generators](#list-comprehensions--generators)
11. [Decorators](#decorators)
12. [Type Hints](#type-hints)
13. [Best Practices](#best-practices)

---

## Getting Started

```bash
python --version        # Check version
python script.py        # Run script
python                  # Interactive REPL
```

```python
print("Hello, World!")

# Single-line comment
"""Multi-line comment / docstring"""
```

---

## Variables & Data Types

```python
x = 10          # int
y = 3.14        # float
name = "Alice"  # str
flag = True     # bool
nothing = None  # NoneType
```

| Type       | Example                 | Mutable? |
|------------|-------------------------|----------|
| `int`      | `42`                    | ❌       |
| `float`    | `3.14`                  | ❌       |
| `str`      | `"hello"`               | ❌       |
| `bool`     | `True / False`          | ❌       |
| `list`     | `[1, 2, 3]`             | ✅       |
| `tuple`    | `(1, 2, 3)`             | ❌       |
| `set`      | `{1, 2, 3}`             | ✅       |
| `dict`     | `{"a": 1}`              | ✅       |
| `NoneType` | `None`                  | ❌       |

### Type Checking & Conversion

```python
type(x)              # <class 'int'>
isinstance(x, int)   # True

int("10")       # 10
float("3.14")   # 3.14
str(42)         # "42"
bool(0)         # False
list("abc")     # ['a', 'b', 'c']
```

### Multiple Assignment

```python
a, b, c = 1, 2, 3
x = y = z = 0
a, b = b, a          # Swap
```

---

## Operators

```python
# Arithmetic
a + b    a - b    a * b    a / b    a // b    a % b    a ** b

# Comparison
a == b   a != b   a > b    a < b    a >= b    a <= b

# Logical
a and b   a or b   not a

# Identity & Membership
x is y        x is not y
"a" in "abc"  3 in [1, 2, 3]

# Walrus operator (Python 3.8+)
if (n := len("hello")) > 3:
    print(n)  # 5
```

---

## Strings

```python
s = "Hello, World!"

# Common Methods
s.upper()    s.lower()    s.title()    s.strip()
s.replace("World", "Python")    s.split(", ")
", ".join(["a", "b"])           s.find("World")  # 7
s.startswith("He")              s.count("l")      # 3

# Slicing
s[0]       # 'H'       s[-1]     # '!'
s[0:5]     # 'Hello'   s[::-1]   # reversed

# f-strings (PREFERRED)
name, age = "Alice", 30
f"Name: {name}, Age: {age}"
f"{3.14159:.2f}"    # '3.14'
f"{'hi':>10}"       # '        hi'
```

---

## Data Structures

### Lists

```python
fruits = ["apple", "banana", "cherry"]
fruits[0]                    # "apple"
fruits.append("date")        # Add to end
fruits.insert(1, "berry")    # Insert at index
fruits.extend(["fig"])       # Add multiple
fruits.remove("banana")      # Remove first match
fruits.pop()                 # Remove & return last
fruits.sort()                # Sort in-place
sorted(fruits)               # Return sorted copy
len(fruits)                  # Length

# Unpacking
first, *rest = [1, 2, 3, 4]  # first=1, rest=[2,3,4]
```

### Tuples

```python
coords = (10, 20)       # Immutable
single = (42,)           # Single element needs comma

from collections import namedtuple
Point = namedtuple("Point", ["x", "y"])
p = Point(10, 20)       # p.x → 10
```

### Dictionaries

```python
person = {"name": "Alice", "age": 30}
person["name"]                  # "Alice"
person.get("phone", "N/A")     # "N/A" (default)
person["email"] = "a@b.com"    # Add/update
person.pop("age")               # Remove & return
person.update({"city": "NYC"})

for key, val in person.items():
    print(f"{key}: {val}")

# Dict comprehension
squares = {x: x**2 for x in range(6)}

# Merge (Python 3.9+)
merged = dict1 | dict2
```

### Sets

```python
s = {1, 2, 3}
s.add(4)       s.remove(2)     s.discard(99)

a = {1, 2, 3, 4}
b = {3, 4, 5, 6}
a | b     # Union:        {1,2,3,4,5,6}
a & b     # Intersection: {3,4}
a - b     # Difference:   {1,2}
a ^ b     # Symmetric:    {1,2,5,6}
```

### Collections Module

```python
from collections import Counter, defaultdict, deque

Counter("abracadabra").most_common(3)  # [('a',5), ('b',2), ('r',2)]
dd = defaultdict(list); dd["key"].append(1)
dq = deque([1,2,3]); dq.appendleft(0); dq.popleft()
```

---

## Control Flow

### Conditionals

```python
if x > 0:
    print("Positive")
elif x == 0:
    print("Zero")
else:
    print("Negative")

# Ternary
result = "even" if x % 2 == 0 else "odd"

# Match (Python 3.10+)
match command:
    case "quit": quit()
    case "hello": print("Hi!")
    case _: print("Unknown")
```

### Loops

```python
for i in range(5):          print(i)      # 0-4
for i in range(1, 10, 2):  print(i)      # 1,3,5,7,9
for i, item in enumerate(["a","b","c"]): print(i, item)
for a, b in zip(list1, list2): print(a, b)

while count < 5:
    count += 1

# Control: continue (skip), break (exit)
# for-else: else block runs if loop finishes without break
```

---

## Functions

```python
def greet(name, greeting="Hello"):
    """Greet a person."""
    return f"{greeting}, {name}!"

# *args and **kwargs
def func(*args, **kwargs):
    print(args)     # Tuple
    print(kwargs)   # Dict

# Return multiple values
def min_max(lst):
    return min(lst), max(lst)
lo, hi = min_max([3, 1, 4])

# Scope
# nonlocal → enclosing scope
# global → module-level scope
```

---

## Object-Oriented Programming

```python
class Dog:
    species = "Canis familiaris"  # Class variable

    def __init__(self, name, age):
        self.name = name
        self.age = age

    def bark(self):
        return f"{self.name} says Woof!"

    def __str__(self):
        return f"{self.name}, {self.age} yrs"

    @classmethod
    def from_birth_year(cls, name, year):
        return cls(name, 2026 - year)

    @staticmethod
    def is_valid_age(age):
        return 0 <= age <= 30

    @property
    def human_age(self):
        return self.age * 7

# Inheritance
class GuideDog(Dog):
    def __init__(self, name, age, handler):
        super().__init__(name, age)
        self.handler = handler
```

### Dataclasses (Python 3.7+)

```python
from dataclasses import dataclass, field

@dataclass
class Employee:
    name: str
    age: int
    department: str = "Engineering"
    skills: list = field(default_factory=list)
```

### Key Dunder Methods

| Method           | Trigger           |
|------------------|-------------------|
| `__init__`       | Constructor       |
| `__str__`        | `str(obj)`        |
| `__repr__`       | `repr(obj)`       |
| `__len__`        | `len(obj)`        |
| `__getitem__`    | `obj[key]`        |
| `__iter__`       | `for x in obj`    |
| `__call__`       | `obj()`           |
| `__eq__`         | `obj == other`    |
| `__enter__/__exit__` | `with obj:`   |

---

## File Handling

```python
# Read
with open("file.txt", "r") as f:
    content = f.read()

# Write ("w" overwrites, "a" appends)
with open("file.txt", "w") as f:
    f.write("Hello!\n")

# pathlib (modern)
from pathlib import Path
p = Path("data") / "file.txt"
p.read_text()
p.write_text("Hello")
list(Path(".").rglob("*.py"))

# JSON
import json
data = json.loads('{"key": "val"}')
json.dumps(data, indent=2)
```

---

## Error Handling

```python
try:
    result = 10 / 0
except ZeroDivisionError as e:
    print(f"Error: {e}")
except (ValueError, TypeError):
    print("Value or Type error")
else:
    print("Success — no exception")
finally:
    print("Always runs")

raise ValueError("Invalid!")

# Custom exception
class MyError(Exception):
    pass

# Assertions
assert x > 0, "x must be positive"
```

| Exception           | Cause                      |
|----------------------|----------------------------|
| `ValueError`         | Wrong value                |
| `TypeError`          | Wrong type                 |
| `KeyError`           | Key not found in dict      |
| `IndexError`         | Index out of range         |
| `FileNotFoundError`  | File not found             |
| `ZeroDivisionError`  | Division by zero           |
| `AttributeError`     | Attribute not found        |

---

## List Comprehensions & Generators

```python
# List comprehension
[x**2 for x in range(10)]
[x for x in range(20) if x % 2 == 0]
[n for row in matrix for n in row]  # Flatten

# Dict & set comprehension
{word: len(word) for word in words}
{len(w) for w in words}

# Generator (lazy, memory-efficient)
gen = (x**2 for x in range(1000000))
next(gen)

def fibonacci():
    a, b = 0, 1
    while True:
        yield a
        a, b = b, a + b
```

---

## Decorators

```python
from functools import wraps

def timer(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        import time
        start = time.time()
        result = func(*args, **kwargs)
        print(f"{func.__name__}: {time.time()-start:.4f}s")
        return result
    return wrapper

@timer
def slow(): import time; time.sleep(1)

# Decorator with arguments
def repeat(n):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            for _ in range(n): result = func(*args, **kwargs)
            return result
        return wrapper
    return decorator

@repeat(3)
def hello(): print("Hi!")

# Built-in: @staticmethod, @classmethod, @property
# functools: @lru_cache, @cached_property
```

---

## Lambda, Map, Filter, Reduce

```python
square = lambda x: x ** 2

list(map(lambda x: x**2, [1,2,3]))           # [1, 4, 9]
list(filter(lambda x: x > 0, [-1, 0, 1, 2])) # [1, 2]

from functools import reduce
reduce(lambda a, b: a + b, [1,2,3,4])         # 10

# Prefer comprehensions:
[x**2 for x in [1,2,3]]           # replaces map
[x for x in [-1,0,1] if x > 0]    # replaces filter
```

---

## Virtual Environments & Packages

```bash
python -m venv myenv
myenv\Scripts\activate       # Windows
source myenv/bin/activate    # macOS/Linux
deactivate

pip install package_name
pip install -r requirements.txt
pip freeze > requirements.txt
pip list
```

---

## Type Hints

```python
def greet(name: str) -> str:
    return f"Hello, {name}"

def process(items: list[int]) -> dict[str, int]:   # 3.9+
    return {"total": sum(items)}

from typing import Optional, Union, Callable

def find(name: str) -> Optional[str]: ...           # str | None
def parse(val: str | int) -> float: ...             # 3.10+
def apply(fn: Callable[[int, int], int], a: int, b: int) -> int:
    return fn(a, b)
```

---

## Useful Built-in Functions

```python
abs(-5)  round(3.7)  min(1,2,3)  max(1,2,3)  sum([1,2,3])
len(x)  sorted(x)  reversed(x)  enumerate(x)  zip(a, b)
any(iterable)  all(iterable)  map(fn, it)  filter(fn, it)
type(x)  isinstance(x, int)  dir(obj)  hasattr(obj, "name")
id(obj)  hash(obj)  callable(obj)  help(fn)
input("Enter: ")  print("hi", end="", sep=", ")
```

---

## Best Practices (PEP 8)

- **4 spaces** indentation, **79 char** max line length
- `snake_case` functions/variables, `PascalCase` classes, `UPPER_CASE` constants
- Use `in` for containment: `if key in my_dict`
- Use `enumerate()` not `range(len(...))`
- Use `with` for file handling
- Use f-strings for formatting
- Prefer EAFP: `try/except` over checking conditions
- Truthiness: `if items:` not `if len(items) > 0`
- Chained comparisons: `0 < x < 10`

---

> **Last Updated:** April 2026 | **Python 3.12+**
