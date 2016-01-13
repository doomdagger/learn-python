Dynamic Typing Interlude
========================

## Variables, Objects and References

In sum, variables are created when assigned, can reference any type of object, and must be assigned before they are referenced.


```python
a = 3

"""
1. Create an object to represent the value 3.
2. Create the variable a, if it does not yet exist.
3. Link the variable a to the new object 3.
"""
```

Each object also has two standard header fields: **a type designator** used to mark the type of the object, and **a reference counter** used to determine when it's OK to reclaim the object.


## Types Live with Objects, Not Variables

Names have no types; as stated earlier, types live with objects, not names. Objects, on the other hand, know what type they are¡ªeach object contains a header field that tags the object with its type.

## Objects Are Garbage-Collected

**Caution cyclic reference:** Python's garbage collection is based mainly upon reference counters, as described here; however, it also has a component that detects and reclaims objects with cyclic references in time. This component can be disabled if you're sure
that your code doesn't create cycles, but it is enabled by default.

## Shared References

**Definition:**This scenario in Python¡ªwith multiple names referencing the same object¡ªis usually called a shared reference (and sometimes just a shared object).

> How to copy objects instead of making references:
```python
# using built-in function
list([1, 2, 3])
# using slice from start to finish
L1 = [1, 2, 3]
L1[:]
# using copy module
import copy
copy.copy(L1)
copy.deepcopy(L1)    # all the elements and object itself will be copied 
```


## Shared References and Equality

```python
L = [1, 2, 3]
M = L

L == M        # same value
L is M        # same object
```

#### want to get the reference count of an object?
```python
import sys
sys.getrefcount(1)
```
