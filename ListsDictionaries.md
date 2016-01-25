Lists and Dictionaries
======================

## Lists

### Overview

* Ordered collections of arbitrary objects
* Accessed by offset
* Variable-length, heterogeneous, and arbitrarily nestable
* Of the category "mutable sequence"
* **Arrays of object references**

### List Operations

|Operation  | Interpretation |
|--------------|--------------|
|L = []  | An empty list |
|L = [0, 1, 2, 3]  | Four items: indexes 0..3 |
|L = ['abc', ['def', 'ghi']]   | Nested sublists |
|L = list(range(-4, 4))    | |
|L[i]| Index, index of index, slice, length |
|L[i][j] | |
|L[i:j]||
|len(L)||
| L1 + L2 | Concatenate, repeat |
| L * 3 ||
|for x in L: print(x)|Iteration, membership|
|3 in L||
|L.append(4)|Methods: growing|
|L.extend([5, 6, 7])||
|L.insert(I, X)||
|L.index(1)|Methods: searching|
|L.count(X)||
|L.sort()|Methods: sorting, reversing, etc.|
|L.reverse()||
|del L[k]|Methods, statement: shrinking|
|del L[i:j]||
|L.pop()||
|L.remove(2)||
|L[i:j] = []||
|L[i:j] = {4, 5, 6]|slice assignment|
|L = [x**2 for x in range(10)]|List comprehensions and maps|
|**list(map(ord, 'spam'))**||


## Changing Lists In-Place

#### Index and slice assignments

* **Deletion**. The slice you specify to the left of the = is deleted.
* **Insertion**. The new items contained in the iterable object to the right of the = are inserted into the list on the left, at the place where the old slice was deleted.

Because the length of the sequence being as- signed does not have to match the length of the slice being assigned to, slice assignment can be used to **replace (by overwriting)**, **expand (by inserting)**, or **shrink (by deleting)** the subject list.

> **Caution:**
This description requires elaboration when the value and the slice being assigned overlap: **`L[2:5]=L[3:6]`**, for instance, works fine because the value to be inserted is fetched before the deletion
happens on the left.

> **Tips:** You
can also mimic append with the clever slice assignments of the prior section:
```python
# equal operations
L = [1, 2, 3, 4]
L[len(L):] = [5]    # L.append(5)
L[:0] = [5]         # L.insert(0, 5)
```

#### More on sorting lists

```python
# default sorts in ascending order 
L.sort()
# use the return value from str.lower as the sorting key
L.sort(key=str.lower)
# use the newly defined comparison method for comparison, return 1 ,0 ,-1
L.sort(cmp=new_cmp_func)
# the reverse argument allows sorts to be made in descending order
L.sort(key=str.lower, reverse=True)

```

> **Caution:** `[1, 2, 'spam'].sort()` succeeds in Python 2.X but will raise an exception in Python 3.X. Sorting mixed-types fails by proxy.  Really need to consider this when sorting mix-types list.


> **tips**: beware that ``append`` and ``sort`` change the associated list object in place, but don’t return the list as a result (technically, they both return a value called None).


sorting is also available in recent Pythons as a built-in function, which sorts any collection (not just lists) and returns a new list for the result (instead of in-place changes).

```python
# return a new list of the result, no in-place changes for the original list
sorted(iterable, cmp=None, key=None, reverse=False) --> new sorted list
```

#### Other common list methods

```python
# add many items at end (in-place)
L.extend([3, 4, 5])

# Delete and return last item (in-place)
L.pop()

# In-place reversal method
L.reverse()

# Reversal built-in with a result (iterator!!!)
>>> type(reversed(L))
<type 'listreverseiterator'>

list(reversed(L))
```

> **Caution**: Difference between `extend` and `append`.
```python
L = [0]
L.extend([1, 2, 3])		# [0, 1, 2, 3]
L.append([1, 2, 3])		# [0, 1, 2, 3, [1, 2, 3]]
```

```python
L = ['spam', 'eggs', 'ham']

L.index('eggs')				# 1
L.insert(1, 'toast')		# ['spam', 'toast', 'eggs', 'ham']
L.remove('eggs')			# ['spam', 'toast', 'ham']
L.pop(1)					# return 'toast', and now L: ['spam', 'eggs', 'ham']
L.count('spam')				# 1 (number of occurrence of 'spam')

del L[0]					# L.pop(0)
del L[1:]					# L[1:] = []

L2 = L[:]					# top-level copy
L2 = list(L)				# top-level copy
L2 = copy.copy(L)			# top-level copy
L2 = copy.deepcopy(L)		# deep copy
```


## Dictionaries

### Overview

* Accessed by key, not offset position
* Unordered collections of arbitrary objects
* Variable-length, heterogeneous, and arbitrarily nestable
* Of the category “mutable mapping”
* Tables of object references (hash tables)

![dictionary](https://raw.githubusercontent.com/doomdagger/learn-python/master/res/ListsDictionaries-2.png)

### More Dictionary Methods

```python
zip(L.keys(), L.values())		# list of tuples

L.items()						# list of tuples

list(table.items())
# map comprehension
[title for (title, year) in table.items() if year == '1975']

```

### Dictionary Usage Notes

* Sequence operations don’t work: things like **concatenation** (an ordered joining) and **slicing** (extracting a contiguous section) simply don’t apply.
* Assigning to new indexes adds entries
* Keys need not always be strings: any immutable objects work just as well.

### Tricks

#### Using dictionaries to simulate flexible lists: Integer keys

```python
# Normal List
L = []
L[99] = 'spam'
# Traceback (most recent call last):
# File "<stdin>", line 1, in ?
# IndexError: list assignment index out of range

# Dictionary
D = {}
D[99] = 'spam'			# {99: 'spam'}
```

#### Using dictionaries for sparse data structures: Tuple keys

for example, multidimensional arrays where only a few positions have values stored in them

```python
# Sparse data structure, multidimensional array
Matrix = {}
Matrix[(2, 3, 4)] = 88
Matrix[(7, 8, 9)] = 99
X = 2; Y = 3; Z = 4
Matrix[(X, Y, Z)] 				# 88
```

> **Caution**: accessing an empty slot triggers a nonexistent key exception, as these slots are not physically stored. To avoiding missing-key errors, use a **try** statement to catch and recover from the exception explicitly, or simply use the dictionary **get** method shown earlier to provide a default for keys that do not exist.

```python
# use if and else
if (2, 3, 6) in Matrix:
	print(Matrix[(2, 3, 6)])
else:
	print(0)
# use try statement
try:
	print(Matrix[(2, 3, 6)])
except KeyError:
	print(0)
# use get method and provide default value
Matrix.get((2, 3, 4), 0)
```

#### Other Ways to Make Dictionaries

```python
# spell out the entire dictionary ahead of time
{'name': 'Bob', 'age': 40}
# create the dictionary on the fly
D = {}
D['name'] = 'Bob' D['age'] = 40
# use constructors to create dictionaries
dict(name='Bob', age=40)
dict([('name', 'Bob'), ('age', 40)])
dict(zip(keyslist, valueslist))
dict.fromkeys(keyslist, default_value)
```

In practice, dictionaries tend to be best for data with labeled components, as well as structures that can benefit from quick, direct lookups by name, instead of slower linear searches. As we’ve seen, **they also may be better for sparse collections and collections that grow at arbitrary positions.**

### Dictionary Changes in Python 3.X and 2.7

Dictionaries in Python 3.X:
* Support a new dictionary comprehension expression, a close cousin to list and set comprehensions
* Return set-like iterable views instead of lists for the methods `D.keys`, `D.values`, and `D.items`
* Require new coding styles for scanning by sorted keys, because of the prior point (`list(...)`)
* No longer support relative magnitude comparisons directly - compare manually instead
* No longer have the `D.has_key` method—the `in` membership test is used instead

Dictionaries in Python 2.7:
* Support item1 in the prior list—dictionary comprehensions—as a direct back-port from 3.X
* Support item 2 in the prior list—set-like iterable views—but do so with special method names `D.viewkeys`, `D.viewvalues`, `D.viewitems`); their nonview methods return lists as before


#### Comprehension
```python
D = {k: v for (k, v) in zip(['a', 'b', 'c'], [1, 2, 3])}

D = {c.lower(): c + '!' for c in ['SPAM', 'EGGS', 'HAM']}

```

#### Dictionary views

View objects are **iterables**, which simply means objects that **generate result items one at a time**, instead of producing the result list all at once in memory. Besides being iterable, dictionary views also retain the original order of dictionary components, **reflect future changes to the dictionary**, and may **support set operations**. **On the other hand**, because they are not lists, they **do not directly support operations like indexing or the list sort method**, and **do not display their items as a normal list when printed**.


> Dictionary views dynamically reflect future changes made to the dictionary after the view object has been created.


```python
D = {'a': 1, 'b': 2, 'c': 3} 
K = D.keys() 
V = D.values()
list(K)			# ['b', 'c', 'a'] 
list(V)			# [2, 3, 1]
del D['b']
list(K) 		# ['c', 'a'] 
list(V) 		# [3, 1]
```

> View objects returned by the `keys` method are set-like and support common set operations such as intersection and union; `values` views are not set-like, but items results are if their (key, value) pairs are unique and hashable (immutable). 

```python
a = {1: 'g', 2: 'e', 3: 'r', 4: 'r'}
b = {1: 'fd', 5: 'fdad'}
a.viewvalues() & b.viewvalues()
# Traceback (most recent call last):
#   File "<stdin>", line 1, in <module>
# TypeError: unsupported operand type(s) for &: 'dict_values' and 'dict_values'

a.viewkeys() & b.viewvalues()		# correct
a.viewkyes() & b.viewkeys()			# correct
```

**Items views are set-like too if they are hashable—that is, if they contain only immutable objects.**

#### Sorting dictionary keys

```python
D = {'a': 1, 'b': 2, 'c': 3}
Ks = list(D.keys())					# use list to wrap dict.keys method in 3.X
Ks.sort()							# dictionary views do not have sort method
for k in Ks: print(k, D[k])

Ks = D.keys()						# or we can use unified sorted method
for k in sorted(Ks): print(k, D[k])

for k in sorted(D): print(k, D[k])	# or we can sort dictionary directly
```

#### Dictionary magnitude comparisons no longer work in 3.X

You can simulate it by comparing sorted keys lists manually:
```python
sorted(D1.items()) < sorted(D2.items())		# Like 2.X D1 < D2
```

#### The `has_key` method is dead in 3.X: Use `in` for Unity

```python
D.has_key('c')			# in 2.X only

'c' in D				# both 2.X and 3.X work
```

