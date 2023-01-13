# Type Hinting
Type hinting is a formal solution to statically indicate the type of a value within your Python code.

**Example:**

```python
class Account:
  owner: str            # Type hint
  _balance: float       # Type hint
  
  def __init__(self, owner, balance):
    self.owner = owner
    self._balance = balance
```

**Notes:**

* The inclusion of type hints changes nothing about the actual runtime
behavior of a class—that is, no extra checking takes place and nothing
prevents a user from setting bad values in their code.
* In practice, accurate type hinting can be difficult. 
* The type hint does not require the use of the specified type and only provides an optional type for the suggested type.
-----------
# Properties
A property is a special kind of attribute that intercepts attribute access and handles it via
user-defined methods. These methods have complete freedom to manage
the attribute as they see fit.

**Example:**

```python
import string
class Account:
  def __init__(self, owner, balance):
    self.owner = owner
    self._balance = balance
  @property
  def owner(self):
    return self._owner
  @owner.setter
  def owner(self, value):
    if not isinstance(value, str):
      raise TypeError('Expected str')
    if not all(c in string.ascii_uppercase for c in value):
      raise ValueError('Must be uppercase ASCII')
    if len(value) > 10:
      raise ValueError('Must be 10 characters or less')     
    self._ owner = value
```

In this example The ```@property``` decorator is used to establish an attribute as a property.

Here is how it works when you try to use the class:

```python
>>> a = Account('GUIDO', 1000.0)
>>> a.owner = 'EVA'
>>> a.owner = 42
Traceback (most recent call last):
...
TypeError: Expected str
>>> a.owner = 'Carol'
Traceback (most recent call last):
...
ValueError: Must be uppercase ASCII
>>> a.owner = 'RENÉE'
Traceback (most recent call last):
...
ValueError: Must be uppercase ASCII
>>> a.owner = 'RAMAKRISHNAN'
Traceback (most recent call last):
...
ValueError: Must be 10 characters or less
>>>
```

In general, properties allow for the interception of any specific attribute
name. You can implement methods for getting, setting, or deleting the
attribute value.

**Example:**

```python
class SomeClass:
  @property
  def attr(self):
    print('Getting')
  @attr.setter
  def attr(self, value):
    print('Setting', value)
  @attr.deleter
    def attr(self):
      print('Deleting')
# Example
s = SomeClass()
s.attr        # Getting
s.attr = 13   # Setting
del s.attr    # Deleting
```
-----------
# Types, Interfaces, and Abstract Base Classes
When you create an instance of a class, the type of that instance is the class
itself.

* ```isinstance(obj, class)```: To test for membership in a class.
 
**Example:**

```python
class A:
  pass
class B(A):
  pass
class C:
  pass
a = A()     # Instance of 'A'
b = B()     # Instance of 'B'
c = C()     # Instance of 'C'

type(a)               # Returns the class object A
isinstance(a, A)      # Returns True
isinstance(b, A)      # Returns True, B derives from A
isinstance(b, C)      # Returns False, B not derived from C
```

* ```issubclass```: returns True if the class A is a subclass of class B.

A common use of class typing relations is the specification of programming interfaces.
As an example, a top-level base class might be
implemented to specify the requirements of a programming interface. for example:

```pyhton
class Stream:
  def receive(self):
    raise NotImplementedError()
  def send(self, msg):
    raise NotImplementedError()
  def close(self):
    raise NotImplementedError()
# Example.
def send_request(stream, request):
  if not isinstance(stream, Stream):
    raise TypeError('Expected a Stream')
  stream.send(request)
  return stream.receive()
```

It is common for interfaces to be defined as abstract base classes using the abc module. This module defines a base
class (ABC) and a decorator (@abstractmethod) that are used together to
describe an interface. 

**Notes:**

* An abstract class is not meant to be instantiated directly.
* An abstract base class will catch the mistake upon instantiation. This is useful because errors are caught early.
* Although an abstract class cannot be instantiated, it can define methods
and properties for use in subclasses.
