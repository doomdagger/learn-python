Strings
=======

# Overview

* In Python 3.X there are three string types: **str** is used for Unicode text (including ASCII), **bytes** is used for binary data (including encoded text), and **bytearray** is a mutable variant of bytes. Files work in two modes: text, which represents content
as **str** and implements Unicode encodings, and **binary**, which deals in raw **bytes** and does no data translation.
* In Python 2.X, **unicode** strings represent Unicode text, **str** strings handle both 8-bit text and binary data, and **bytearray** is available in 2.6 and later as a back-port from 3.X. Normal files¡¯ content is simply bytes represented as **str**, but a **codecs** module opens Unicode text files, handles encodings, and represents content as **unicode** objects.

> Python has no distinct type for individual characters;
instead, you just use one-character strings


# Common String literals and operations

![pic](https://raw.githubusercontent.com/doomdagger/learn-python/master/res/String-1.jpg "")

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

![pic](https://raw.githubusercontent.com/doomdagger/learn-python/master/res/String-2.jpg "")

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
the backslash escapes the following quote character**, you still
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
You can also use a negative stride to collect items in the opposite order. For example, the slicing expression `"hello"[::-1]` returns the new string `"olleh"`.

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

![pic](https://raw.githubusercontent.com/doomdagger/learn-python/master/res/String-3.jpg "")
![pic](https://raw.githubusercontent.com/doomdagger/learn-python/master/res/String-4.jpg "")

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

**none of the string methods accepts patterns - for pattern-based text processing,
you must use the Python `re` standard library module, an advanced tool. string methods may sometimes run more quickly than the re module's tools.**


# String Formatting

1. String formatting expressions: The original technique, available since Python's inception.
2. String formatting method calls: A newer technique added in Python 2.6 and 3.0.
	
	

## Expressions

Left of the % operator, provide a format string containing one or more embedded conversion targets, each of which starts with a % (e.g., %d).

Right of the % operator, provide the object (or objects, embedded in a tuple).

```python
>>> 'That is %d %s bird!' % (1, 'dead')
That is 1 dead bird!
```
### Advanced String Formatting Expressions

#### The list of type codes

![pic](https://raw.githubusercontent.com/doomdagger/learn-python/master/res/String-5.png "")

> General Structure of Conversion Targets: **`%[(name)][flags][width][.precision]typecode`**

* **name**: provide a dictionary key;
* **flags**: left justification (-), numeric sign (+), zero fills (0);
* **width**: total minimum field width;
* **precision**: number of digits after a decimal point;

Both *width* and *precision* can also be coded as a * to specify that they should take their values from the next item in the input values. 

```python
>>> "%0-10.*f" % (2, 10.1)
'10.10     '
>>> "%(good)-10.5d" % {'good': 10}
'00010     '
>>> food = 'spam'>>> qty = 10>>> vars(){'food': 'spam', 'qty': 10, ...plus built-in names set by Python... }
>>> '%(qty)d more %(food)s' % vars() # Variables are keys in vars()
'10 more spam'

# use dict method to make dictionaries
>>> template = '%(motto)s, %(pork)s and %(food)s'>>> template % dict(motto='spam', pork='ham', food='eggs')
'spam, ham and eggs'
```

## Method Calls

Unlike formatting expressions, formatting method calls are not closely based upon the C language’s “printf” model, and are sometimes more explicit in intent. 

### The Basics

```python
# by position
>>> template = '{0}, {1} and {2}'>>> template.format('spam', 'ham', 'eggs')
'spam, ham and eggs'

# by keyword
>>> template = '{motto}, {pork} and {food}'>>> template.format(motto='spam', pork='ham', food='eggs')
'spam, ham and eggs'

# by both
>>> template = '{motto}, {0} and {food}'>>> template.format('ham', motto='spam', food='eggs') 'spam, ham and eggs'

# by relative position
>>> template = '{}, {} and {}'>>> template.format('spam', 'ham', 'eggs')
'spam, ham and eggs'
```

> General Structure of Conversion Targets: **`{fieldname!conversionflag:formatspec}`**

* **fieldname**: a number or keyword naming an argument, followed by optional ".name" attribute or "[index]" component references.
* **conversionflag**: can be **r**, **s**, or **a** to call **repr**, **str**, or **ascii** built-in functions on the value, respectively.
* **formatspec**: specifies how the value should be presented, including details such as field width, alignment, padding, decimal precision, and so on, and ends with an optional data type code. `[[fill]align][sign][#][0][width][,][.precision][typecode]`

* ***fill*** can be any fill character other than { or }; 
* ***align*** may be <, >, =, or ^, for left alignment, right alignment, padding after a sign character, or centered alignment, respectively; 
* ***sign*** may be +, −, or space; 
* ***, (comma)*** option requests a comma for a thousands separator as of Python 2.7 and 3.1. 
* ***width and precision*** are much as in the % expression
> The **formatspec** may also contain nested {} format strings with field names only, to take values from the arguments list dynamically (much like the * in formatting expressions).

```python
>>> '{:{}10.2f}'.format(10.11, '^')
'  10.11   '
```

## Comparison between Expressions and Method Calls

* Has a handful of extra features not found in the % expression itself (though % can use alternatives)* Has more flexible value reference syntax (though it may be overkill, and % often has equivalents)* Can make substitution value references more explicit (though this is now optional)* Trades an operator for a more mnemonic method name (though this is also more verbose)* Does not allow different syntax for single and multiple values (though practice suggests this is trivial)* As a function can be used in places an expression cannot (though a one-line function renders this moot)

### Extra features: Special-case “batteries” versus general techniques

```python
>>> '{0:b}'.format((2 ** 16) − 1)
'1111111111111111'>>> '%b' % ((2 ** 16) − 1)ValueError: unsupported format character 'b'

>>> '{:,d}'.format(999999999999) # New str.format method feature in 3.1/2.7 '999,999,999,999'>>> '%s' % commas(999999999999) # But % is same with simple 8-line function '999,999,999,999'
```

### Functions versus expressions: A minor convenience

The final rationale for the format method—it’s a function that can appear where an expression cannot—requires more information about functions than we yet have at this point in the book, so we won’t dwell on it here. Suffice it to say that both the str.format method and the format built-in function can be passed to other functions, stored in other objects, and so on. An expression like % cannot directly, but this may be narrow-sighted—it’s trivial to wrap any expression in a one-line def or partial-line lambda once to turn it into a function with the same properties (though finding a reason to do so may be more challenging):

```python
def myformat(fmt, args): return fmt % args

# Call your function object # Versus calling the built-inmyformat('%s %s', (88, 99))
str.format('{} {}', 88, 99)# Your function is an object toootherfunction(myformat)
```

# General Type Categories

## Types Share Operation Sets by Categories

* **Numbers (integer, floating-point, decimal, fraction, others)**: Support addition, multiplication, etc.* **Sequences (strings, lists, tuples)**: Support indexing, slicing, concatenation, etc.
* **Mappings (dictionaries)**: Support indexing by key, etc.

## Mutable Types Can Be Changed in Place

* **Immutables (numbers, strings, tuples, frozensets)**: None of the object types in the immutable category support in-place changes, though we can always run expressions to make new objects and assign their results to variables as needed.
* **Mutables (lists, dictionaries, sets, bytearray)**: Conversely, the mutable types can always be changed in place with operations that do not create new objects. Although such objects can be copied, in-place changes support direct modification.

