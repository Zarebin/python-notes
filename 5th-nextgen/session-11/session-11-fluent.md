# Memory Views  

The built-in *memoryview* class is a shared-memory sequence type that lets you handle
slices of arrays without copying bytes. It was inspired by the NumPy library.  

### Note  
A memoryview is essentially a generalized NumPy array structure in Python itself
(without the math). It allows you to share memory between data-structures (things like
PIL images, SQLite databases, NumPy arrays, etc.) without first copying. This is very
important for large data sets.  

Using notation similar to the array module, the *memoryview.cast* method lets you
change the way multiple bytes are read or written as units without moving bits
around. *memoryview.cast* returns yet another memoryview object, always sharing the
same memory.  

**Handling 6 bytes of memory as 1×6, 2×3, and 3×2 views:**

```python
from array import array

octets = array('B', range(6))
m1 = memoryview(octets)
m1.tolist()
>>> [0, 1, 2, 3, 4, 5]
m2 = m1.cast('B', [2, 3])
m2.tolist()
>>> [[0, 1, 2], [3, 4, 5]]
m3 = m1.cast('B', [3, 2])
m3.tolist()
>>> [[0, 1], [2, 3], [4, 5]]
m2[1,1] = 22
m3[1,1] = 33
>>> octets
>>> array('B', [0, 1, 2, 33, 22, 5])

```

* Build array of 6 bytes (typecode 'B').
* Build memoryview from that array, then export it as a list.
* Build new memoryview from that previous one, but with 2 rows and columns.
* Yet another memoryview, now with 3 rows and 2 columns.
* Overwrite byte in m2 at row 1, column 1 with 22.
* Overwrite byte in m3 at row 1, column 1 with 33.
* Display original array, proving that the memory was shared among octets, m1, m2, and m3.


The awesome power of memoryview can also be used to corrupt. *Example below* shows
how to change a single byte of an item in an array of *16-bit* integers:

```python
numbers = array.array('h', [-2, -1, 0, 1, 2])
memv = memoryview(numbers)
len(memv)
>>> 5
memv[0]
>>> -2
memv_oct = memv.cast('B')
memv_oct.tolist()
>>> [254, 255, 255, 255, 0, 0, 1, 0, 2, 0]
memv_oct[5] = 4
numbers
>>> array('h', [-2, -1, 1024, 1, 2])
```

* Build memoryview from array of 5 16-bit signed integers (typecode 'h').
* memv sees the same 5 items in the array.
* Create memv_oct by casting the elements of memv to bytes (typecode 'B').
* Export elements of memv_oct as a list of 10 bytes, for inspection.
* Assign value 4 to byte offset 5.
* Note the change to numbers: a 4 in the most significant byte of a 2-byte unsigned
integer is 1024.


## NumPy  

Throughout this book, I make a point of highlighting what is already in the Python
standard library so you can make the most of it. But NumPy is so awesome that a
detour is warranted.  
For advanced array and matrix operations, NumPy is the reason why Python became
mainstream in scientific computing applications. NumPy implements multi-
dimensional, homogeneous arrays and matrix types that hold not only numbers but
also user-defined records, and provides efficient element-wise operations.  

**Basic operations with rows and columns in a numpy.ndarray:**

```python
import numpy as np

a = np.arange(12)
a
>>> array([ 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11])
type(a)
>>> <class 'numpy.ndarray'>
a.shape
>>> (12,)
a.shape = 3, 4
a
>>> array([[ 0, 1, 2, 3],
[ 4, 5, 6, 7],
[ 8, 9, 10, 11]])
a[2]
>>> array([ 8, 9, 10, 11])
a[2, 1]
>>> 9
a[:, 1]
>>> array([1, 5, 9])
a.transpose()
>>> array([[ 0, 4, 8],
[ 1, 5, 9],
[ 2, 6, 10],
[ 3, 7, 11]])
```

* Import NumPy, after installing (it’s not in the Python standard library). Conven‐
tionally, numpy is imported as np.
* Build and inspect a numpy.ndarray with integers 0 to 11.
* Inspect the dimensions of the array: this is a one-dimensional, 12-element array.
* Change the shape of the array, adding one dimension, then inspecting the result.
* Get row at index 2.
* Get element at index 2, 1.
* Get column at index 1.
* Create a new array by transposing (swapping columns with rows).


### Note:  
NumPy also supports high-level operations for loading, saving, and operating on all
elements of a *numpy.ndarray:*  

```python
import numpy

floats = numpy.loadtxt('floats-10M-lines.txt')
floats[-3:]
>>> array([ 3016362.69195522,
535281.10514262, 4566560.44373946])
floats *= .5
floats[-3:]
>>> array([ 1508181.34597761,
267640.55257131, 2283280.22186973])
from time import perf_counter as pc
t0 = pc(); floats /= 3; pc() - t0
>>> 0.03690556302899495
numpy.save('floats-10M', floats)
floats2 = numpy.load('floats-10M.npy', 'r+')
floats2 *= 6
floats2[-3:]
>>> memmap([ 3016362.69195522,
535281.10514262, 4566560.44373946])
```

* Load 10 million floating-point numbers from a text file.
* Use sequence slicing notation to inspect the last three numbers.
* Multiply every element in the floats array by .5 and inspect the last three
elements again.
* Import the high-resolution performance measurement timer (available since
Python 3.3).
* Divide every element by 3; the elapsed time for 10 million floats is less than 40
milliseconds.
* Save the array in a .npy binary file.
* Load the data as a memory-mapped file into another array; this allows efficient
processing of slices of the array even if it does not fit entirely in memory.
* Inspect the last three elements after multiplying every element by 6.


## Deques and Other Queues  

The *.append* and *.pop* methods make a list usable as a stack or a queue (if you
use *.append* and *.pop(0)*, you get FIFO behavior). But inserting and removing from
the head of a list (the 0-index end) is costly because the entire list must be shifted in
memory.  
The class *collections.deque* is a thread-safe double-ended queue designed for fast
inserting and removing from both ends.  

***Example below* shows some typical operations performed on a deque:**

```python
from collections import deque

dq = deque(range(10), maxlen=10)
dq
>>> deque([0, 1, 2, 3, 4, 5, 6, 7, 8, 9], maxlen=10)
dq.rotate(3)
dq
>>> deque([7, 8, 9, 0, 1, 2, 3, 4, 5, 6], maxlen=10)
dq.rotate(-4)
dq
>>> deque([1, 2, 3, 4, 5, 6, 7, 8, 9, 0], maxlen=10)
dq.appendleft(-1)
dq
>>> deque([-1, 1, 2, 3, 4, 5, 6, 7, 8, 9], maxlen=10)
dq.extend([11, 22, 33])
dq
>>> deque([3, 4, 5, 6, 7, 8, 9, 11, 22, 33], maxlen=10)
dq.extendleft([10, 20, 30, 40])
dq
>>>> deque([40, 30, 20, 10, 3, 4, 5, 6, 7, 8], maxlen=10)
```

* The optional maxlen argument sets the maximum number of items allowed in
this instance of deque; this sets a read-only maxlen instance attribute.
* Rotating with n > 0 takes items from the right end and prepends them to the
left; when n < 0 items are taken from left and appended to the right.
* Appending to a deque that is full (len(d) == d.maxlen) discards items from the
other end; note in the next line that the 0 is dropped.
* Adding three items to the right pushes out the leftmost -1, 1, and 2.
* Note that extendleft(iter) works by appending each successive item of the
iter argument to the left of the deque, therefore the final position of the items is
reversed.



### Note:  
Note that deque implements most of the list methods, and adds a few that are specific to its design, like popleft and rotate. But there is a hidden cost: removing
items from the middle of a deque is not as fast. It is really optimized for appending
and popping from the ends.
The append and popleft operations are atomic, so deque is safe to use as a FIFO
queue in multithreaded applications without the need for locks.

















