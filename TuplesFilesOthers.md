Tuples, Files, and Everything Else
==================================

## Tuples

* Ordered collections of arbitrary objects
* Accessed by offset
* Of the category “immutable sequence”
* Fixed-length, heterogeneous, and arbitrarily nestable
* Arrays of object references


|Operation  | Interpretation |
|--------------|--------------|
|()|An empty tuple|
|T = (0,)|A one-item tuple (not an expression)|
|T = (0, 'Ni', 1.2, 3)|A four-item tuple|
|T = 0, 'Ni', 1.2, 3|Another four-item tuple (same as prior line)|
|T = ('Bob', ('dev', 'mgr'))|Nested tuples|
|T = tuple('spam')|Tuple of items in an iterable|
|T[i]|Index, index of index, slice, length|
|T[i][j]||
|T[i:j]||
|len(T)||
|T1 + T2|Concatenate, repeat|
|T * 3||
|for x in T: print(x)|Iteration, membership|
|'spam' in T||
|[x ** 2 for x in T]||
|T.index('Ni')|Methods in 2.6, 2.7, and 3.X: search, count|
|T.count('Ni')||
|namedtuple('Emp', ['name', 'jobs'])|Named tuple extension type|


> **Caution**: Because parentheses can also enclose expressions, you need to do something special to tell Python when a single object in parentheses is a tuple object and not a simple expression. If you really want a single-item tuple, simply add a trailing comma after the single item, before the closing parenthesis: `a = (40,)`


Also, note that the rule about tuple immutability applies only to the top level of the tuple itself, not to its contents. A list inside a tuple, for instance, can be changed as usual:

```python
T = (1, [2, 3], 4)
T[1] = 'spam' 			# This fails: can't change tuple itself
# TypeError: object doesn't support item assignment

T[1][0] = 'spam' # This works: can change mutables inside
# (1, ['spam', 3], 4)
```

### Why Lists and Tuples?

> The notion of constantness is associated with objects in Python, not variables.

The immutability of tuples provides some integrity—you can be sure a tuple won’t be changed through another reference elsewhere in a program, but there’s no such guarantee for lists. Tuples and other immutables, therefore, serve a similar role to “constant” declarations in other languages.

### Named Tuples

We can implement objects that offer both **positional** and **named access** to record fields. For example, the `namedtuple` utility, available in the standard library’s collections module, implements an extension type that adds logic to tuples that allows components to be accessed by **both position** and **attribute name**, and **can be converted to dictionary-like form** for access by key if desired. Attribute names come from classes and are **not exactly dictionary keys**, but they are **similarly mnemonic**:

```python
from collections import namedtuple
Rec = namedtuple('Rec', ['name', 'age', 'jobs'])
bob = Rec('Bob', age=40.5, jobs=['dev', 'mgr'])
bob							# Rec(name='Bob', age=40.5, jobs=['dev', 'mgr'])bob[0], bob[2]				# access by position
bob.name, bob.jobs		# access by attribute

# Converting to a dictionary supports key-based behavior when needed
D = bob._asdict()
D	# OrderedDict([('name', 'Bob'), ('age', 40.5), ('jobs', ['dev', 'mgr'])])
```

As a quick preview, though, both tuples and named tuples support unpacking tuple assignment.

```python
bob = Rec('Bob', 40.5, ['dev', 'mgr'])
name, age, jobs = bob					# tuple assignment
```

Tuple-unpacking assignment doesn’t quite apply to dictionaries, short of fetching and converting keys and values and assuming or imposing an positional ordering on them.


## Files

Compared to the types you’ve seen so far, file objects are somewhat unusual. They are considered a **core type** because they are **created by a built-in function**, but they’re **not numbers, sequences, or mappings**, and they **don’t respond to expression operators**; they export only methods for common file-processing tasks.

|Operation|Interpretation|
|---------|--------------|
|output = open(r'C:\spam', 'w')|Create output file ('w' means write)|
|input = open('data', 'r')|Create input file ('r' means read)|
|input = open('data')|Same as prior line ('r' is the default)|
|aString = input.read()|Read entire file into a single string|
|aString = input.read(N)| Read up to next N characters (or bytes) into a string|
|aString = input.readline()|Read next line (including \n newline) into a string|
|aList = input.readlines()|Read entire file into list of line strings (with \n)|
|output.write(aString)|Write a string of characters (or bytes) into file|
|output.writelines(aList)|Write all line strings in a list into file|
|output.close()|Manual close (done for you when file is collected)|
|output.flush()|Flush output buffer to disk without closing|
|anyFile.seek(N)|Change file position to offset N for next operation|
|for line in open('data'): use line|File iterators read line by line|
|open('f.txt', encoding='latin-1')|Python 3.X Unicode text files (str strings)|
|open('f.bin', 'rb')|Python 3.X bytes files (bytes strings)|
|codecs.open('f.txt', encoding='utf8')|Python 2.X Unicode text files (unicode strings)|
|open('f.bin', 'rb')|Python 2.X bytes files (str strings)|

```python
afile = open(filename, mode)
```

* The `filename` may also contain non-ASCII Unicode characters that Python automatically translates to and from the underlying platform’s encoding, or be provided as a pre-encoded byte string.

* The second argument to open, processing mode, is typically the string `r` to open for text input (the default), `w` to create and open for text output, or `a` to open for appending text to the end (e.g., for adding to logfiles). The processing mode argument can specify additional options:
	* Adding a `b` to the mode string allows for binary data (end-of-line translations and 3.X Unicode encodings are turned off).	* Adding a `+` opens the file for both input and output(i.e.,you can both read and write to the same file object, often in conjunction with seek operations to reposition in the file).
* An optional third argument can be used to control output **buffering**—passing a zero means that output is unbuffered (it is transferred to the external file immediately on a write method call), and additional arguments may be provided for special types of files (e.g., an encoding for Unicode text files in Python 3.X).

### Using Files

* File iterators are best for reading lines
* Content is strings, not objects
* By default, files are buffered and seekable
* `close` is often optional: auto-close on collection. `close` calls may sometimes be required to flush buffered output of file objects not yet reclaimed

> **Notice**: Notice that the third `readline` call returns an empty string; this is how Python file methods tell you that you’ve reached the end of the file (empty lines in the file come back as strings containing just a newline character, not as empty strings).

> **Notice**: Notice that file write calls return the number of characters written in Python 3.X; in 2.X they don’t, so you won’t see these numbers echoed interactively. 

### Text and Binary Files: The Short Story

Python has always supported both text and binary files. 
* In Python 3.X there is a sharper distinction between the two:
	* Text files represent content as normal `str` strings, perform Unicode encoding and decoding automatically, and perform end-of-line translation by default.
	* Binary files represent content as a special bytes string type and allow programs to access file content unaltered.

* Python 2.X text files handle both 8-bit text and binary data, and a special string type (`u''`) and file interface (unicode strings and codecs.open) handles Unicode text.

If you need to handle internationalized applications or byte-oriented data, though, the distinction in 3.X impacts your code (usually for the better). In general, 
* you must use bytes strings for binary files. You should not open a binary data file in text mode—decoding its content to Unicode text will likely fail. In addition, binary files do not perform any end-of-line translation on data. byte strings are simply normal strings and have no leading b when displayed in Python 2.X.
* normal `str` strings for text files, because text files implement Unicode encodings. text files by default map all forms to and from `\n` when written and read and implement Unicode encodings on transfers in 3.X. text files must use the codecs module to add Unicode processing.

### Storing Python Objects in Files: Conversions

> File data is always strings in our scripts, and write methods do not do any automatic to-string formatting for us.

```python
[int(P) for P in parts]		# normal conversion
[eval(P) for P in parts]		# use eval, too dangerous
```

### Storing Native Python Objects: pickle

```python
# Pickle any object to file
D = {'a': 1, 'b': 2}
L = range(10)

F = open('datafile.pkl', 'wb')

import picklepickle.dump(D, F)
pickle.dump(L, F)F.close()

# Load any object from file
F = open('datafile.pkl', 'rb')
E = pickle.load(F)					# {'a': 1, 'b': 2}
G = pickle.load(F)					# [0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
```

> Binary mode is always required in Python 3.X, because the pickler creates and uses a bytes string object, and these objects imply binary- mode files (text-mode files imply str strings in 3.X).

For more on the pickle module, see the Python standard library manual, or import pickle and pass it to help interactively. While you’re exploring, also take a look at the shelve module. shelve is a tool that uses pickle to store Python objects in an access-by-key filesystem.

### Storing Python Objects in JSON Format

Because JSON is so close to Python dictionaries and lists in syntax, the translation to and from Python objects is trivial, and is automated by the `json` standard library module.

```python
name = dict(first='Bob', last='Smith')
rec = dict(name=name, job=['dev', 'mgr'], age=40.5)
import json
S = json.dumps(rec)		# '{"job": ["dev", "mgr"], "name": {"last": "Smith", "first": "Bob"}, "age": 40.5}'
O = json.loads(S)			# {'job': ['dev', 'mgr'], 'name': {'last': 'Smith', 'first': 'Bob'}, 'age': 40.5}
```

It’s similarly straightforward to translate Python objects to and from JSON data strings in files. Prior to being stored in a file, your data is simply Python objects; the JSON module recreates them from the JSON textual representation when it loads it from the file.

```python
json.dump(rec, fp=open('testjson.txt', 'w'), indent=4)
print(open('testjson.txt').read())
```

Note that strings are all Unicode in JSON to support text drawn from international character sets, so you’ll see a leading `u` on strings after translating from JSON data in Python 2.X (but not in 3.X).

For another semirelated tool that deals with formatted data files, see the standard library’s csv module. It parses and creates CSV (comma-separated value) data in files and strings.

```python
import csv
rdr = csv.reader(open('csvdata.txt'))
for row in rdr: print(row)
...
# ['a', 'bbb', 'cc', 'dddd']
# ['11', '22', '33', '44']
```

### Storing Packed Binary Data: struct

One other file-related note before we move on: some advanced applications also need to deal with packed binary data, created perhaps by a C language program or a network connection. Python’s standard library includes a tool to help in this domain—the `struct` module knows how to both compose and parse packed binary data. In a sense, this is another data-conversion tool that interprets strings in files as binary data.

```python
F = open('data.bin', 'wb')
import struct
data = struct.pack('>i4sh', 7, b'spam', 8)	# b'\x00\x00\x00\x07spam\x00\x08'
F.write(data)
F.close()
```

### File Context Managers

Though more a feature of exception processing than files themselves, it allows us to wrap file-processing code in a logic layer that ensures that the file will be closed (and if needed, have its output flushed to disk) automatically on exit, instead of relying on the auto-close during garbage collection:

```python
with open(r'C:\code\data.txt') as myfile:
	for line in myfile:
		print(line)
```

The `with` context manager scheme ensures **release of system resources** in all Pythons, and may be more useful for output files to **guarantee buffer flushes**; unlike the more general try, though, it is also limited to objects that support its protocol.

### Other File Tools

* Standard streams
* Descriptor files in the `os` module
* Sockets, pipes, and FIFOs
* Access-by-key files known as ***shelves***
* Shell command streams

## Core Types Review and Summary

* Objects share operations according to their category;for instance, sequence objects—`strings`, `lists`, and `tuples`—all share sequence operations such as **concatenation, length, and indexing**.
* Only mutable objects—`lists`, `dictionaries`, and `sets`—may be changed in place; you cannot change `numbers`, `strings`, or `tuples` in place.
* Files export only methods, so mutability doesn’t really apply to them—their state may be changed when they are processed, but this isn’t quite the same as Python core type mutability constraints.
* “Numbers”: `integer` (and the distinct long integer in 2.X), `floating point`, `complex`, `decimal`, and `fraction`
* “Strings”: `str`, as well as `bytes` in 3.X and `unicode` in 2.X; the `bytearray` string type in 3.X, 2.6, and 2.7 is mutable.
* Sets are something like the keys of a valueless dictionary, but they don’t map to values and are not ordered, so sets are neither a mapping nor a sequence type; `frozenset` is an immutable variant of `set`.

![classification](https://raw.githubusercontent.com/doomdagger/learn-python/master/res/Files-1.png)

### References Versus Copies

Assignments always store references to objects, not copies of those objects. Assignments can generate multiple references to the same object, though, it’s important to be aware that changing a mutable object in place may affect other references to the same object elsewhere in your program. If you don’t want such behaviour, you’ll need to tell Python to copy the object explicitly.

This is a feature—you can pass a large object around a program without generating expensive copies of it along the way. If you really do want copies, however, you can request them:
* Slice expressions with empty limits (`L[:]`) copy sequences.
* The dictionary, set, and list copy method (`X.copy()`) copies a dictionary, set, or list. (the list’s copy is new as of 3.3).
* Some built-in functions, such as `list` and `dict` make copies (`list(L)`, `dict(D)`, `set(S)`).
* The `copy` standard library module makes full copies when needed.### Comparisons, Equality, and Truth

Python comparisons always inspect all parts of compound objects until a result can be determined. In fact, when nested objects are present, Python automatically traverses data structures to apply comparisons from left to right, and as deeply as needed. The first difference found along the way determines the comparison result.

This is sometimes called a recursive comparison.

* **The `==` operator tests value equivalence**
* **The `is` operator tests object identity.**

As a rule of thumb, the `==` operator is what you will want to use for almost all equality checks; `is` is reserved for highly specialized roles.

> **Caution**: Dictionaries compare as equal if their sorted (key,value) lists are equal. ***Relative magnitude comparisons are not supported for dictionaries in Python 3.X***, but they work in 2.X as though comparing sorted (key, value) lists.

>The alternative in 3.X is to either write loops to compare values by key, or compare the sorted key/value lists manually—the items dictionary methods and sorted built-in suffice:
```python
sorted(D1.items()) < sorted(D2.items())
```

> **Caution**: Nonnumeric mixed-type magnitude comparisons (e.g.,`1<'spam'`) are ***errors*** in Python 3.X. They are allowed in Python 2.X, but use a fixed but arbitrary ordering rule based on type name string. By proxy, this also applies to sorts, which use comparisons internally: nonnumeric mixed-type collections cannot be sorted in 3.X.
```python
# error in python 3.X
11 > '12'		# error
# both correct in python 3.X and 2.X
11 == '11'		# False
```


### The Meaning of True and False in Python

* Numbers are false if zero, and true otherwise.
* Other objects are false if empty, and true otherwise.
* `None` (serves as an empty placeholder (much like a `NULL` pointer in C)) is false.

#### None

To preallocate a 100-item list such that you can add to any of the 100 offsets, you can fill it with None objects.


```python
L = [None] * 100
```

Keep in mind that `None` does not mean “undefined.” That is, `None` is something, not nothing (despite its name!)—it is a real object and a real piece of memory that is created and given a built-in name by Python itself.

## Python’s Type Hierarchies

The largest point to notice here is that ***everything*** in a Python system is an object type and may be processed by your Python programs. For instance, you can pass a class to a function, assign it to a variable, stuff it in a list or dictionary, and so on.

![type-hiera](https://raw.githubusercontent.com/doomdagger/learn-python/master/res/Files-2.png)

## Type Objects

One note on type names: as of Python 2.2, each core type has a new built-in name added to support type customization through object-oriented subclassing: `dict`, `list`, `str`, `tuple`, `int`, `float`, `complex`, `bytes`, `type`, `set`, and more. In Python 3.X names all references classes, and in Python 2.X but not 3.X, `file` is also a type name and a synonym for `open`. Calls to these names are really **object constructor calls**, not simply conversion functions, though you can treat them as simple functions for basic usage.

In addition, the `types` standard library module in Python 3.X provides additional type names for types that are not available as built-ins (e.g., the type of a function; in Python 2.X but not 3.X, this module also includes synonyms for built-in type names).

It is possible to do type tests with the `isinstance` function.

```python
type([1]) == type([])
isinstance([], list)			# recommended

import types
def f(): pass
type(f) == types.FunctionType
```

### Other Types in Python

The main difference between these extra tools and the built-in types we’ve seen so far is that the built-ins provide special language creation syntax for their objects (e.g., 4 for an integer, [1,2] for a list, the open function for files, and def and lambda for functions). Other tools are generally made available in standard library modules that you must first import to use, and aren’t usually considered core types. For instance, to make a regular expression object, you import `re` and call `re.compile()`.

## Built-in Type Gotchas

* Assignment Creates References, Not Copies
* Repetition Adds One Level Deep

> **Caution**: For instance, in the following example `X` is assigned to `L` repeated four times, whereas `Y` is assigned to a list containing `L` repeated four times.
```python
L = [4, 5, 6]X = L * 4		# like [4, 5, 6] + [4, 5, 6] + ...
X = [L] * 4	# like [L] + [L] + ... = [L, L, L, L]
```
**That is absolutely what we don't want:**
```python
L[1] = 0		# impact Y but not X
X
# [4, 5, 6, 4, 5, 6, 4, 5, 6, 4, 5, 6]
Y
# [[4, 0, 6], [4, 0, 6], [4, 0, 6], [4, 0, 6]] 
```
**Try this then:**
```python
Y = [list(L)] * 4
```
Even more subtly, although `Y` doesn’t share an object with `L` anymore, **it still embeds four references to the same copy of it.**
```python
Y = [list(L) for i in range(4)]		# works
```
If you remember that **repetition, concatenation, and slicing copy only the top level of their operand objects**, these sorts of cases make much more sense.

* Beware of Cyclic Data Structures

	* Don’t use cyclic references unless you really need to, and make sure you anticipate them in programs that must care. There are good reasons to create cycles, but unless you have code that knows how to handle them, objects that reference themselves may be more surprise than asset.

* Immutable Types Can’t Be Changed in Place

