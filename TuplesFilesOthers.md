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

