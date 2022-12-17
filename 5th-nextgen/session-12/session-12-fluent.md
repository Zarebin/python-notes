# dict Comprehensions

A dictcomp (dict comprehension) builds a dict instance by taking key value pairs from any iterable.

Examples of dict comprehensions:
```python
>>> dial_codes = [
  (880, 'Bangladesh'),
  (55, 'Brazil'),
  (86, 'China'),
  (91, 'India'),
  (62, 'Indonesia'),
  (81, 'Japan'),
  (234, 'Nigeria'),
  (92, 'Pakistan'),
  (7, 'Russia'),
  (1, 'United States') ]
>>> country_dial = {country: code for code, country in dial_codes}
>>> country_dial
{'Bangladesh': 880, 'Brazil': 55, 'China': 86, 'India': 91, 'Indonesia': 62,
'Japan': 81, 'Nigeria': 234, 'Pakistan': 92, 'Russia': 7, 'United States': 1}
>>> {code: country.upper()
for country, code in sorted(country_dial.items())
if code < 70}
{55: 'BRAZIL', 62: 'INDONESIA', 7: 'RUSSIA', 1: 'UNITED STATES'}
```
# Unpacking Mappings

PEP 448—Additional Unpacking Generalizations enhanced the support of mapping
unpackings in two ways, since Python 3.5.

* First: Use double star to more than one argument in a function call. This works when
keys are all strings and unique across all arguments.

```python 
>>> def dump(**kwargs):
        return kwargs
>>> dump(**{'x': 1}, y=2, **{'z': 3})
{'x': 1, 'y': 2, 'z': 3}
```
* Second: Double star can be used inside a dict literal—also multiple times:
```python 
>>> {'a': 0, **{'x': 1}, 'y': 2, **{'z': 3, 'x': 4}}
{'a': 0, 'x': 4, 'y': 2, 'z': 3}
```
# Merging Mappings with |

* The | operator creates a new mapping:
```python
>>> d1 = {'a': 1, 'b': 3}
>>> d2 = {'a': 2, 'b': 4, 'c': 6}
>>> d1 | d2
{'a': 2, 'b': 4, 'c': 6}
```
* To update an existing mapping in place, use |=:
```python
>>> d1
{'a': 1, 'b': 3}
>>> d1 |= d2
>>> d1
{'a': 2, 'b': 4, 'c': 6}
```
# Pattern Matching with Mappings

The match/case statement supports subjects that are mapping objects.
### Notes:
* match/case statement is useful for semi-structured data such as JSON.
  * Include a field describing the kind of record.
  * Include a field identifying the schema version
  * Have case clauses to handle invalid records of a specific type
* In contrast with sequence patterns, mapping patterns succeed on partial matches.
* There is no need to use double star extra to match extra key-value pairs, but if you want to
capture them as a dict, you can prefix one variable with **.

# Standard API of Mapping Types

The collections.abc module provides the Mapping and MutableMapping ABCs
describing the interfaces of dict and similar types.
```python 
>>> my_dict = {}
>>> isinstance(my_dict, abc.Mapping)
True
>>> isinstance(my_dict, abc.MutableMapping)
True
```

# What Is Hashable

An object is hashable if it has a hash code which never changes during its lifetime, and can be compared to other objects.

### Notes:

* Hashable objects which compare equal must have the same hash code.
* Numeric types and flat immutable types str and bytes are all hashable.
* The hash code of an object may be different depending on the version of Python, machine architecture and security reasons.
* Container types are hashable if they are immutable and all contained objects are also hashable. example: 
```python 
>>> tt = (1, 2, (30, 40))
>>> hash(tt)
8027212646858338501
>>> tl = (1, 2, [30, 40])
>>> hash(tl)
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'
>>> tf = (1, 2, frozenset([30, 40]))
>>> hash(tf)
-4118419923444501110
```
