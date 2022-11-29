##Iteration and Looping

The most widely used looping construct is the for statement that iterates
over a collection of items. One common form of iteration is to loop over all
the members of a sequence—such as a string, list, or tuple.

**__example:__**

```python
for n in [1, 2, 3, 4, 5, 6, 7, 8, 9]:
print(f'2 to the {n} power is {2**n}')

```
In this example, the variable n will be assigned successive items from the
list **[1, 2, 3, 4, ..., 9]** on each iteration. Since looping over ranges of
integers is quite common


**range():**
The **range(i, j [,step])** function creates an object that represents a
range of integers with values from i up to, but not including, j. If the
starting value is omitted, it’s taken to be zero.
example:

```python
for n in range(1, 10):
print(f'2 to the {n} power is {2**n}')

```

###Note:
The object created by range() computes the values it represents on
demand when lookups are requested. Thus, it’s efficient to use even with a
large range of numbers.

```python
a = range(5)
>>> a = 0, 1, 2, 3, 4
b = range(1, 8)
>>> b = 1, 2, 3, 4, 5, 6, 7
c = range(0, 14, 3)
>>> c = 0, 3, 6, 9, 12 
d = range(8, 1, -1)
>>> d = 8, 7, 6, 5, 4, 3, 2
```


The for statement is not limited to sequences of integers. It can be used
to iterate over many kinds of objects including strings, lists, dictionaries,
and files. Here’s an example:

```python
message = 'Hello World'
# Print out the individual characters in message
for c in message:
	print(c)
```

```python
prices = { 'GOOG' : 490.10, 'IBM' : 91.50, 'AAPL' : 123.15 }
# Print out all of the members of a dictionary
for key in prices:
	print(key, '=', prices[key])
```

##Functions

Use the def statement to define a function,To invoke a function, use its name followed by its arguments in
parentheses, for __example__ **result = remainder(37, 15)**.

**__example:__**
```python
def remainder(a, b):
	q = a // b
	# // is truncating division.
	r = a - q * b
	return r
```

###Note:
It is common practice for a function to include a documentation string as
the first statement. This string feeds the help() command and may be used
by IDEs and other development tools to assist the programmer. For
example:

```python
def remainder(a, b):
	'''
	Computes the remainder of dividing a by b
	'''
	q = a // b
	r = a - q * b
	return r
```

If the inputs and outputs of a function aren’t clear from their names, they
might be annotated with types:

```python
def remainder(a: int, b: int) -> int:
	'''
	Computes the remainder of dividing a by b
	'''
	q = a // b
	r = a - q * b
	return r
```
Such annotations are merely informational and are not actually enforced
at runtime. Someone could still call the above function with non-integer
values, such as result = remainder(37.5, 3.2).


Use a tuple to return multiple values from a function:
```python
def divide(a, b):
	q = a // b
	r = a - q * b
	return (q, r)
```

**To assign a default value to a function parameter, use assignment:**

```python
def connect(hostname, port, timeout=300):
	# Function body
	...
```


##Exceptions

If an error occurs in your program, an exception is raised and a traceback
message appears:
```python
Traceback (most recent call last):
	File "readport.py", line 9, in <module>
		shares = int(row[1])
ValueError: invalid literal for int() with base 10: 'N/A'
```
The traceback message indicates the type of error that occurred, along
with its location. Normally, errors cause a program to terminate. However,
you can catch and handle exceptions using try and except statements, like
this:

```python
try:
	...
	statements
	...

except ValueError as err:
	raise ValueError('Computer says no')

finally:
	...
	statements
	...

```

In this code, if a ValueError occurs, details concerning the cause of the
error are placed in err and control passes to the code in the except block. If
some other kind of exception is raised, the program crashes as usual. If no
errors occur, the code in the except block is ignored.

###Note:
The raise statement is used to signal an exception. You need to give the
name of an exception.




