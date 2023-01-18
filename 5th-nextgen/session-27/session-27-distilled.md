# Class Variables and Methods
 the class itself is also an object that can carry state and be manipulated as well. 

```python
class Account:
num_accounts = 0
def __init__(self, owner, balance):
    self.owner = owner
    self.balance = balance
 Account.num_accounts += 1
 def __repr__(self):
    return f'{type(self).__name__}({self.owner!r},
    {self.balance!r})'
 def deposit(self, amount):
    self.balance += amount
 def withdraw(self, amount):
    self.deposit(-amount) # Must use self.deposit()
 def inquiry(self):
    return self.balance
```

Class variables are defined outside the normal __init__() method.
It’s a bit unusual, but class variables can also be accessed via instances.

to define a class method you can write a @classmethod like this:
```python
class Account:
 def __init__(self, owner, balance):
    self.owner = owner
    self.balance = balance
 @classmethod
 def from_xml(cls, data):
 
    from xml.etree.ElementTree import XML
    doc = XML(data)
    return cls(doc.findtext('owner'),
                float(doc.findtext('amount')))
# Example use
data = '''
<account>
 <owner>Guido</owner>
 <amount>1000.0</amount>
</account>
'''
a = Account.from_xml(data)
```

Class variables and class methods are sometimes used together to
configure and control how instances operate.
```python
import time
class Date:
 datefmt = '{year}-{month:02d}-{day:02d}'
 def __init__(self, year, month, day):
    self.year = year
    self.month = month
    self.day = day
 def __str__(self):
    return self.datefmt.format(year=self.year,
 month=self.month,
 day=self.day)
 @classmethod
 def from_timestamp(cls, ts):
    tm = time.localtime(ts)
    return cls(tm.tm_year, tm.tm_mon, tm.tm_mday)
 @classmethod
 def today(cls):
    return cls.from_timestamp(time.time())

This class features a class variable datefmt that adjusts output from the
__str__() method. This is something that can be customized via
inheritance:

```python
class MDYDate(Date):
 datefmt = '{month}/{day}/{year}'
class DMYDate(Date):
 datefmt = '{day}/{month}/{year}'
```

# Static Methods
Sometimes a class is merely used as a namespace for functions declared as
static methods using @staticmethod. 
```python
class Ops:
 @staticmethod
 def add(x, y):
    return x + y
 @staticmethod
 def sub(x, y):
    return x - y
```

Here is an alternate formulation of Account that does that with static methods:
```python
class StandardPolicy:
 @staticmethod
 def deposit(account, amount):
    account.balance += amount
 @staticmethod
 def withdraw(account, amount):
    account.balance -= amount
 @staticmethod
 def inquiry(account):
    return account.balance
class EvilPolicy(StandardPolicy):
 @staticmethod
 def deposit(account, amount):
    account.balance += 0.95*amount
 @staticmethod
 def inquiry(account):
    if random.randint(0,4) == 1:
        return 1.10 * account.balance
    else:
        return account.balance
class Account:
 def __init__(self, owner, balance, *, policy=StandardPolicy):
    self.owner = owner
    self.balance = balance
    self.policy = policy
 def __repr__(self):
    return f'Account({self.policy}, {self.owner!r},
                    {self.balance!r})'
 def deposit(self, amount):
    self.policy.deposit(self, amount)
 def withdraw(self, amount):
    self.policy.withdraw(self, amount)
 def inquiry(self):
    return self.policy.inquiry(self)
 ```

One reason why @staticmethod makes sense here is that there is no need to create instances of StandardPolicy or EvilPolicy.

# Data Encapsulation and Private Attributes
all attributes and methods of a class are public, that is, accessible without any restrictions. This is often undesirable in object-oriented applications that have reasons to hide or encapsulate internal implementation details.

implementation details. To address this problem, Python relies on naming conventions as a means of signaling intended usage. 
```python
class EvilAccount(Account):
 def inquiry(self):
    if random.randint(0,4) == 1:
        return 1.10 * self._balance
    else:
        return self._balance
```
If you come from C++, Java, or another similar object-oriented language, consider _balance similar to a “protected” attribute.

If you wish to have an even more private attribute, prefix the name with
two leading underscores ( __ ).

```python
class A:
 def __init__(self):
    self.__x = 3 # Mangled to self._A__x
 def __spam(self): # Mangled to _A__spam()
    print('A.__spam', self.__x)
 def bar(self):
    self.__spam() # Only calls A.__spam()
class B(A):
 def __init__(self):
    A.__init__(self)
    self.__x = 37 # Mangled to self._B__x
 def __spam(self): # Mangled to _B__spam()
    print('B.__spam', self.__x)
 def grok(self):
    self.__spam() # Calls B.__spam()
```
Although this scheme provides the illusion of data hiding, there’s no mechanism in place to actually prevent access to the “private” attributes of a class. In particular, if the names of the class and the corresponding private attribut

At first glance, name mangling might look like an extra processing step.
However, the mangling process actually only occurs once when the class is
defined.

Be aware that name mangling does
not occur in functions such as getattr(), hasattr(), setattr(), or
delattr() where the attribute name is specified as a string. For these
functions, you would need to explicitly use the mangled name such as
'_Classname__name' to access the attribute.

