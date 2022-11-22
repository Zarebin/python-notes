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




#Tuples
Tuples are immutable and they use as record you can pack a collection of values into an
immutable object known as a tuple. you can create tuples like this :
```python
a = ()
b = (item,)
```
# 0-tuple (empty tuple)
# 1-tuple (note the trailing comma)

**Although tuples support most of the same operations as lists**

##Note:
you cannot replace, delete, or append new
elements to an existing tuple. A tuple is best viewed as a single immutable
object that consists of several parts, not as a collection of distinct objects
like a list.

total = sum([shares * price for _, shares, price in portfolio])
When iterating over tuples, the variable _ can be used to indicate a
discarded value. In the above calculation, it means we’re ignoring the first
item (the name).



#Sets
A set is an unordered collection of unique objects.
you can make a set of numbers, strings, or tuples. However, you
can’t make a set containing lists.To create an empty set, use set() with no arguments.

####exp:
```python
set1 =  {'CAT', 'IBM', 'MSFT', 'HPE'}
```
##Note:
t.add('DIS') 
# Add a single item
s.update({'JJ', 'GE', 'ACME'})
# Adds multiple items to s

**An item can be removed using remove() or discard()**



#Dictionaries

A dictionary is a mapping between keys and values. You create a dictionary
by enclosing the key-value pairs, each separated by a colon, in curly braces
({ }), like this:
```python
s = {
'name' : 'GOOG',
'shares' : 100,'price' : 490.10
}
```
***An empty dictionary is created in one of two ways:***
prices = {}
# An empty dict
prices = dict()
# An empty dict

####To access members of a dictionary, use the indexing operator as follows:
name = s['name']

A dictionary is a useful way to define an object that consists of named
fields. However, dictionaries are also commonly used as a mapping for
performing fast lookups on unordered data.

* Dictionary membership is tested with the in operator:

if 'IBM' in prices:
	p = prices['IBM']
else:
	p = 0.0


This particular sequence of steps can also be performed more compactly
using the get() method:

p = prices.get('IBM', 0.0)
# prices['IBM'] if it exists, else 0.0

To obtain a list of dictionary keys, convert a dictionary to a list:
dict1 = {"benz":33, "bmw":45}
keys_list = list(dict1) ==>  #["benz", "bmw"]

Alternatively, you can obtain the keys using dict.keys():
keys_list = dict1.keys()  ==>  #dict_keys(['x', 'y'])

##Notes:
To obtain the values stored in a dictionary, use the dict.values()
method. To obtain key-value pairs, use dict.items().
```python
values_list = dict1.values()

for i, j in dict1.items():
	print(f"key:{i} and value:{j}")
```





