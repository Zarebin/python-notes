# The Exception Hierarchy  

One challenge of working with exceptions is managing the vast number of
exceptions that might potentially occur in your program. *For example*, there
are more than **60 built-in** exceptions alone.  
Exceptions aren’t recorded as part of a
function’s calling signature nor is there any sort of compiler to verify
correct exception handling in your code.As a result, exception handling can
sometimes feel haphazard and disorganized.


```python
try:
    item = items[index]
except IndexError:
    ...                       # Raised if items is a sequence
except KeyError:
    ...                        # Raised if items is a mapping

```

### Note:
Instead of writing code to handle two highly specific exceptions, it might
be easier to do this:  

```python
try:
    item = items[index]
except LookupError:
    ...

```
*LookupError* is a class that represents a higher-level grouping of
exceptions. *IndexError* and *KeyError* both inherit from *LookupError*, so this
except clause would catch either one. Yet, *LookupError* isn’t so broad as to
include errors unrelated to the lookup.  

## Exception Categories  

|Exception Class| Description|
|---------------|------------|
|BaseException |The root class for all exceptionsion|
|Exception |Base class for all program-related errors|
|Arithmetic Error |Base class for all math-related errors|
|ImportError |Base class for import-related errors|
|LookupError |Base class for all container lookup errors|
|OSError |Base class for all system-related errors. IOError and EnvironmentError are aliases.
|ValueError |Base class for value-related errors, including Unicode|
|UnicodeError |Base class for a Unicode string encoding-related errors|


The **BaseException** class is rarely used directly in exception handling
because it matches all possible exceptions whatsoever. This includes special
exceptions that affect the control flow such as *SystemExit*,
KeyboardInterrupt, and StopIteration. Catching these is rarely what you
want. Instead, all normal program-related errors inherit from *Exception*.
*ArithmeticError* is the base for all math-related errors such as
*ZeroDivisionError*, *FloatingPointError*, and *OverflowError*. *ImportError*
is a base for all import-related errors. *LookupError* is a base for all container
lookup-related errors. *OSError* is a base for all errors originating from the
operating system and environment. *OSError* encompasses a wide range of
exceptions related to files, network connections, permissions, pipes,
timeouts, and more. The *ValueError* exception is commonly raised when a
bad input value is given to an operation. *UnicodeError* is a subclass of
*ValueError* grouping all Unicode-related encoding and decoding errors.



## Exceptions and Control Flow  

Normally, exceptions are reserved for the handling of errors. However, a
few exceptions are used to alter the control flow. These exceptions, shown
in Table below inherit from BaseException directly.

|Exception Class |Description|
|---------------|------------|
|SystemExit | Raised to indicate program exit|
|KeyboardInterrupt |Raised when a program is interrupted via Control-C|
|StopIteration |Raised to signal the end of iteration|


### Note:
The SystemExit exception is used to make a program terminate on
purpose. As an argument, you can either provide an integer exit code or a
string message. If a string is given, it is printed to **sys.stderr** and the
program is terminated with an exit code of 1.

```python
import sys
if len(sys.argv) != 2:
    raise SystemExit(f'Usage: {sys.argv[0]} filename)

filename = sys.argv[1]

```

## Defining New Exceptions  

All the built-in exceptions are defined in terms of classes. To create a new
exception, create a new class definition that inherits from Exception, such
as the following:

```python
class HostnameError(NetworkError):
    pass
class TimeoutError(NetworkError):
    pass
def error1():
    raise HostnameError('Unknown host')
def error2():
    raise TimeoutError('Timed out')
try:
    error1()
except NetworkError as e:
    if type(e) is HostnameError:
        ...    # Perform special actions for this kind of error
```


## Chained Exceptions  

Sometimes, in response to an exception, you might want to raise a different
exception. To do this, raise a chained exception:  

```python
class ApplicationError(Exception):
    pass
def do_something():
    x = int('N/A')
    # raises ValueError
def spam():
    try:
        do_something()
    except Exception as e:
        raise ApplicationError('It failed') from e
```


**If an uncaught ApplicationError occurs, you will get a message that
includes both exceptions.**  

## Exception Tracebacks  

Exceptions have an associated stack traceback that provides information
about where an error occurred. The traceback is stored in the __traceback__
attribute of an exception. For the purposes of reporting or debugging, you
might want to produce the traceback message yourself. The traceback
module can be used to do this.  


## Exception Handling Advice  

Exception handling is one of the most difficult things to get right in larger
programs. However, there are a few rules of thumb that make it easier.
The first rule is to not catch exceptions that can’t be handled at that
specific location in the code. Consider a function like this:  

```python
def read_data(filename):
    with open(filename, 'rt') as file:
        rows = []
        for line in file:
            row = line.split()
            rows.append((row[0], int(row[1]), float(row[2]))
    return rows
```

Suppose the open() function fails due to a bad filename. Is this an error
that should be caught with a try-except statement in this function?
Probably not. If the caller gives a bad filename, there is no sensible way to
recover. There is no file to open, no data to read, and nothing else that’s
possible. It’s better to let the operation fail and report an exception back to
the caller. Avoiding an error check in read_data() doesn’t mean that the
exception would never be handled anywhere—it just means that it’s not the
role of read_data() to do it. Perhaps the code that prompted a user for a
filename would handle this exception.