# Section 1: Python Fundamentals for Data Engineering (Theory)

## Overview
This section covers the foundational Python skills every data engineer needs.

## Topics Covered
- Variables and Data Types
- Loops and Conditionals
- Functions and Modules
- Error Handling and Logging
- Working with Files (CSV, JSON, etc.)

---

## 1. Variables and Data Types
Python supports several core data types: `int`, `float`, `complex`, `str`, `bool`, `list`, `tuple`, `dict`, and `set`.

**Examples:**
```python
age = 30            # int
gpa = 3.75          # float
z = 2 + 3j          # complex
name = "Alice"      # str
is_active = True    # bool
scores = [90, 85, 92]  # list
colors = ("red", "green", "blue")  # tuple (immutable)
unique_ids = {101, 102, 103}        # set (unique values)
student = {"name": "Alice", "age": 30}  # dict (key-value pairs)
```
- **Tuple:** Immutable sequence, great for fixed collections.
- **Set:** Unordered, unique items, fast membership checks.
- **Dict:** Key-value store, essential for structured data.

**Mutability:**
- `list`, `dict`, `set` are mutable (can be changed in-place)
- `int`, `float`, `str`, `tuple`, `bool` are immutable

**String Operations:**
```python
s = "Data Engineering"
print(s.lower(), s.upper(), s[0:4], s[::-1])  # slicing, reversing
print(f"Hello {name}, age {age}")  # f-string
print("{} scored {}".format(name, scores[0]))
```

**Type Conversion:**
```python
num = int("42")
f = float("3.14")
s = str(123)
```

**Type Hints (Static Typing):**
```python
def add(a: int, b: int) -> int:
    return a + b
```
Useful for code clarity and tools like mypy.


---

## 2. Loops and Conditionals
Control flow is essential for data processing.

**If-Else, Elif, and Nested Conditionals:**
```python
if age < 13:
    print("Child")
elif age < 20:
    print("Teenager")
else:
    print("Adult")

# Nested
if age > 18:
    if is_active:
        print("Active adult")
    else:
        print("Inactive adult")

# Ternary expression
status = "Adult" if age > 18 else "Minor"
```

**Special Control Statements:**
- `pass`: Do nothing (placeholder)
- `continue`: Skip to next iteration
- `break`: Exit loop early

**Example:**
```python
for i in range(10):
    if i == 5:
        continue  # skip 5
    if i == 8:
        break     # stop at 8
    print(i)
```

**For Loop:**
```python
for score in scores:
    print(score)
```

**While Loop:**
```python
count = 0
while count < 5:
    print(count)
    count += 1
```

**Comprehensions:**
- List, dict, and set comprehensions are powerful for data transformation.
```python
squared = [x**2 for x in scores]  # list comprehension
score_map = {i: s for i, s in enumerate(scores)}  # dict comprehension
unique_even = {x for x in scores if x % 2 == 0}  # set comprehension
```

**Truthy/Falsy Values:**
- `if []`, `if 0`, `if None` are all False; non-empty, non-zero, non-None are True.

**Chained Comparisons:**
```python
if 18 <= age < 65:
    print("Working age")
```

**Pattern Matching (Python 3.10+):**
```python
value = "cat"
match value:
    case "cat":
        print("It's a cat!")
    case "dog":
        print("It's a dog!")
    case _:
        print("Unknown animal")
```

---

## 3. Functions and Modules
Functions help modularize code. Modules organize functions/classes into files.

**Defining a Function:**
```python
def greet(name: str) -> str:
    """Return a greeting for the given name."""
    return f"Hello, {name}!"
```

**Arguments:**
- Positional, keyword, default, `*args`, `**kwargs`
```python
def foo(a, b=2, *args, **kwargs):
    pass
```

**Lambda Functions:**
```python
add = lambda x, y: x + y
print(add(2, 3))
```

**Scope:**
- Local, global, nonlocal
```python
x = 10
def outer():
    x = 20
    def inner():
        nonlocal x
        x = 30
        print(x)
    inner()
    print(x)
outer()
print(x)
```

**Decorators (Intro):**
```python
def my_decorator(func):
    def wrapper(*args, **kwargs):
        print("Before")
        result = func(*args, **kwargs)
        print("After")
        return result
    return wrapper

@my_decorator
def say_hello():
    print("Hello!")

say_hello()
```

**Creating and Importing Custom Modules:**
- Save your functions in a file (e.g., `utils.py`):
```python
# utils.py
def add(a, b):
    return a + b
```
- Import and use in another file:
```python
from utils import add
print(add(2, 3))
```

**Import System:**
- Absolute vs. relative imports
- Built-in modules: `os`, `sys`, `pathlib`, `random`, `datetime`, etc.

**Virtual Environments:**
- Use `venv` or `conda` to isolate project dependencies
```bash
python -m venv .venv
source .venv/bin/activate  # or .venv\Scripts\activate on Windows
```

**Best Practices:**
- Use descriptive function names
- Write docstrings for functions (see below)
- Follow PEP8 style guidelines
- Comment your code where necessary

**Docstrings:**
```python
def foo(x):
    """
    Args:
        x (int): Description
    Returns:
        int: Description
    """
    return x * 2
```

---

## 4. Error Handling and Logging
Robust data pipelines handle errors and log events.

**Try-Except:**
```python
try:
    value = int(input("Enter a number: "))
except ValueError:
    print("Invalid input!")
```

**Multiple Excepts, Else, Finally:**
```python
try:
    # risky code
    pass
except ValueError:
    print("Value error!")
except Exception as e:
    print(f"Other error: {e}")
else:
    print("No errors occurred!")
finally:
    print("Always runs")
```

**Custom Exceptions:**
```python
class DataError(Exception):
    pass
raise DataError("Something went wrong!")
```

**Warnings:**
```python
import warnings
warnings.warn("This is a warning!")
```

**Print vs Logging:**
- Use `print()` for quick debugging, but `logging` for production/data pipelines.

**Logging:**
```python
import logging
logging.basicConfig(level=logging.INFO)
logging.info("Starting data pipeline...")
```

**Logging Levels and File Logging:**
- Levels: `DEBUG`, `INFO`, `WARNING`, `ERROR`, `CRITICAL`
- Log to a file:
```python
logging.basicConfig(filename='pipeline.log', level=logging.DEBUG,
                    format='%(asctime)s %(levelname)s:%(message)s')
logging.debug("This is a debug message")
```

---

## 5. Working with Files
Reading and writing files is fundamental.

**Context Managers:**
- Always use `with open(...) as f:` to ensure files are closed properly.

**CSV Example:**
```python
import csv
with open('data.csv', 'r', encoding='utf-8') as f:
    reader = csv.reader(f)
    for row in reader:
        print(row)
# Writing CSV with custom dialect
with open('data2.csv', 'w', newline='', encoding='utf-8') as f:
    writer = csv.writer(f, delimiter=';')
    writer.writerow(['a', 'b'])
```

**JSON Example:**
```python
import json
with open('data.json', 'r', encoding='utf-8') as f:
    data = json.load(f)
    print(data)
with open('data.json', 'w', encoding='utf-8') as f:
    json.dump({'x': 1}, f, indent=2)
```

**Other File Formats:**
- **TXT:**
```python
with open('notes.txt', 'w', encoding='utf-8') as f:
    f.write('Some notes')
```
- **Parquet:** Use `pyarrow` or `pandas` for columnar storage (covered in Section 2).
- **Binary:**
```python
with open('data.bin', 'wb') as f:
    f.write(b'\x00\x01')
```
- **Compressed:**
```python
import gzip
with gzip.open('data.txt.gz', 'wt', encoding='utf-8') as f:
    f.write('compressed text')
```

**File Encodings:**
- Always specify encoding (`utf-8` recommended)

**Directory Operations:**
```python
import os
os.listdir('.')
os.makedirs('data', exist_ok=True)
from pathlib import Path
Path('data').glob('*.csv')
```

**Gotchas:**
- Always close files (use context managers)
- Handle exceptions (file not found, permission error)

---

---

## 6. Best Practices for Python Code
- **PEP8:** Follow the official Python style guide for readable code. Use tools like `black` or `autopep8` to auto-format.
- **Linting:** Use `flake8` or `pylint` to catch errors and enforce style.
- **Testing:** Use `unittest` or `pytest` for automated testing.
- **Docstrings:** Document your functions and modules. Use Google, NumPy, or reStructuredText style.
- **Comments:** Explain complex logic, but avoid obvious comments.
- **Consistent naming:** Use snake_case for variables and functions.
- **Modularization:** Break code into small, reusable functions and modules.
- **Project Organization:**
  - Use folders like `src/`, `tests/`, `data/`
  - Keep configuration in `.env` or `config.yaml`
- **Version Control:** Use git for all projects.

---

## 7. Glossary & Cheat Sheet
- **Mutable:** Can be changed after creation (`list`, `dict`, `set`)
- **Immutable:** Cannot be changed after creation (`int`, `str`, `tuple`)
- **Comprehension:** Compact way to create lists, dicts, sets
- **Context Manager:** Handles setup/teardown (e.g., `with open()`)
- **Decorator:** Function that modifies another function
- **Virtual Environment:** Isolated Python environment for dependencies
- **Type Hint:** Syntax to specify expected types

**Cheat Sheet:**
```python
# List comprehension
[x for x in range(5) if x%2==0]
# Lambda
f = lambda x: x + 1
# Try-except
try:
    ...
except Exception as e:
    ...
# File
with open('file.txt') as f:
    data = f.read()
```

---

**References:**
- [Official Python Docs](https://docs.python.org/3/)
- [PEP8 Style Guide](https://pep8.org/)
- [Real Python Tutorials](https://realpython.com/)

---

**Next:**
- Try the hands-on lesson in `lesson.ipynb`
- Practice with exercises in `exercises.ipynb`
