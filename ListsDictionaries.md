Lists and Dictionaries
======================

# Overview

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


