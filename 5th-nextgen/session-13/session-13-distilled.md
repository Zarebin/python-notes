# Program Structure and Control Flow

This chapter covers the details of program structure and control flow.
Topics include conditionals, looping, exceptions, and context managers.

## Program Structure and Execution
Python programs are structured as a sequence of statements. All language
features, including variable assignment, expressions, function definitions,
classes, and module imports, are statements that have equal status with all
other statements—meaning that any statement can be placed almost
anywhere in a program (although certain statements such as return can
only appear inside a function). For example, this code defines two different
versions of a function inside a conditional:

```python
if debug:
 def square(x):
 if not isinstance(x,float):
 raise TypeError('Expected a float')
 return x * x
else:
 def square(x):
 return x * x
```

### Conditional Execution

The if, else, and elif statements control conditional code execution.

```python
if expression:
 statements
elif expression:
 statements
elif expression:
 statements
...
else:
 statements
```
### Note
Use the pass statement if no statements exist for a particular clause

```python
if expression:
 pass # To do: please implement
else:
 statements
```

### Loops and Iteration
You implement loops using the for and while statements.

```python
while expression:
 statements
for i in s:
 statements
```

* The while statement executes statements until the associated expression
evaluates to false
* The for statement iterates over all the elements of s until
no more elements are available

### Iteration Variable Scope
In for i s if we name i as iteration variable the The scope of the iteration variable is not private to the for statement. If a previously defined variable has the same name, that value will be overwritten.

### Iteration Variable unpacking
you can unpack their values into separate iteration variables using a statement

```python
s = [ (1, 2, 3), (4, 5, 6) ]
for x, y, z in s:
 statements
```

Sometimes a throw-away variable such as _ is used while unpacking.

```python
for x, _, z in s:
 statements
```

If the items produced by an iterable have varying sizes, you can use
wildcard unpacking to place multiple values into a variable.

```python
s = [ (1, 2), (3, 4, 5), (6, 7, 8, 9) ]
for x, y, *extra in s:
 statements # x = 1, y = 2, extra = []
 # x = 3, y = 4, extra = [5]
# x = 6, y = 7, extra = [8, 9]
# ...

for *first, x, y in s:
 ...

for x, *middle, y in s:
 ...

```
#### Note*
In this example, at least two values x and y are required, but *extra
receives any extra values that might also be present. These values are
always placed in a list. At most, only one starred variable can appear in a
single unpacking.

if you want to track iterable items index you can do it by using enumerat() method:

```python
for i, x in enumerate(s):
 statements
```

#### Note
* enumerate(s) creates an iterator that produces tuples (0, s[0]), (1, s[1]), (2, s[2]), ...
* with enumerate(s,start=k) we can produce (k,s[0]), (k+1, s[1]), ...



if you want to iterate on 2 iterables at the same time you can use zip():
```python
# s and t are two sequences
for x, y in zip(s, t):
 statements
```

## Break and Continue
To break out of a loop, use the break statement.
```python
with open('foo.txt') as file:
 for line in file:
 stripped = line.strip()
 if not stripped:
 break # A blank line, stop reading
 # process the stripped line
 ...
```
use the continue statement. This statement is useful when
reversing a test and indenting another level would make the program too
deeply nested or unnecessarily complicated.

```python
with open('foo.txt') as file:
 for line in file:
 stripped = line.strip()
 if not stripped:
 continue # Skip the blank line
 # process the stripped line
 ...
```
### Note
The else clause of a loop executes only if the loop runs to completion.
This either occurs immediately (if the loop wouldn’t execute at all) or after
the last iteration. If the loop is terminated early using the break statement,
the else clause is skipped.
```python
with open('foo.txt') as file:
 for line in file:
 stripped = line.strip()
 if not stripped:
 break
 # process the stripped line
 ...
 else:
 raise RuntimeError('Missing section separator')
 ```

 # Exceptions

### Note
Exceptions indicate errors and break out of the normal control flow of a
program. An exception is raised using the raise statement. The general
format of the raise statement is raise Exception([value]), where
Exception is the exception type and value is an optional value giving
specific details about the exception.

```python
raise RuntimeError('Unrecoverable Error')
```
To catch an exception, use the try and except statements.

```python
try:
 file = open('foo.txt', 'rt')
except FileNotFoundError as e:
 statements
```

### Note
If the raise statement is used by itself, the last exception generated is
raised again. This works only while handling a previously raised exception.

```python
try:
 file = open('foo.txt', 'rt')
except FileNotFoundError:
 print("Well, that didn't work.")
 raise # Re-raises current exception
```

Exceptions have a few standard attributes that might be useful in code
that needs to perform further actions in response to an error.

* e.args: The tuple of arguments supplied when raising the exception
* e.\_\_cause\_\_: Previous exception if the exception was intentionally raised in response to handling another exception.
* e.\_\_context\_\_: Previous exception if the exception was raised whilehandling another exception.
* e.\_\_traceback\_\_: Stack traceback object associated with the exception.

Multiple exception-handling blocks can be specified using multiple
except clauses.

```python
try:
 do something
except TypeError as e:
 # Handle Type error
 ...
except ValueError as e:
 # Handle Value error
 ...

```

A single handler clause can catch multiple exception types.
```python
try:
 do something
except (TypeError, ValueError) as e:
 # Handle Type or Value errors
 ...
```

The try statement also supports an else clause, which must follow the
last except clause. This code is executed if the code in the try block
doesn’t raise an exception.

```python
try:
 file = open('foo.txt', 'rt')
except FileNotFoundError as e:
 print(f'Unable to open foo : {e}')
 data = ''
else:
 data = file.read()
 file.close()
```

The finally statement defines a cleanup action that must execute
regardless of what happens in a try-except block.
```python
file = open('foo.txt', 'rt')
try:
 # Do some stuff
 ...
finally:
 file.close()
 # File closed regardless of what happened
```
### Note
The finally clause isn’t used to catch errors. Rather, it’s used for code
that must always be executed, regardless of whether an error occurs. If no
exception is raised, the code in the finally clause is executed immediately
after the code in the try block. If an exception occurs, a matching except
block (if any) is executed first and then control is passed to the first
statement of the finally clause. If, after this code has executed, an
exception is still pending, that exception is reraised to be caught by another exception handler.
