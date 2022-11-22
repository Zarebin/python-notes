# Conditionals and Control Flow
The **while**, **if** and **else** statements are used for looping and conditional code execution. 
### Notes
* **pass** statement: creating an empty clause
* **elif** statement: handling a multiple-test cases
* **braek** statement: aborting a loop early
* **continue** statement: skips the rest of the loop body and goes back to the top of the loop
# Text String
To define a string literal, enclose it in single, double, or triple quotes.
## String Formatting
* f-string
e.g:

```python
print(f'{year:>3d} {principal:0.2f}')
```
* format()
e.g:

```python
print('{0:>3d} {1:0.2f}'.format(year, principal))
```
e.g:

```python
print('%3d %0.2f' % (year, principal))
```
## Create String
* str()
* repr()

both of them are creating strings but their output is often different from each others.
# File Input and Output
The open() function returns a new file object.

The with statement that precedes it declares a block of statements (or context) where the file (file)is going to be used.

Once control leaves this block, the file is automatically closed.

# Lists
Lists are an ordered collection of arbitrary objects. Create a list byenclosing values in square brackets.

Lists are indexed by integers, starting with zero.
## List Methods
* append(): adds an element at the end of the list.
* insert(): adds an element at the specified position.
### Notes 
* Use the plus (+) operator to concatenate lists:
e.g:

```python
a = ['x','y'] + ['z','z','y'] # Result is ['x','y','z','z','y']
```
* Creating an empty list:
e.g: 

```python
names = [] 
names = list() 
```
Specifying [] for an empty list is more idiomatic. list is the name of the class associated with the list type.
