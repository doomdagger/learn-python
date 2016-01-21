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
L[-1:] = [5]    # L.append(5)
L[:0] = [5]         # L.insert(0, 5)
```

#### More on sorting lists

```python
# default sorts in ascending order 
L.sort()# use the return value from str.lower as the sorting keyL.sort(key=str.lower)# use the newly defined comparison method for comparison, return 1 ,0 ,-1L.sort(cmp=new_cmp_func)# the reverse argument allows sorts to be made in descending orderL.sort(key=str.lower, reverse=True)
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




