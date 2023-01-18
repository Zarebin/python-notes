# Inheritance

Inheritance is a mechanism for creating a new class that specializes or
modifies the behavior of an existing class.
The new class is called a derived class, child class, subclass, or subtype.
Inheritance is specified with a comma-separated list of base-class names
in the class statement.
f there is no specified base class, a class implicitly
inherits from object and it provides the default implementation of some common methods such as __str__() and __repr__().

```python
class MyAcount(Account):
    def panic(self):
    self.withdraw(self.balance)
# Example
a = MyAcount('Guido', 1000.0)
a.withdraw(23.0) # a.balance = 977.0
a.panic() 
```

Inheritance can also be used to redefine the already existing methods.

```python
import random
class EvilAccount(Account):
    def inquiry(self):
        if random.randint(0,4) == 1:
            return self.balance * 1.10
        else:
            return self.balance
a = EvilAccount('Guido', 1000.0)
a.deposit(10.0) # Calls Account.deposit(a, 10.0)
available = a.inquiry() # Calls EvilAccount.inquiry(a)
```

instances of EvilAccount are identical to instances of Account except for the redefined inquiry() method.
Occasionally, a derived class would reimplement a method but also need
to call the original implementation. A method can explicitly call the
original method using super():

```python
class EvilAccount(Account):
    def inquiry(self):
        if random.randint(0,4) == 1:
            return 1.10 * super().inquiry()
        else:
            return super().inquiry()
```

t’s less common, but inheritance might also be used to add additional
attributes to instances. Here’s how you could make the 1.10 factor in the
above example an instance-level attribute that could be adjusted:
Click here to view code image.
```python
class EvilAccount(Account):
 def __init__(self, owner, balance, factor):
    super().__init__(owner, balance)
    self.factor = factor
 def inquiry(self):
    if random.randint(0,4) == 1:
        return self.factor * super().inquiry()
    else:
        return super().inquiry()
```

A tricky issue with adding attributes is dealing with the existing __init__()  method.
we define a new version of __init__() that includes our additional instance variable factor. However, when __init__() is redefined, it is the responsibility of the child to initialize its parent using super().__init__() as shown.

nheritance can break code in subtle ways. Consider the __repr__() method of the Account class:
```python
class Account:
 def __init__(self, owner, balance):
    self.owner = owner
    self.balance = balance
 def __repr__(self):
    return f'Account({self.owner!r}, {self.balance!r})'
```
The purpose of this method is to assist in debugging by making nice output.

the method is hardcoded to use the name Account. If you start using inheritance, you’ll find that the output is wrong

```python
class Account:
    ...
    def __repr__(self):
        return f'{type(self).__name__}({self.owner!r},{self.balance!r})'
```

Sometimes, the “is a” inheritance relationship is used to define object type ontologies or taxonomies. For example:

```python
class Food:
 pass
class Sandwich(Food):
 pass
class RoastBeef(Sandwich):
 pass
class GrilledCheese(Sandwich):
 pass
class Taco(Food):
 pass
```

you can inherit multiple classes at same time:
```python
class HotDog(Sandwich, Taco):
 pass
```

### Avoiding Inheritance via Composition
suppose you wanted to make a stack data structure
with push and pop operations. A quick way to do that would be to inherit
from a list and add a new method to it:
```python
class Stack(list):
    def push(self, item):
    self.append(item)
# Example
s = Stack()
s.push(1)
s.push(2)
s.push(3)
s.pop() # -> 3
s.pop() # -> 2
```

A better approach is composition.you should build a stack as an independent class that happens to have a list contained in it.

```python
class Stack:
 def __init__(self):
    self._items = list()
 def push(self, item):
    self._items.append(item)
 def pop(self):
    return self._items.pop()
 def __len__(self):
    return len(self._items)
# Example use
s = Stack()
s.push(1)
s.push(2)
s.push(3)
s.pop() # -> 3
s.pop() # -> 2
```
A slight extension of this implementation might accept the internal list
class as an optional argument:
```python
class Stack:
 def __init__(self, *, container=None):
    if container is None:
    container = list()
    self._items = container
 def push(self, item):
    self._items.append(item)
 def pop(self):
    return self._items.pop()
 def __len__(self):
    return len(self._items)
```
This is also an example of what’s known as dependency injection.

making the internal list a hidden implementation

## voiding Inheritance via Functions


```python
class DataParser:
 def parse(self, lines):
   records = []
   for line in lines:
   row = line.split(',')
   record = self.make_record(row)
   records.append(row)
   return records
 def make_record(self, row):
   raise NotImplementedError()
class PortfolioDataParser(DataParser):
 def make_record(self, row):
   return {
     'name': row[0],
     'shares': int(row[1]),
     'price': float(row[2])
   }
```
There is too much plumbing going on here. If you’re writing a lot of
single-method classes, consider using functions instead. For example:
```python
def parse_data(lines, make_record):
 records = []
 for line in lines:
 row = line.split(',')
 record = make_record(row)
 records.append(row)
 return records
def make_dict(row):
 return {
 'name': row[0],
 'shares': int(row[1]),
 'price': float(row[2])
 }
```
##Dynamic Binding and Duck Typing
* Dynamic binding is the runtime mechanism that Python uses to find the attributes of objects.
* If you make a lookup, such as obj.name, it will work
on any obj whatsoever that happens to have a name attribute. This behavior
is sometimes referred to as duck typing—in reference to the adage “if it
looks like a duck, quacks like a duck, and walks like a duck, then it’s a
duck.

# The Danger of Inheriting from Builtin Types
Python allows inheritance from built-in types. 
```python
class udict(dict):
 def __setitem__(self, key, value):
    super().__setitem__(key.upper(), value)
```
 As an example, if you decide to subclass dict in order to force all
keys to be uppercase, you might redefine the __setitem__() method like
this:

```python
class udict(dict):
 def __setitem__(self, key, value):
 super().__setitem__(key.upper(), value)
```

however, reveals that it only seems to work. In fact, it
doesn’t really seem to work at all:
```python
>>> u = udict(name='Guido', number=37)
>>> u
{ 'name': 'Guido', 'number': 37 }
>>> u.update(color='blue')
>>> u
{ 'name': 'Guido', 'number': 37, 'color': 'blue' }
```

```python
from collections import UserDict
class udict(UserDict):
 def __setitem__(self, key, value):
    super().__setitem__(key.upper(), value)
```

```python
>>> u = udict(name='Guido', num=37)
>>> u.update(color='Blue')
>>> u
{'NAME': 'Guido', 'NUM': 37, 'COLOR': 'Blue'}
>>> v = udict(u)
>>> v['title'] = 'BDFL'
>>> v
{'NAME': 'Guido', 'NUM': 37, 'COLOR': 'Blue', 'TITLE': 'BDFL'}
>>>
```



