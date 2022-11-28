# Nested Unpacking
Target of an unpacking can use nesting, e.g., (a, b, (c, d))  

example:
```python
metro_areas = [
('Tokyo', 'JP', 36.933, (35.689722, 139.691667)),
('Delhi NCR', 'IN', 21.935, (28.613889, 77.208889)),
('Mexico City', 'MX', 20.142, (19.433333, -99.133333)),
('New York-Newark', 'US', 20.104, (40.808611, -74.020386)),
('São Paulo', 'BR', 19.649, (-23.547778, -46.635833)),
]

def main():
	print(f'{"":15} | {"latitude":>9} | {"longitude":>9}')
	for name, _, _, (lat, lon) in metro_areas:
		if lon <= 0:
			print(f'{name:15} | {lat:9.4f} | {lon:9.4f}')
```

**Notes:**
* Each tuple holds a record with four fields, the last of which is a coordinate pair.
* By assigning the last field to a nested tuple, we unpack the coordinates.

```python
name, _, _, (lat, lon)
```

The target of an unpacking assignment can also be a list (but it is rare). E.g if you have a database query that returns a single record.
```python
[record] = query_returning_single_row()
```
**Note**
Single-item tuples must be written with a trailing comma.
```python
(record,) = query_returning_single_row()
```

# Pattern Matching with Sequences
The most visible new feature in Python 3.10 is pattern matching with the match/case.  
example:
a robot that accepts commands sent as sequences of words and numbers, like BEEPER 440 3

```python
def handle_command(self, message):
	match message:
		case ['BEEPER', frequency, times]:
			self.beep(times, frequency)
		case ['NECK', angle]:
			self.rotate_neck(angle)
		case ['LED', ident, intensity]:
			self.leds[ident].set_brightness(ident, intensity)
		case ['LED', ident, red, green, blue]:
			self.leds[ident].set_color(ident, red, green, blue)
		case _:
			raise InvalidCommand(message)
```

**Notes:**
* The expression after the match keyword is the subject.
* First case matches any subject that is a sequence with three items. The first item must be the string 'BEEPER'.
* The last case is the default case. It will match any subject that did not match a previous pattern.

match/case may look like the switch/case statement. One key improvement of match over switch is destructuring—a more advanced form of unpacking.

```python
def main():
	print(f'{"":15} | {"latitude":>9} | {"longitude":>9}')
	for record in metro_areas:
		match record:
			case [name, _, _, (lat, lon)] if lon <= 0:
				print(f'{name:15} | {lat:9.4f} | {lon:9.4f}')
```

**Notes:**
* The subject of this match is record.
* A case clause has two parts: a pattern and an optional guard with the if keyword.

In general, a sequence pattern matches the subject if:
1. The subject is a sequence and;
2. The subject and the pattern have the same number of items and;
3. Each corresponding item matches, including nested items.

A sequence pattern can match instances of most actual or virtual subclasses of collections.abc.Sequence, with the exception of str, bytes, and bytearray.  
In the standard library, these types are compatible with sequence patterns:
* list
* memoryview
* array.array
* tuple
* range
* collections.deque

**Note**
As said before, the integer 987 is treated as one value, not a sequence of digits. If you want to treat an object of those types as a sequence subject, convert it in the match clause. For example:

```python
match tuple(phone):
	case ['1', *rest]: # North America and Caribbean
		...
	case ['2', *rest]: # Africa and some territories
		...
	case ['3' | '4', *rest]: # Europe
		...
```

You can bind any part of a pattern with a variable using the 'as' keyword.
```python
case [name, _, _, (lat, lon) as coord]:
```

We can make patterns more specific by adding type information. For example:
```python
case [str(name), _, _, (float(lat), float(lon))]:
```
This syntax performs a runtime type check.  

If we want to match any subject sequence starting with a str, and ending with a nested sequence of two floats:
```python
case [str(name), *_, (float(lat), float(lon))]:
```
The *_ matches any number of items, without binding them to a variable. Using *extra instead of *_ would bind the items to extra as a list with 0 or more items.
