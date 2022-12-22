# What is UTF-8 and Unicode?
To understand Unicode and UTF-8 easily, we want to go step by step and illustrate this topic.

### IASCII_1

To have a character in the computer, we want to create a system. In the first step, we implement the following system:
‍‍
    01001000 10001110 01000000 0111101 (Each 8 bits is called a byte) 
    2*2*2...*2 ?= 256! (Each bit has two states)

    00000000 char0
    00000001 char1
    00000010 char2
    ...
    01111111 char127 

We call this system IASCII_1.

But this system has a problem. With 128 characters, we cannot cover many other characters.
(assuming it only covers Persian characters and does not cover English characters)
To improve IASCII_1, we are going to create a new version of IASCII_1.

### IASCII_2

Now we want to create a system that covers more characters (such as English, French characters, emojis,...)
One idea to do this is to increase the number of bits in the system.
So we increase the number of bits from 8 to 16:

    ‍‍‍00000000.00000000 char0
    00000000.00000001 char1
    00000000.00000010 char2
    ...
    00000000.01111111 char2^15
    
We call this system IASCII_2.

**Is IASCII_2 suitable?**

*‌Although this new system covers more characters, But it still doesn't cover the number of characters we expect!
*‌In addition, there is another concern. In the previous system, we used 1 byte for each character,
  but in the new system, we use 2 bytes for each character.
  

How to solve the problem of IASCII_2? We need to create a new system.

Let's jump to create it.


### Unirahimicode

The third idea is to specify how many characters are needed. 
(for example, specify that we need one byte from 0 to 127, but for more than 127 characters we need 2 bytes).

    00 000000 chara
    01 000000 00000001 charb
    10 000000 00000010 00000000 charc
    11 000000.00000010 00000000 00000000 chard

We call this system Unirahimicode.

In this new system, by reading the first byte, we find out how many bytes each character is.
_____

# Unicode and UTF-8
With the above introduction, we will now introduce Unicode and utf-8 with an example:

    A Chinese character:      汉
    its Unicode value:        U+6C49
    convert 6C49 to binary:   01101100 01001001

Now, let's say we decide to store this character on our hard drive.
To do that, we need to store the character in binary format. We can simply store it as is '01101100 01001001'. Done!

But wait a minute, is '01101100 01001001' one character or two characters?
You knew this is one character because I told you, but when a computer reads it,
it has no idea. So we need some sort of encoding to tell the computer to treat it as one.

This is where the rules of UTF-8 come in:

    Binary format of bytes in sequence
    1st Byte    2nd Byte    3rd Byte    4th Byte    Number of Free Bits   Maximum Expressible Unicode Value
    0xxxxxxx                                                7             007F hex (127)
    110xxxxx    10xxxxxx                                (5+6)=11          07FF hex (2047)
    1110xxxx    10xxxxxx    10xxxxxx                  (4+6+6)=16          FFFF hex (65535)
    11110xxx    10xxxxxx    10xxxxxx    10xxxxxx    (3+6+6+6)=21          10FFFF hex (1,114,111)

According to the table above, if we want to store this character using the UTF-8 format, we need to prefix our character with some 'headers'.
Our Chinese character is 16 bits long (count the binary value yourself),
so we will use the format on row 3 as it provides enough space:

    Header  Place holder    Fill in our Binary   Result         
    1110    xxxx            0110                 11100110
    10      xxxxxx          110001               10110001
    10      xxxxxx          001001               10001001 

Writing out the result in one line:

    11100110 10110001 10001001  

This is the UTF-8 binary value of the Chinese character! 

**Summary**

    A Chinese character:      汉
    its Unicode value:        U+6C49
    convert 6C49 to binary:   01101100 01001001
    encode 6C49 as UTF-8:     11100110 10110001 10001001

convert binary to Unicode value:

    0110 1100 0100 1001
    6    c    4    9   
_____

# Binary, decimal and hexadecimal numbers
Now let's introduce binary decimal and hexadecimal numbers.

### Decimal
    0

    1

    2

    3

    ...

    9

    1 0

    1 1

    1 2

    ...

    2 0

    2 1

    ...

### Binary
    0

    1

    1 0

    1 1

    1 0 0

    ...

### Hexadecimal
    0

    1

    2

    ...

    9

    A

    B

    ...

    F

    1 0

    1 1

    1 2
 
    1 3

    ...

    1 F

    2 0

    2 1

    ...

    2 F

    ...
