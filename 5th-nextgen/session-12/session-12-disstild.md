## List, Set, and Dictionary Comprehensions

here we take all
the items in a list, apply an operation, and create a new list:

```python
nums = [1, 2, 3, 4, 5]
squares = []
for n in nums:
    squares.append(n * n)
```

Here is a more compact version of this
code:

```python
nums = [1, 2, 3, 4, 5]
squares = [n * n for n in nums]
```

It is also possible to apply a filter to the operation:
```python
squares = [n * n for n in nums if n > 2] # [9, 16, 25]
```

The general syntax for a list comprehension is as follows:

```python
[expression for item1 in iterable1 if condition1
            for item2 in iterable2 if condition2
            ...
            for itemN in iterableN if conditionN ]
```
---
---
```python
# Some data (a list of dictionaries)
portfolio = [
 {'name': 'IBM', 'shares': 100, 'price': 91.1 },
 {'name': 'MSFT', 'shares': 50, 'price': 45.67 },
 {'name': 'HPE', 'shares': 75, 'price': 34.51 },
 {'name': 'CAT', 'shares': 60, 'price': 67.89 },
 {'name': 'IBM', 'shares': 200, 'price': 95.25 }
]
# Collect all names ['IBM', 'MSFT', 'HPE', 'CAT', 'IBM' ]
names = [s['name'] for s in portfolio]
# Find all entries with more than 100 shares ['IBM']
more100 = [s['name'] for s in portfolio if s['shares'] > 100 ]
# Find the total shares*price
cost = sum([s['shares']*s['price'] for s in portfolio])
# Collect (name, shares) tuples
name_shares = [ (s['name'], s['shares']) for s in portfolio ]
```

All of the variables used inside a list comprehension are private to the
comprehension.

### for example:
```python
>>> x = 42
>>> squares = [x*x for x in [1,2,3]]
>>> squares
[1, 4, 9]
>>> x
42
>>>
```

## creating a set comprehension by changing the brackets into curly braces

### for example:
```python
names = { s['name'] for s in portfolio }
```

## If you specify key:value pairs, you’ll create a dictionary instead. This is known as a dictionary comprehension. 

## For example:

```python
prices = { s['name']:s['price'] for s in portfolio }
# prices = { 'IBM': 95.25, 'MSFT': 45.67, 'HPE': 34.51, 'CAT':67.89 }

```


## Generator Expressions

The syntax is the
same as for a list comprehension except that you use parentheses instead of
square brackets.

## example:
```python
nums = [1,2,3,4]
squares = (x*x for x in nums)
```

Unlike a list comprehension, a generator expression does not actually
create a list .
Instead, it creates a generator object that produces the values on demand via
iteration.

```python
>>> squares
<generator object at 0x590a8>
>>> next(squares)
1
>>> next(squares)
4
...
>>> for n in squares:
... print(n)
9
16
>>>

```



## difference between list comprehensions and generator expressions

* list comprehension, Python actually creates a
list that contains the resulting data.
* generator expression, Python
creates a generator that merely knows how to produce data on demand


---


When passed as a single function argument, one set of parentheses can
be removed.

## For example:
```python
sum((x*x for x in values))
sum(x*x for x in values) # Extra parens removed

```


# The Attribute (.) Operator

The dot (.) operator is used to access the attributes of an object. Here’s an
example:

---

#  Order of Evaluation
lists the order of operation (precedence rules) for Python
operators. All operators except the power (**) operator are evaluated from
left to right



| Operator                 | Name                               |
| -------------------------|:----------------------------------:| 
| (...), [...], {...}      | Tuple, list, and dictionarycreation|
|  s[i], s[i:j]            |        indexing and slicing                             |  
|  s.attr                        |         Attribute lookup                          |    
|   f(...)                       |           Function calls                          |    
|     +x, -x, ~x                 |         Unary operators                           |    
|      x ** y                    |             Power (right associative)             |    
|    x + y, x - y                |       Addition, subtraction                       |    
|    x << y, x >> y              |      Bit-shifting                                 |    
|   x ^ y                        |                Bitwise exclusive or               |    
|    not x                       |             Logical negation                      |    
|       x and y                  |         Logical and                               |    
|      x or y                    |           Logical or                              |    
|      lambda args: expr         |         Anonymoss function                        |    
|    expr if expr else expr      |            Conditional expression                 |    
|  name := expr                  |             Assignment expression                 |    
  





