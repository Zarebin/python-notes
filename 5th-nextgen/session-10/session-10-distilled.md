## Literals  
A literal is a value typed directly into a program such as **42, 4.2, or 'forty-two'.**
You can use the built-in functions
**bin(x), oct(x), or hex(x)** to convert an integer to a string representing its value in different bases.  
In numeric literals, a single underscore ( _ ) can be used as a visual
separator between digits. *For example:*  
```python
123_456_789
0x1234_5678
0b111_00_101
123.789_012
```

The digit separator is not stored as part of the number—it’s only used to make large numeric literals easier to read in source code.  
### Note:  
String literals are written by enclosing characters with single, double, or triple quotes. Single- and double-quoted strings must appear on the same line. Triple-quoted strings can span multiple lines. *For example:*  
```python
'hello world'
"hello world"
'''hello world'''
"""hello world"""
```

## Expressions and Locations  
An expression represents a computation that evaluates to a concrete value.An expression can always appear on the right-hand side of an
assignment statement, be used as an operand in operations in other
expressions, or be passed as a function argument. *For example:*  
```python
value = 2 + 3 * 5 + sqrt(6+7)
```

### Note:  
The left hand side of an assignment represents a location where a
reference to an object is stored. That location, as shown in the previous example, might be a simple identifier such as value. It could also be an attribute of an object or an index within a container. *For example:*  
```python
a = 4 + 2
b[1] = 4 + 2
c['key'] = 4 + 2
d.value = 4 + 2
```

## Standard Operators  
Python objects can be made to work with any of the operators:  

|Operation | Description|
|----------|------------|
|x + y |Addition|
|x - y |Subtraction|
|x * y |Multiplication|
|x / y |Division|
|x // y |Truncating division|
|x @ y |Matrix multiplication|
|x ** y |Power (x to the y power)|
|x % y |Modulo (x mod y). Remainder.|
|x << y |Left shift|
|x >> y |Right shift|
|x & y |Bitwise and|
|x \| y |Bitwise or|
|x ^ y |Bitwise xor (exclusive or)|
|~x |Bitwise negation|
|–x |Unary minus|
|+x |Unary plus|
|abs(x) |Absolute value|


## In-Place Assignment  
Python objects can be made to work with any of the operators:

|Operation|Description
|--------|-----------
|x += y|x = x + y
|x -= y|x = x - y
|x *= y|x = x * y
|x /= y|x = x / y
|x //= y|x = x // y
|x **= y|x = x ** y
|x %= y|x = x % y
|x @= y|x = x @ y
|x &= y|x = x & y
|x \|= y|x = x \| y
|x ^= y|x = x ^ y
|x >>= y|x = x >> y
|x <<= y|x = x << y


### Note:  
These are not considered to be expressions. Instead, they are a syntactic convenience for updating a value in place.  

## Object Comparison  
The equality operator (x == y) tests the values of x and y for equality. In the
case of lists and tuples, they must be of equal size, have equal elements, and
be in the same order. For dictionaries, True is returned only if x and y have
the same set of keys and all the objects with the same key have equal
values. Two sets are equal if they have the same elements.  


## Ordered Comparison Operators  
The ordered comparison operators have the standard
mathematical interpretation for numbers. **They return a Boolean value.**

|Operation|Description|
|---------|----------|
|x < y|Less than|
|x > y|Greater than|
|x >= y|Greater than or equal to|
|x <= y|Less than or equal to|


### Note:  
When comparing two sequences, the first elements of each sequence are
compared. If they differ, this determines the result. If they’re the same, the
comparison moves to the second element of each sequence. This process
continues until two different elements are found or no more elements exist
in either of the sequences. If the end of both sequences is reached, the
sequences are considered equal. If a is a subsequence of b, then a < b.  

## Boolean Expressions and Truth Values  
The _and, or,_ and _not_ operators can form complex Boolean expressions. The
behavior of these operators is shown in table below:

|Operator|Description|
|--------|----------|
|x or y|If x is false, return y; otherwise, return x.|
|x and y|If x is false, return x; otherwise, return y.|
|not x|If x is false, return True; otherwise, return False.|


### Note:  
a and b evaluates b only if a is true. This is known as short-circuit
evaluation. It can be useful to simplify code involving a test and a
subsequent operation. *For example:*  

```python
if y != 0:
    result = x / y
else:
    result = 0
    # Alternative
    result = y and x / y
```

## Conditional Expressions  
A common programming pattern is assigning a value conditionally based
on the result of an expression. *For example:*  

```python
if a <= b:
    minvalue = a
else:
    minvalue = b
```

**This code can be shortened using a _conditional expression_**. *For example:*  

```python
minvalue = a if a <= b else b
```




















