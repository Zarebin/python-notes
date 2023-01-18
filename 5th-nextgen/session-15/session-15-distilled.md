# Context Managers and the with Statement

Proper management of system resources such as files, locks, and connections is often a tricky problem when combined with exceptions.

## With
The with statement allows a series of statements to execute inside a runtime context that is controlled by an object serving as a context manager.

```python
with open('debuglog', 'wt') as file:
 file.write('Debugging\n')
 statements
 file.write('Done\n')
import threading
lock = threading.Lock()
with lock:
 # Critical section
 statements
 # End critical section
```

### Note
the with statement automatically causes the opened file to be closed when the control flow leaves the block of statements that follow.

In the second example, the with statement automatically acquires and releases a lock when control enters and leaves the block of statements that follow.

* When the with obj statement executes, it calls the method
obj.__enter__() to signal that a new context is being entered. 
* When control flow leaves the context, the method obj.__exit__(type, value,traceback) executes. If no exception has been raised, the three arguments
* to __exit__() are all set to None. Otherwise, they contain the type, value,
* If the __exit__() method returns True, it
indicates that the raised exception was handled and should no longer be
propagated. Returning None or False will cause the exception to propagate

### Note
The with obj statement accepts an optional as var specifier. If given, the
value returned by obj.__enter__() is placed into var

```python
class Manager:
 def __init__(self, x):
 self.x = x
 def yow(self):
 pass
 def __enter__(self):
 return self
 def __exit__(self, ty, val, tb):
 pass
 ```
you can create and use an instance as a context manager in a
single step.
```python
with Manager(42) as m:
 m.yow()
```
```python
class ListTransaction:
 def __init__(self,thelist):
 self.thelist = thelist
 def __enter__(self):
 self.workingcopy = list(self.thelist)
 return self.workingcopy
 def __exit__(self,type,value,tb):
 if type is None:
 self.thelist[:] = self.workingcopy
 return False
```
```python
items = [1,2,3]
with ListTransaction(items) as working:
 working.append(4)
 working.append(5)
print(items) # Produces [1,2,3,4,5]
try:
 with ListTransaction(items) as working:
 working.append(6)
 working.append(7)
 raise RuntimeError("We're hosed!")
except RuntimeError:
 pass
print(items) # Produces [1,2,3,4,5]
```

# Assertions and \_\_debug\_\_
The assert statement can introduce debugging code into a program. The
general form of assert is :
```python 
assert test [, msg] 
```
where test is an expression that should evaluate to True or False. If test evaluates to False, assert raises an AssertionError exception.

```python
def write_data(file, data):
 assert file, 'write_data: file not defined!'
 ...
```
* The assert statement should not be used for code that must be executed
to make the program correct.
* A common use of assert is in testing
* The purpose of such a test is not to be exhaustive

```python
def factorial(n):
 result = 1
 while n > 1:
 result *= n
 n -= 1
 return result
assert factorial(5) == 120
```

# Objects, Types, and Protocols
Python programs manipulate objects of various types. There are a variety of built-in types such as numbers, strings, lists, sets, and dictionaries.

### Essential Concepts
Each object has an
identity, a type (also known as its class), and a value
* The type of an object, also known as the object’s class, defines the object’s internal data representation as well as supported methods.
* identity of the object is a number representing its location in memory.
* An attribute is a value
associated with an object that is accessed using the dot operator (.).

## Object Identity and Type
The built-in function id() returns the identity of an object.
is and is not operators compare the identities of two objects.
type() returns the type of an object
```python
# Compare two objects
def compare(a, b):
 if a is b:
 print('same object')
 if a == b:
 print('same value')
 if type(a) is type(b):
 print('same type')
```
```
>>> a = [1, 2, 3]
>>> b = [1, 2, 3]
>>> compare(a, a)
same object
same value
same type
>>> compare(a, b)
same value
same type
>>> compare(a, [4,5,6])
same type
>>>
```
The type of an object is itself an object, known as object’s class.

Classes usually have names (list, int, dict, and so on) that can
be used to create instances.
```python
items = list()
if isinstance(items, list):
 items.append(item)
def removeall(items: list, item) -> list:
 return [i for i in items if i != item]
```

A subtype is a type defined by inheritance. It carries all of the features of
the original type plus additional and/or redefined methods
```python
class mylist(list):
 def removeall(self, val):
 return [i for i in self if i != val]
# Example
items = mylist([5, 8, 2, 7, 2, 13, 9])
x = items.removeall(2)
print(x) # [5, 8, 7, 13, 9]
```
### Note
isinstance() method can also check against many possible types.
```python
if isinstance(items, (list, tuple)):
 maxval = max(items)
```


