Strings
=======

# Overview

* In Python 3.X there are three string types: **str** is used for Unicode text (including ASCII), **bytes** is used for binary data (including encoded text), and **bytearray** is a mutable variant of bytes. Files work in two modes: text, which represents content
as **str** and implements Unicode encodings, and **binary**, which deals in raw **bytes** and does no data translation.
* In Python 2.X, **unicode** strings represent Unicode text, **str** strings handle both 8-bit text and binary data, and **bytearray** is available in 2.6 and later as a back-port from 3.X. Normal files¡¯ content is simply bytes represented as **str**, but a **codecs** module opens Unicode text files, handles encodings, and represents content as **unicode** objects.

> Python has no distinct type for individual characters;
instead, you just use one-character strings


# Common String literals and operations

![pic](http://git.candylee.cn/doomdagger/learn-python/raw/master/res/String-1.jpg "")

# String Literals

* Single quotes: `'spa"m'`
* Double quotes: `"spa'm"`
* Triple quotes (**can be used as comments**): `'''...spam...''', """...spam..."""`
* Escape sequences (**normal string literal**): `"s\tp\na\0m"`
* Raw strings (**do not escape**): `r"C:\new\test.spm"`
* Byte strings in **3.0**: b'sp\x01am'
* Unicode strings in **2.6**: u'eggs\u0020spam'

## Single- and Double-Quoted Strings Are the Same

Without comma, Python automatically concatenates adjacent string literals in any expression, although it is almost as simple to add a + operator between them to invoke concatenation explicitly.
```python

title = "Meaning " 'of' " Life"

```

## Escape Sequences Represent Special Bytes

![pic](http://git.candylee.cn/doomdagger/learn-python/raw/master/res/String-2.jpg "")

```python
'\xhh'        # Character with hex value hh (exactly 2 digits)
'\ooo'        # Character with octal value ooo (up to 3 digits)
'\0'          # Null: binary 0 character (doesn't end string)

'\uhhhh'      # Unicode character with 16-bit hex value
'\Uhhhhhhhh'  # Unicode character with 32-bit hex value           
```

> **Tips:**
The `\Uhhhh...` escape sequence takes exactly eight hexadecimal digits (h); both \u and \U are recognized only in **Unicode string literals(`u'\Uhhhh...'`)**
in 2.X, but can be used in **normal strings** (which are Unicode) in 3.X. In a 3.X **bytes literal**, hexadecimal and octal escapes denote the byte
with the given value; in a string literal, these escapes denote a Unicode character with the given code-point value.
