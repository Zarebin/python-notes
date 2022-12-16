# Set Comprehensions  

Set comprehensions (setcomps) were added way back in Python 2.7, together with the
dictcomps that we saw in “dict Comprehensions” on page 79. Example 3-15 shows
how.  

Example 3-15. Build a set of Latin-1 characters that have the word “SIGN” in their Unicode names.

```python
>>> from unicodedata import name
>>> {chr(i) for i in range(32, 256) if 'SIGN' in name(chr(i),'')}
{'§', '=', '¢', '#', '¤', '<', '¥', 'µ', '×', '$', '¶', '£', '©',
'°', '+', '÷', '±', '>', '¬', '®', '%'}
```

* Import name function from unicodedata to obtain character names.
* Build set of characters with codes from 32 to 255 that have the word 'SIGN' in
their names.

## Practical Consequences of How Sets Work  

The set and frozenset types are both implemented with a hash table. This has these effects:  

* Set elements must be hashable objects. They must implement proper __hash__
and __eq__ methods.
* Membership testing is very efficient. A set may have millions of elements, but an element can be located directly by computing its hash code and deriving an indexoffset, with the possible overhead of a small number of tries to find a matching element or exhaust the search.
* Sets have a significant memory overhead, compared to a low-level array pointers
to its elements—which would be more compact but also much slower to search
beyond a handful of elements.
* Element ordering depends on insertion order, but not in a useful or reliable way.
If two elements are different but have the same hash code, their position depends
on which element is added first.
* Adding elements to a set may change the order of existing elements. That’s
because the algorithm becomes less efficient if the hash table is more than two-
thirds full, so Python may need to move and resize the table as it grows. When
this happens, elements are reinserted and their relative ordering may change.


## Set Operations

The infix operators require that both operands be sets,
but all other methods take one or more iterable arguments. *For
example*, to produce the union of four collections, a, b, c, and d,
you can call *a.union(b, c, d)*, where a must be a set, but b, c,
and d can be iterables of any type that produce hashable items. If
you need to create a new set with the union of four iterables,
instead of updating an existing set, you can write {*a, *b, *c,
*d} since Python 3.5 thanks to PEP 448—Additional Unpacking
Generalizations.  


## Set Operations on dict Views  

the view objects returned by the dict methods *.keys()*
and *.items()* are remarkably similar to frozenset.  
In particular, dict_keys and dict_items implement the special methods to support
the powerful set operators & (intersection), | (union), - (difference), and ^ (symmet‐ric difference).  

*For example*, using & is easy to get the keys that appear in two dictionaries:  

```python
>>> d1 = dict(a=1, b=2, c=3, d=4)
>>> d2 = dict(b=20, d=40, e=50)
>>> d1.keys() & d2.keys()
{'b', 'd'}
```


### Note:  
the return value of & is a set. Even better: the set operators in dictionary
views are compatible with set instances. Check this out:

```python
>>> s = {'a', 'e', 'i'}
>>> d1.keys() & s
{'a'}
>>> d1.keys() | s
{'a', 'c', 'b', 'd', 'i', 'e'}
```

A dict_items view only works as a set if all values in the dict are
hashable. Attempting set operations on a dict_items view with an
unhashable value raises TypeError: unhashable type 'T', with T
as the type of the offending value.
On the other hand, a dict_keys view can always be used as a set,
because every key is hashable—by definition.
