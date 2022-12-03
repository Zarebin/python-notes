# Multidimensional Slicing and Ellipsis
[] operator can take multiple indexes or slices separated by commas. __getitem__ and __setitem__ special methods that handle the [] operator, receive the indices as a tuple.
e.g:
```python
a[i, j] --> a.__getitem__((i, j))
```

All built-in sequence types in Python are one-dimensional (except for memoryview), so they support only one index or slice.  
The ellipsis—written with three full stops (...)- can be passed as an argument to
functions and as part of a slice specification.  
NumPy uses ... as a shortcut when slicing arrays of many dimensions. e.g:
```python
x[i, ...] --> x[i, :, :, :,]
```
# Assigning to Slices
Mutable sequences can be grafted, excised, and otherwise modified in place using slice notation.
```python
>>> l = list(range(10))
>>> l[2:5] = [20, 30]
>>> l
[0, 1, 20, 30, 5, 6, 7, 8, 9]
>>> del l[5:7]
>>> l
[0, 1, 20, 30, 5, 8, 9]
>>> l[3::2] = [11, 22]
>>> l
[0, 1, 20, 11, 5, 22, 9]
>>> l[2:5] = 100
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
TypeError: can only assign an iterable
>>> l[2:5] = [100]
>>> l
[0, 1, 100, 22, 9]
```

**Note**
When the target of the assignment is a slice, the righthand side must be an iterable object, even if it has just one item. Otherwise you'll receive an error.

# Using + and * with Sequences
* Usually both operands of + must be of the same sequence type
* neither of them is modified
* a new sequence of that same type is created as result

To concatenate multiple copies of the same sequence, **multiply it by an integer** e.g:
```python
>>> l = [1, 2, 3]
>>> l * 5
[1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3]
```

Both + and * always **create a new object**.

# Building Lists of Lists
You should be careful in using expressions like a * n when a is a sequence containins mutable items. We'll see an example of tic-tac-toe board.
 In these two examples, lists **behave as expected**.
```python
>>> board = [['_'] * 3 for i in range(3)]
>>> board
[['_', '_', '_'], ['_', '_', '_'], ['_', '_', '_']]
>>> board[1][2] = 'X'
>>> board
[['_', '_', '_'], ['_', '_', 'X'], ['_', '_', '_']]
```

another way of building the same list:
```python
board = []
>>> for i in range(3):
... row = ['_'] * 3
... board.append(row)
...
>>> board
[['_', '_', '_'], ['_', '_', '_'], ['_', '_', '_']]
>>> board[2][0] = 'X'
>>> board
[['_', '_', '_'], ['_', '_', '_'], ['X', '_', '_']]
```
Each iteration builds a new row and appends it to board.

Now we'll see two examples that are implementations with **differenct behavior**.

```python
>>> weird_board = [['_'] * 3] * 3
>>> weird_board
[['_', '_', '_'], ['_', '_', '_'], ['_', '_', '_']]
>>> weird_board[1][2] = 'O'
>>> weird_board
[['_', '_', 'O'], ['_', '_', 'O'], ['_', '_', 'O']]
```
A list with **three references to the same list** is made which is useless. Implementing the weird_board using for loop will be like:

```python
row = ['_'] * 3
board = []
for i in range(3):
    board.append(row)
```

# Augmented Assignment with Sequences
They behave quite differently, depending on the first operand.  
The special method that makes += work is __iadd__ (in-place addition). If __iadd__ is not implemented, Python falls back to calling __add__. e.g:
```python
>>> a += b
```
If a implements __iadd__, that will be called. In the case of mutable sequences, a will be changed in place.  
Otherwise, the expression has the same effect as a = a + b : a + b is evaluated first, producing a new object, bound to a.  
The identity of the object may or may not change and it depends on the availability of __iadd__.  
Generally, it is a good bet that __iadd__ is implemented.  

*= is implemented via __imul__.
```python
l = [1, 2, 3]
l *= 2
```
After multiplication, the list is the same object, with new items appended. Its id does not change.

```python
t = (1, 2, 3)
t *= 2
```
After multiplication, a new tuple is created. Also, the id will change.  

**Note:** Repeated concatenation of immutable sequences is inefficient.

## Interesting example
```python
>>> t = (1, 2, [30, 40])
>>> t[2] += [50, 60]
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
>>> t
(1, 2, [30, 40, 50, 60])
```
In this example, although we get an error, the assignment will be done. Checking the bytecode through `dis.dis(s[a] += b)`, shows that:
1. the value of `s[a]` is on TOS.
2. `TOS += b` succeeds because TOS refers to a mutable object.
3. Assign `s[a] = TOS` fails because s is immutable.
So:
* Avoid putting mutable items in tuples.
* Augmented assignment is not an atomic operation.
* Inspect Python bytecode.

# list.sort Versus the sorted Built-In
two optional, keyword-only arguments:
1. reverse: default is False. If True, the items are returned in descending order.
2. key: A one-argument function that will be applied to each item. Default is the identity function.

list.sort:
* in-place sorting
* without making a copy
* returns `None` (so we cannot cascade calls to thois method)
sorted:
* creates a new list
* returns it
* accepts any iterable object as an argument

**Note:** Python’s sorting algorithm is stable (it preserves the relative ordering of items that compare equally).
e.g:
```python
>>> fruits = ['grape', 'raspberry', 'apple', 'banana']
>>> sorted(fruits)
['apple', 'banana', 'grape', 'raspberry']
>>> fruits
['grape', 'raspberry', 'apple', 'banana']
>>> sorted(fruits, reverse=True)
['raspberry', 'grape', 'banana', 'apple']
>>> sorted(fruits, key=len)
['grape', 'apple', 'banana', 'raspberry']
>>> sorted(fruits, key=len, reverse=True)
['raspberry', 'banana', 'grape', 'apple']
>>> fruits
['grape', 'raspberry', 'apple', 'banana']
>>> fruits.sort()
>>> fruits
['apple', 'banana', 'grape', 'raspberry']
```

Once sorted, it can be very efficiently searched. binary search is provided in `bisect` module. By using `bisect.insort` you can make sure that your sorted sequences will stay sorted.

# When a List Is Not the Answer
Although list is flexible and easy to use, depending on specific requirements, there are better options. e.g:
* `array` saves a lot of memory when you need to **handle millions of floating-point values**.
* **constantly adding and removing items from opposite ends** of a list, a `deque` (double-ended queue) is a more efficient FIFO to be used.
* **frequently checking whether an item is present**, consider using a `set`, especially if it holds a large number of items. Sets are also iterable,
but they are not sequences 

## Array
* If a list only contains numbers, an array.array is more efficient.
* supports all mutable sequence operations.
* additional methods for fast loading and saving, such as .frombytes and .tofile
* as lean as a C array
* provide a typecode, a letter to determine the underlying C type used to store each item (e.g 'b' is used for signed char)
*  not allowed to put any number that does not match the type 

example:
```python
>>> from array import array
>>> from random import random
>>> floats = array('d', (random() for i in range(10**7)))
>>> fp = open('floats.bin', 'wb')
>>> floats.tofile(fp)
>>> fp.close()
>>> floats2 = array('d')
>>> fp = open('floats.bin', 'rb')
>>> floats2.fromfile(fp, 10**7)
>>> fp.close()
>>> floats2 == floats
True
```

It is not only easy to use, but also very fast:
* about 0.1 seconds for array.fromfile to load 10 million double-precision floats from a binary file (60 times faster than reading the numbers from a text file)
* Saving with array.tofile is about seven times faster than writing one float per line in a text file.
* the size of the binary file with 10 million doubles is 80,000,000 bytes, while the text file has 181,515,739 bytes for the same data.

Array type does not have an in-place sort. Use the built-in sorted function:
```python
a = array.array(a.typecode, sorted(a))
```