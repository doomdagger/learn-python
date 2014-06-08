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

Each object also has two standard header fields: a type designator used to mark the type of the object, and a reference counter used to determine when it¡¯s OK to reclaim the object.


## Types Live with Objects, Not Variables

Names have no types; as stated earlier, types live with objects, not names. Objects, on the other hand, know what type they are¡ªeach object contains a header field that tags the object with its type.

## Objects Are Garbage-Collected

