#    reference counting

## Reference Counting and Garbage Collection


Python manages objects through automatic garbage collection. All objects
are reference-counted.

An object’s reference count is increased whenever
it’s assigned to a new name or placed in a container such as a list, tuple, or
dictionary.

## example
```python
a = 37         # Creates an object with value 37
b = a          # Increases reference count on 37
c = []
c.append(b)    # Increases reference count on 37
```

This example creates a single object containing the value 37. a is a name
that initially refers to the newly created object. When b is assigned a, b
becomes a new name for the same object, and the object’s reference count
increases. When you place b into a list, the object’s reference count
increases again.



---
An object’s reference count is decreased by the del statement or
whenever a reference goes out of scope or is reassigned

```python

del a         # Decrease reference count of 37
b = 42        # Decrease reference count of 37
c[0] = 2.0    # Decrease reference count of 37
```
---
---

The current reference count of an object can be obtained using the
sys.getrefcount() 

## For example:
```python
>>> a = 37
>>> import sys
>>> sys.getrefcount(a)
7
>>>

```
The reference count is often much higher than you might expect.
For
immutable data such as numbers and strings, the interpreter aggressively
shares objects between different parts of the program in order to conserve
memory.

---
---

# garbage-collected
## When an object’s reference count reaches zero, it is garbage-collected.
However, in some cases a circular dependency may exist in a collection of
objects that are no longer in use.
 ## example:
 
```python
a = { }
b = { }
a['b'] = b #     a contains reference to b
b['a'] = a #     b contains reference to a
del a
del b
```

In this example, the del statements decrease the reference count of a and
b and destroy the names used to refer to the underlying objects. However,
since each object contains a reference to the other, the reference count
doesn’t drop to zero and the objects remain allocated. The interpreter won’t
leak memory, but the destruction of the objects will be delayed until a cycle
detector executes to find and delete the inaccessible objects.

---
---

#  References and Copies

## When a program makes an assignment such as b = a, a new reference to a is created.

##  For immutable objects such as numbers and strings, this assignment appears to create a copy of a (even though this is not the case).

## However, the behavior appears quite different for mutable objects such as lists and dictionaries. Here’s an example:

```python

>>> a = [1,2,3,4]
>>> b = a           # b is a reference to a
>>> b is a
True
>>> b[2] = -100     # Change an element in b
>>> a               # Notice how a also changed
[1, 2, -100, 4]
>>>
```


(a) and (b) refer to the same object in this example, a change made to
one of the variables is reflected in the other. To avoid this, you have to
create a copy of an object rather than a new reference.


## Two types of copy operations are applied to container

Two types of copy operations are applied to container:
* shallow copy 
* deep copy

A shallow copy
creates a new object, but populates it with references to the items contained
in the original object

## example:
```python
>>> a = [ 1, 2, [3,4] ]
>>> b = list(a)             # Create a shallow copy of a.
>>> b is a
False
>>> b.append(100)           # Append element to b.
>>> b
[1, 2, [3, 4], 100]
>>> a                       # Notice that a is unchanged
[1, 2, [3, 4]]
>>> b[2][0] = -100          # Modify an element inside b
>>> b
[1, 2, [-100, 4], 100]
>>> a                       # Notice the change inside a
[1, 2, [-100, 4]]
>>>

```
In this case, (a) and (b) are separate list objects, but the elements they
contain are shared. Therefore, a modification to one of the elements of (a)
also modifies an element of (b)

---

A deep copy creates a new object and recursively copies all the objects it
contains.
```python

>>> import copy
>>> a = [1, 2, [3, 4]]
>>> b = copy.deepcopy(a)
>>> b[2][0] = -100
>>> b
[1, 2, [-100, 4]]
>>> a                   # Notice that a is unchanged
[1, 2, [3, 4]]
>>>
```
deepcopy() for situations
where you actually need a copy because you’re about to mutate data and
you don’t want your changes to affect the original object. 

---
---

# Object Representation and Printing
Programs often need to display objects—for example, to show data to the
user or print it for the purposes of debugging.

If you supply an object x to
the print(x) function or convert it to a string using str(x)

To get more information, use the repr(x)
function that creates a string with a representation of the object that you
would have to type out in source code to create it.
## for example
```python
>>> d = date(2012, 12, 21)
>>> repr(d)
'datetime.date(2012, 12, 21)'
>>> print(repr(d))
datetime.date(2012, 12, 21)
>>> print(f'The date is: {d!r}')
The date is: datetime.date(2012, 12, 21)
>>>

```

---
---

# First-Class Objects

All objects in Python are said to be first-class. This means that all objects
that can be assigned to a name can also be treated as data.
 
## As :
* data
* objects
* can be stored as variables
* passed as arguments
* returned from functions
* compared against other objects
* and more.


## example
```python
items = {
 'number' : 42
 'text' : "Hello World"
}

items['func'] = abs             # Add the abs() function
import math
items['mod'] = math             # Add a module
items['error'] = ValueError     # Add an exception type
nums = [1,2,3,4]
items['append'] = nums.append   # Add a method of another object


```

In this example, the items dictionary now contains a function, a module,
an exception, and a method of another object. If you want, you can use
dictionary lookups on items in place of the original names and the code
will still work. For example:

```python
>>> items['func'](-45)              # Executes abs(-45)
45
>>> items['mod'].sqrt(4)            # Executes math.sqrt(4)
2.0
>>> try:
...     x = int('a lot')
... except items['error'] as e:     # Same as except ValueError as e
...     print("Couldn't convert")
...
Couldn't convert
>>> items['append'](100)            # Executes nums.append(100)
>>> nums
[1, 2, 3, 4, 100]
>>>


```

---
---

# Object Protocols and Data Abstraction

Most Python language features are defined by protocols. Consider the
following function:

```python
def compute_cost(unit_price, num_units):
    return unit_price * num_units


```

Now, ask yourself the question: What inputs are allowed? The answer is
deceptively simple—everything is allowed.
 
* fractions or decimals
* arrays and other complex structures
* unexpected ways



fractions or decimals
```python
>>> from fractions import Fraction
>>> compute_cost(Fraction(5, 4), 50)
Fraction(125, 2)
>>> from decimal import Decimal
>>> compute_cost(Decimal('1.25'), Decimal('50'))
Decimal('62.50')
>>>
```
arrays and other complex structures
```python
>>> import numpy as np
>>> prices = np.array([1.25, 2.10, 3.05])
>>> units = np.array([50, 20, 25])
>>> compute_cost(prices, quantities)
array([62.5 , 42. , 76.25])
>>>
```

```python
>>> compute_cost('a lot', 10)
'a lota lota lota lota lota lota lota lota lota lot'
>>>
```














