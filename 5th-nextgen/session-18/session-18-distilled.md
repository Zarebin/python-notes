# Container Protocol
Methods for Containers:

| Method                              | Description                    |
| ----------------------------------- | : --------------------------- :|
| `__len__(self)`                     | Returns the length of self     |
| `__getitem__(self, key)`            | Returns `self [key]`           |
| `__setitem__(self, key, value)`     | Sets `self [key] = value`      |
| `__delitem__(self, key)`            | Deletes `self [key]`           |
| `__contains__(self, obj)`           | obj in self                    |

**Example**:

```python
a = [1, 2, 3, 4, 5, 6]
len(a)        # a.__len__()
x = a[2]      # x = a.__getitem__(2)
a[1] = 7      # a.__setitem__(1,7)
del a[2]      # a.__delitem__(2)
5 in a        # a.__contains__(5)
```
Slicing operations are also implemented using `__getitem__()`, `__setitem__()`, and `__delitem__()`.

**Example:**

```python
a = [1,2,3,4,5,6]
x = a[1:5]              # x = a.__getitem__(slice(1, 5, None))
a[1:3] = [10,11,12]     # a.__setitem__(slice(1, 3, None), [10, 11,12])
del a[1:4]              # a.__delitem__(slice(1, 4, None))
```
______
# Iteration Protocol
If an instance of object supports iteration, it provides a method, obj.`__iter__()`.
An iterator implement a single method `iter.__next__()` that returns the next object or raises
**StopIteration** to signal the end of iteration (like for loop).

**Example:**
```python
_iter = s.__iter__()
while True:
  try:
    x = _iter.__next__()
  except StopIteration:
    break
  # Do statements in body of for loopnotes
```
**Notes**

+ An object may optionally provide a reversed iterator if it implements the `__reversed__()` special method.
+ A common implementation technique for iteration is to use a generator function involving yield.

_______
# Attribute Protocol
Methods for Attribute Access:

| Method                               | Description                                                                      |
| ------------------------------------ | ------------------------------------------------------------------------------- :|
| `__getattribute__(self, name)`       | Returns the attribute `self.name`                                                |
| `__getattr__(self, name`             | Returns the attribute `self.name` if it's not found through `__getattribute__`   |
| `__setattr__(self, name, value)`     | Sets the attribute `self.name = value`                                           |
| `__delattr__(self, name)`            | Deletes the attribute del `self.name`                                            |

________
# Function Protocol
An object can emulate a function by providing the `__call__()` method. If an
object, x, provides this method, it can be invoked like a function. That is,
x(arg1, arg2, ...) invokes `x.__call__(arg1, arg2, ...)`.
_______
# Context Manager Protocol
The with statement allows a sequence of statements to execute under the
control of an instance known as a context manager.

**Example:**

```python
with context [ as var]:
  statements
```
Methods for Context Managers:
+ `__enter__(self)`: Called when entering a new context. The return value is placed in the variable listed with the as specifier to the with statement.
+ `__exit__(self, type, value, tb)`: Called when leaving a context. If an exception occurred,
   type, value, and tb have the exception type, value, and traceback information.
_____
_____
# Functions
Functions are a fundamental building block of most Python programs.
_______
# Function Definitions

```python
def add(x, y):
  return x + y
```
_______

# Default Arguments
You can attach default values to function parameters by assigning values in the function definition.

Default parameter values are evaluated once when the function is first defined, not each time the function is called.

**Example:**

```python
def func(x, items=[]):
  items.append(x)
  return items
func(1)     # returns  [1]
func(2)     # returns  [1, 2]
func(3)     # returns  [1, 2, 3]
```
Notice how the default argument retains the modifications made from previous invocations. To prevent this, it is better to use None.

**Example:**

```python
def func(x, items=None):
  if items is None:
    items = []
  items.append(x)
  return items
```
 
Avoiding such surprises, only use immutable objects for default argument.
