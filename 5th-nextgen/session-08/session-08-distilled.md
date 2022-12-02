# Program Termination
+ A program terminates when no more statements exist to execute in the input program or when an uncaught SystemExit exception is raised.
+ for a specific cleanup action you can use _atexit_ module. 

# Objects and Classes
All values used in a program are objects.

To see all the methods of an object, we can use _dir()_ function.

e.g:

```python 
>>> items = [37, 42]
>>> dir(items)
['__add__', '__class__', '__contains__', '__delattr__',
'__delitem__',
...
'append', 'count', 'extend', 'index', 'insert', 'pop',
'remove', 'reverse', 'sort']
>>>
```

The class statement is used to define new types of objects and for object-oriented programming.
Inside the class definition, methods are defined using the def statement.

### private variable: 
+ They are only used inside the class.
+ It is defined by using a single underscore before the variable name.

### Inheritance:
Inheritance can also be used to change the behavior of an existing method.

e.g:

```python
class NumericStack(Stack):
def push(self, item):
if not isinstance(item, (int, float)):
raise TypeError('Expected an int or float')
super().push(item)
```

# Modules
+ As your programs grow in size, you will want to break them into multiple files for easier maintenance. To do this, use the _import_ statement.
+ The import statement creates a new namespace (or environment) and executes all the statements in it.
+ The dir() function lists the contents of a module:

e.g:

```python
>>> import readport
>>> dir(readport)['__builtins__', '__cached__', '__doc__', '__file__', '__loader__',
'__name__', '__package__', '__spec__', 'read_portfolio']
...
>>>


 
