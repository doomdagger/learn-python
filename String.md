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
* Byte strings in **3.0**: `b'sp\x01am'`
* Unicode strings in **2.6**: `u'eggs\u0020spam'`

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

#### Escape Sequences in Action

```python
s = 'a\0b\0c'    # len(s) -> 5, 'a\x00b\x00c'

s = '\001\002\003'    # len(s) -> 3, '\x01\x02\x03'
```

## Raw Strings Suppress Escapes

```python
# No Escape Anymore
myfile = open(r'C:\new\text.dat', 'w')
```

> **Caution:**
even a raw string cannot end in a single backslash, **because
the backslash escapes the following quote character**¡ªyou still
must escape the surrounding quote character to embed it in the string.
That is, **r"...\" is not a valid string literal**¡ªa raw string cannot end in
an odd number of backslashes. If you need to end a raw string with a
single backslash, you can use two and slice off the second (`r'1\nb\tc\\'[:-1]`), tack one on manually (r'1\nb\tc' + '\\'), or skip the raw
string syntax and just double up the backslashes in a normal string ('1\
\nb\\tc\\'). All three of these forms create the same eight-character
string containing three backslashes.


## Triple Quotes Code Multiline Block Strings

Triple-quoted strings are also commonly used for **documentation strings**, which are string literals that are taken as comments when they appear at specific points in your file.

## Strings in Action

> **Caution:** Python doesn¡¯t allow you to mix numbers and strings in + expressions: `'abc'+9` raises an error instead of automatically converting 9 to a string.

```python
myjob = 'spam'

for s in myjob: print(s, end=' ')
# result is: s p a m
```

#### Indexing and Slicing

```python
S = 'a string'

# it makes a full top-level copy of a sequence object¡ªan object with the same value, but a distinct piece of memory
S[:]

S[0]

S[-2]

S[1:3]

S[1:]

S[:3]

S[:-1]
```

##### Extended slicing: the third limit and slice objects

slice expressions have support for an optional third index, used
as a step (sometimes called a stride). The step is added to the index of each item extracted. The full-blown form of a slice is now **`X[I:J:K]`**

> **Tips:**
You can also use a negative stride to collect items in the opposite order. For example, the slicing expression `"hello"[::?1]` returns the new string `"olleh"`.

> **Caution:**
With a negative stride, the meanings of the first two bounds are essentially reversed. That is, the slice `S[5:1:?1]` fetches the items from 2 to 5, in reverse order.

> **Notion:**
slicing is equivalent to indexing with a slice object, a finding of importance to class writers seeking to support both operations. 

```python
'spam'[1:3]

'spam'[slice(1, 3)]

'spam'[::-1]

'spam'[slice(None, None, -1)]
```

## String Conversion Tools

```python
int('42'), str(42)

repr(42)    # '42'
repr('42')  # "'42'"
```

#### Character code conversions

**Notion**: 
built-in `ord` function - this returns the actual binary value used to represent the corresponding character in memory. The **`chr`** function performs the inverse operation, taking an integer code and converting it to the corresponding character.
```python
ord('s')    # take only a single character as parameter
# 115
chr(115)
# 's'
```

## Changing Strings
+ Remember the term "immutable sequence"? As we've seen, the immutable part means
that you cannot change a string in place. 
+ Generating a new string object for each string change is not as inefficient as it may sound.

# String Methods

<img src="http://git.candylee.cn/doomdagger/learn-python/raw/master/res/String-3.jpg" width="500">
<img src="http://git.candylee.cn/doomdagger/learn-python/raw/master/res/String-4.jpg" width="550">

> **Performance Recommendation:**
If you have to apply many changes to a very large string, you might be able to
improve your script¡¯s performance by converting the string to an object that does support in-place changes.

```python
# use list
S = 'spammy'

L = list(S)

L[3] = 'x'
L[4] = 'x'

S = ''.join(L)    # S now: 'spaxxy'
```

> **Also Notice:** Be sure to also see the earlier note about the mutable **bytearray** string available as of Python 3.0 and 2.6, described 
fully in Chapter 37; because it may be changed in place, it offers an alternative to this list/join combination for some kinds of 8-bit text that must be changed often.

**none of the string methods accepts patterns¡ªfor pattern-based text processing,
you must use the Python `re` standard library module, an advanced tool. string methods may sometimes run more quickly than the re module¡¯s tools.**


# String Formatting Expressions

...