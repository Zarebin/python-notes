# Sorting Unicode Text
Python sorts sequences of any type by comparing the items in each sequence one by
one.

Sorting rules vary for different locales, for example:

```python 
>>> fruits = ['caju', 'atemoia', 'cajá', 'açaí', 'acerola']
>>> sorted(fruits)
['acerola', 'atemoia', 'açaí', 'caju', 'cajá']
```
But the sorted fruits list should be like this:
```python
['açaí', 'acerola', 'atemoia', 'cajá', 'caju']
```
To avoid this problem you can use **local.strxfrm** function. for example:

```python
import locale
my_locale = locale.setlocale(locale.LC_COLLATE, 'pt_BR.UTF-8')
print(my_locale)
fruits = ['caju', 'atemoia', 'cajá', 'açaí', 'acerola']
sorted_fruits = sorted(fruits, key=locale.strxfrm)
print(sorted_fruits)
['açaí', 'acerola', 'atemoia', 'cajá', 'caju']
```

* Be aware of these caveats:
  * Because locale settings are global, calling setlocale in a library is not recom‐
    mended.#
  * The locale must be installed on the OS, to avoid this exception: _locale.Error: unsupported locale setting_
  * You must know how to spell the locale name.
  * The locale must be correctly implemented by the makers of the OS.
_____
# Sorting with the Unicode Collation Algorithm
James Tauber,created pyuca,
a pure-Python implementation of the Unicode Collation Algorithm (UCA). a simpler solution for above problem.

**Example:**
```python
>>> import pyuca
>>> coll = pyuca.Collator()
>>> fruits = ['caju', 'atemoia', 'cajá', 'açaí', 'acerola']
>>> sorted_fruits = sorted(fruits, key=coll.sort_key)
>>> sorted_fruits
['açaí', 'acerola', 'atemoia', 'cajá', 'caju']
```
note that, pyuca has one sorting algorithm that does not respect the sorting
order in individual languages.
_____
# The Unicode Database
The Unicode standard provides an entire database—in the form of several structured
text files—that includes not only the table mapping code points to character names,
but also metadata about the individual characters and how they are related.
_____
# Finding Characters by Name
The unicodedata module has functions to retrieve character metadata, including uni
**codedata.name()**, which returns a character’s official name in the standard.
**Example:**

```python
>>> from unicodedata import name
>>> name('A')
'LATIN CAPITAL LETTER A'
```
_____
# Numeric Meaning of Characters
The unicodedata module includes functions to check whether a Unicode character
represents a number. 

**Example:**
```python
import unicodedata
import re
re_digit = re.compile(r'\d')
sample = '1\xbc\xb2\u0969\u136b\u216b\u2466\u2480\u3285'
for char in sample:
print(f'U+{ord(char):04x}',
char.center(6),
're_dig' if re_digit.match(char) else '-',
'isdig' if char.isdigit() else '-',
'isnum' if char.isnumeric() else '-',
f'{unicodedata.numeric(char):5.2f}',
unicodedata.name(char),
sep='\t')
```
_____
# Dual-Mode str and bytes APIs
Python’s standard library has functions that accept str or bytes arguments and
behave differently depending on the type.

## str Versus bytes in Regular Expressions
If you build a regular expression with bytes, patterns such as \d and \w only match
ASCII characters; in contrast, if these patterns are given as str, they match Unicode
digits or letters beyond ASCII. 

**Example:**
```python
import re
re_numbers_str = re.compile(r'\d+')
re_words_str = re.compile(r'\w+')
re_numbers_bytes = re.compile(rb'\d+')
re_words_bytes = re.compile(rb'\w+')
text_str = ("Ramanujan saw \u0be7\u0bed\u0be8\u0bef"
" as 1729 = 1³ + 12³ = 9³ + 10³.")
text_bytes = text_str.encode('utf_8')
print(f'Text\n {text_str!r}')
print('Numbers')
print(' str :', re_numbers_str.findall(text_str))
```

## str Versus bytes in os Functions
The GNU/Linux kernel is not Unicode savvy, so in the real world you may find file‐
names made of byte sequences that are not valid in any sensible encoding scheme,
and cannot be decoded to str.

**Example:**
>>> os.listdir('.')
['abc.txt', 'digits-of-π.txt']
>>> os.listdir(b'.')
[b'abc.txt', b'digits-of-\xcf\x80.txt']
```
