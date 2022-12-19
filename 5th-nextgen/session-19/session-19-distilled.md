# Variadic Arguments  

A function can accept a variable number of arguments if an asterisk (*) is
used as a prefix on the last parameter name. _For example:_

```python
def product(first, *args):
    result = first
    for x in args:
        result = result * x
    return result


product(10, 20)         # -> 200
product(2, 3, 4, 5)     # -> 120

```

## Keyword Arguments  

Function arguments can be supplied by explicitly naming each parameter
and specifying a value. These are known as keyword arguments. Here is an
_example:_  


```python
def func(w, x, y, z):
    statements

# Keyword argument invocation
func(x=3, y=22, w='hello', z=[1, 2])
```

### Note:  
With keyword arguments, the order of the arguments doesn’t matter as
long as each required parameter gets a single value. If you omit any of the
required arguments or if the name of a keyword doesn’t match any of the
parameter names in the function definition, a _**TypeError**_ exception is raised.
Keyword arguments are evaluated in the same order as they are specified in
the function call.  


## Variadic Keyword Arguments  

If the last argument of a function definition is prefixed with **, all the
additional keyword arguments (those that don’t match any of the other
parameter names) are placed in a dictionary and passed to the function. The
order of items in this dictionary is guaranteed to match the order in which
keyword arguments were provided.  


```python
def make_table(data, **parms):
# Get configuration parameters from parms (a dict)
    fgcolor = parms.pop('fgcolor', 'black')
    bgcolor = parms.pop('bgcolor', 'white')
    width = parms.pop('width', None)
    ...
# No more options
    if parms:
        raise TypeError(f'Unsupported configuration options
                {list(parms)}')


make_table(items, fgcolor='black', bgcolor='white', border=1,
borderstyle='grooved', cellpadding=10,
width=400)
```

## Functions Accepting All Inputs  

By using both * and **, you can write a function that accepts any
combination of arguments. The positional arguments are passed as a tuple
and the keyword arguments are passed as a dictionary. _For example:_  

```python
# Accept variable number of positional or keyword arguments
def func(*args, **kwargs):
    # args is a tuple of positional args
    # kwargs is dictionary of keyword args
    ...
```

### Note:  
This combined use of *args and **kwargs is commonly used to write
wrappers, decorators, proxies, and similar functions.  

## Positional-Only Arguments  

Many of Python’s built-in functions only accept arguments by position.
You’ll see this indicated by the presence of a slash (/) in the callingsignature of a function shown by various help utilities and IDEs. _For
example_, you might see something like _func(x, y, /)_. This means that all
arguments appearing before the slash can only be specified by position.
Thus, you could call the function as _func(2, 3)_ but not as _func(x=2, y=3)_.
For completeness, this syntax may also be used when defining functions.
_For example_, you can write the following:  

```python
def func(x, y, /):
    pass


func(1, 2)      # Ok
func(1, y=2)    # Error

```

## Names, Documentation Strings, and Type Hints  

The standard naming convention for functions is to use lowercase letters
with an underscore ( _ ) used as a word separator.  
The name of a function can be obtained via the __name__ attribute. This is
sometimes useful for debugging.It is common for the first statement of a function to be a documentation string describing its usage.  

```python
def factorial(n):
    '''
    Computes n factorial. For example:
    >>> factorial(6)
    120
    '''
    if n <= 1:
        return 1
    else:
        return n*factorial(n-1)
```

### Note:  
The documentation string is stored in the __doc__ attribute of the
function. It’s often accessed by IDEs to provide interactive help.  


## Return Values  

The return statement returns a value from a function. If no value is
specified or you omit the return statement, None is returned. To return
multiple values, place them in a tuple:  

```python
def parse_value(text):
    '''
    Split text of the form name=val into (name, val)
    '''
    parts = text.split('=', 1)
    return (parts[0].strip(), parts[1].strip())


name, value = parse_value('url=http://www.python.org')
```

**Values returned in a tuple can be unpacked to individual variables**































