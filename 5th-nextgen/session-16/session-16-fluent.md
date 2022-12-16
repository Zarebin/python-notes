# Character Issues 
The concept of **string** is simple enough: a string is a sequence of characters. The
problem lies in the definition of **character**.

best definition of **character** is a Unicode character. 

Unicode standards (explicitly separates the identity of characters from specific byte representations):

* he identity of a character—its code point—is a number from 0 to 1,114,111 (base 10), shown in the Unicode standard as 4 to 6 hex digits
with a “U+” prefix, from U+0000 to U+10FFFF.
* The actual bytes that represent a character depend on the encoding in use. An
encoding is an algorithm that converts code points to byte sequences and vice versa.

# Byte Essentials
The new binary sequence types are unlike the Python 2 str in many regards. Each item in bytes or bytearray is an integer
from 0 to 255, and not a one-character string like in the Python 2 str. 

**Example**
```python
>>> cafe = bytes('café', encoding='utf_8')
>>> cafe
b'caf\xc3\xa9'
>>> cafe[0]
99
>>> cafe[:1]
b'c'
>>> cafe_arr = bytearray(cafe)
>>> cafe_arr
bytearray(b'caf\xc3\xa9')
>>> cafe_arr[-1:]
bytearray(b'\xa9')

```
### Notes
* Binary sequences are really sequences of integers but the fact that ASCII text is often embedded in them.Therefore, four different
displays are used, depending on each byte value:
  * For bytes with decimal codes 32 to 126—from space to ~ (tilde)—the ASCII char‐
    acter itself is used.
  * For bytes corresponding to tab, newline, carriage return, and \, the escape
    sequences \t, \n, \r, and \\ are used.
  * If both string delimiters ' and " appear in the byte sequence, the whole sequence
    is delimited by ', and any ' inside are escaped as \'.
  * For other byte values, a hexadecimal escape sequence is used (e.g., \x00 is the
    null byte).
* The other ways of building bytes or bytearray instances are calling their construc‐
tors with:
  * A str and an encoding keyword argument
  * An iterable providing items with values from 0 to 255
  * An object that implements the buffer protocol

# Basic Encoders/Decoders
The Python distribution bundles more than 100 codecs (encoder/decoders) for text to
byte conversion and vice versa.

**Example**
```python
>>> for codec in ['latin_1', 'utf_8', 'utf_16']:
...
print(codec, 'El Niño'.encode(codec), sep='\t')
...
latin_1 b'El Ni\xf1o'
utf_8
b'El Ni\xc3\xb1o'
utf_16 b'\xff\xfeE\x00l\x00 \x00N\x00i\x00\xf1\x00o\x00'
```

# Understanding Encode/Decode Problems
We have two exception for understanding encode and decode problems: 
* UnicodeEncodeError (when converting str to binary sequences) 
* UnicodeDecodeError (when reading binary sequences into str)

## Coping with UnicodeEncodeError
Most non-UTF codecs handle only a small subset of the Unicode characters. Unico
deEncodeError will be raised, unless special handling is provided by passing an
errors argument to the encoding method or function.

**Example**
```python
>>> city = 'São Paulo'
>>> city.encode('utf_8')
b'S\xc3\xa3o Paulo'
>>> city.encode('utf_16')
b'\xff\xfeS\x00\xe3\x00o\x00 \x00P\x00a\x00u\x00l\x00o\x00'
>>> city.encode('iso8859_1')
b'S\xe3o Paulo'
>>> city.encode('cp437')
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
File "/.../lib/python3.4/encodings/cp437.py", line 12, in encode
return codecs.charmap_encode(input,errors,encoding_map)
UnicodeEncodeError: 'charmap' codec can't encode character '\xe3' in
position 1: character maps to <undefined>
>>> city.encode('cp437', errors='ignore')
b'So Paulo'
>>> city.encode('cp437', errors='replace')
b'S?o Paulo'
>>> city.encode('cp437', errors='xmlcharrefreplace')
b'S&#227;o Paulo'
```

## Coping with UnicodeDecodeError
Not every byte holds a valid ASCII character, and not every byte sequence is valid
UTF-8 or UTF-16. When you assume one of these encodings while convert‐
ing a binary sequence to text, you will get a UnicodeDecodeError if unexpected bytes
are found.

```python
>>> octets = b'Montr\xe9al'
>>> octets.decode('cp1252')
'Montréal'
>>> octets.decode('iso8859_7')
'Montrιal'
>>> octets.decode('koi8_r')
'MontrИal'
>>> octets.decode('utf_8')
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xe9 in position 5:
invalid continuation byte
>>> octets.decode('utf_8', errors='replace')
```
