# Tuples
Tuples are immutable and they use as record you can pack a collection of values into an
immutable object known as a tuple. you can create tuples like this :

```python
a = ()
b = (item,)
```
### 0-tuple (empty tuple)
### 1-tuple (note the trailing comma)

** Although tuples support most of the same operations as lists **

### Note:
you cannot replace, delete, or append new
elements to an existing tuple. A tuple is best viewed as a single immutable
object that consists of several parts, not as a collection of distinct objects
like a list.

total = sum([shares * price for _, shares, price in portfolio])
When iterating over tuples, the variable _ can be used to indicate a
discarded value. In the above calculation, it means we’re ignoring the first
item (the name).



# Sets
A set is an unordered collection of unique objects.
you can make a set of numbers, strings, or tuples. However, you
can’t make a set containing lists.To create an empty set, use set() with no arguments.

#### exp:
```python
set1 =  {'CAT', 'IBM', 'MSFT', 'HPE'}
```
### Note:
t.add('DIS') 
#### Add a single item
s.update({'JJ', 'GE', 'ACME'})
#### Adds multiple items to s

** An item can be removed using remove() or discard() **



# Dictionaries

A dictionary is a mapping between keys and values. You create a dictionary
by enclosing the key-value pairs, each separated by a colon, in curly braces
({ }), like this:

```python
s = {
'name' : 'GOOG',
'shares' : 100,
'price' : 490.10
}
```


*** An empty dictionary is created in one of two ways: ***
prices = {}
#### An empty dict
prices = dict()
#### An empty dict

#### To access members of a dictionary, use the indexing operator as follows:
name = s['name']

A dictionary is a useful way to define an object that consists of named
fields. However, dictionaries are also commonly used as a mapping for
performing fast lookups on unordered data.

* Dictionary membership is tested with the in operator:

```python
if 'IBM' in prices:
	p = prices['IBM']
else:
	p = 0.0
```

This particular sequence of steps can also be performed more compactly
using the get() method:

p = prices.get('IBM', 0.0)

#### prices['IBM'] if it exists, else 0.0

To obtain a list of dictionary keys, convert a dictionary to a list:
```python
dict1 = {"benz":33, "bmw":45}
keys_list = list(dict1)  
>>>["benz", "bmw"]

Alternatively, you can obtain the keys using dict.keys():
keys_list = dict1.keys()  
>>>dict_keys(['x', 'y'])
```

### Notes:
To obtain the values stored in a dictionary, use the dict.values()
method. To obtain key-value pairs, use dict.items().

```python
values_list = dict1.values()

for i, j in dict1.items():
	print(f"key:{i} and value:{j}")
```

