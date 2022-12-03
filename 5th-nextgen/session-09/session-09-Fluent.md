# Shortcut syntax for function definition
Scheme has an alternative define syntax to create a named function without using a nested lambda. 

This is the syntax:

(define (name parm…) body1 body2…)

Adding these two lines to the match takes care of the implementation:

```python
e two lines to the match takes care of the implementation:
 case ['define', [Symbol() as name, *parms], *body] if body:
 env[name] = Procedure(parms, body, env)
 
 ```
# Slicing
A common feature of list, tuple, str, and all sequence types in Python is the sup‐
port of slicing operations, which are more powerful than most people realize.

## Why Slices and Ranges Exclude the Last Item
The Pythonic convention of excluding the last item in slices and ranges works well
with the zero-based indexing used in Python, C, and many other languages. 

Some convenient features of the convention are:

* It’s easy to see the length of a slice or range when only the stop position is given:

range(3) and my_list[:3] both produce three items.

* It’s easy to compute the length of a slice or range when start and stop are given:

just subtract stop - start.

* It’s easy to split a sequence in two parts at any index x, without overlapping:

**e.g:**

```python
>>> l = [10, 20, 30, 40, 50, 60]
>>> l[:2] # split at 2
[10, 20]
>>> l[2:]
[30, 40, 50, 60]
>>> l[:3] # split at 3
[10, 20, 30]
>>> l[3:]
[40, 50, 60]

```

## Slice Objects
```python s[a:b:c]``` can be used to specify a stride or step c, causing the resulting slice to skip items.
The stride can also be negative, returning items in reverse. 

**e.g:**

```python
>>> s = 'bicycle'
>>> s[::3]
'bye'
>>> s[::-1]
'elcycib'
>>> s[::-2]
'eccb'

```

### Notes
* Object slicing is useful because it lets you assign names to slices, just like spreadsheets allow naming of cell ranges.
* The notation a:b:c is only valid within [] when used as the indexing or subscript operator.
