Assignments, Expressions and Prints
==================================

## Assignment Statements

* Assignments create object references
* Names are created when first assigned
* Names must be assigned before being referenced
* Some operations perform assignments implicitly


|Operation|Interpretation|
|----------|---------|
|spam = 'Spam'|Basic form|
|spam, ham = 'yum', 'YUM'|Tuple assignment (positional)|
|[spam, ham] = ['yum', 'YUM']|List assignment (positional)|
|a, b, c, d = 'spam'|Sequence assignment, generalized|
|a, *b = 'spam'|Extended sequence unpacking (Python 3.X)|
|spam = ham = 'lunch'|Multiple-target assignment|
|spams += 42|Augmented assignment (equivalent to spams = spams + 42)|

### Tuple- and list-unpacking assignments

The second and third forms in the table are related. When you code a tuple or list on the left side of the =, Python pairs objects on the right side with targets on the left by position and assigns them from left to right.

> **Common Trick**: Because Python creates a **temporary tuple** that saves the original values of the variables on the right while the statement runs, unpacking assignments are also a way to swap two variables’ values without creating a temporary variable of your own—the tuple on the right remembers the prior values of the variables automatically:
```python
nudge = 1
wink = 2
nudge, wink = wink, nudge 	 # Tuples: swaps values
nudge, wink
```

### Sequence assignments

In later versions of Python, tuple and list assignments were generalized into instances of what we now call ***sequence assignment***—any sequence of names can be assigned to any sequence of values, and Python assigns the items one at a time by position. We can even mix and match the types of the sequences involved. 

In fact, the original tuple and list assignment forms in Python have been generalized to accept any type of sequence (really, iterable) on the right as long as it is of the same length as the sequence on the left.

**Technically speaking, sequence assignment actually supports any iterable object on the right, not just any sequence.**

Normally in 3.X—and always in 2.X—the number of items in the assignment target and subject must match, but Python 3.X allows us to be more general with extended unpacking * syntax. To be more flexible, we can ***slice in both 2.X and 3.X***. There are a variety of ways to employ slicing to make this last case work:
```python
string = 'SPAM'
a, b, c = string[0], string[1], string[2:]

(a, b), c = string[:2], string[2:]			# Nested sequences
```

As the last example in this interaction demonstrates, we can even assign ***nested sequences***, and Python unpacks their parts according to their shape, as expected.

This technique also works in `for` loops and works for **function argument lists** in ***Python 2.X (though not in 3.X)***.

```python
for (a, b, c) in [(1, 2, 3), (4, 5, 6)]: ... # Simple tuple assignment

for ((a, b), c) in [((1, 2), 3), ((4, 5), 6)]: ... # Nested tuple assignment

def f(((a, b), c)): ... # For arguments too in Python 2.X, but not 3.X

f(((1, 2), 3))
```

> **Tips**: Assigning an integer series to a set of variables,
```python
red, green, blue = range(3)
```
> It’s Python’s equivalent of the ***enumerated data types*** you may have seen in other languages. The `range` built-in function generates a list of successive integers. And **in 3.X only, it requires a list around it if you wish to display its values all at once like this**.
```python
# Only in python 3.X
type(range(10))			# <class 'range'>, not list
type(list(range))			# <class 'list'>
```
Another place you may see a tuple assignment at work is for splitting a sequence into its front and the rest in loops like this:
```python
L = [1, 2, 3, 4]
while L:
	front, L = L[0], L[1:]
	print(front, L)
```


### Extended sequence unpacking

In Python 3.X (only), a new form of sequence assignment allows us to be more flexible in how we select portions of a sequence to assign. `a, *b = 'spam'` a is assigned 's', and b is assigned 'pam'. ***This is especially handy for common coding patterns such as splitting a sequence into its “front” and “rest”.***

**In fact, the starred name can appear anywhere in the target.**
```python
# b matches the last item in the se- quence, and a matches everything before the last
*a, b = [1, 2, 3, 4]		# a = [1, 2, 3], b = 4

# When the starred name appears in the middle, it collects everything between the other names listed.
a, *b, c = [1, 2, 3, 4]	# a = 1, b = [2, 3], c = 4
```
Extended sequence unpacking syntax works for any sequence types (really, again, any iterable), not just lists. 

This is **similar** in spirit to slicing, but **not exactly the same**—a sequence unpacking assignment always returns a list for multiple matched items, whereas slicing returns a sequence of the same type as the object sliced.

```python
L = [1, 2, 3, 4]
while L:
	front, *L = L
	print(front, L)
```

> **Caution**: 
* The starred name may match just a single item, but is **always assigned a list**:
>
```python
seq = [1, 2, 3, 4]
a, b, c, *d = seq
print(a, b, c, d)		# 1 2 3 [4]
```
* If there is nothing left to match the starred name, it is assigned an empty list, **regardless of where it appears**. In the following, a, b, c, and d have matched every item in the sequence, but Python assigns e an empty list **instead of treating this as an error case**.
>
```python
seq = [1, 2, 3, 4]
a, b, c, d, *e = seq
print(a, b, c, d, e)		# 1 2 3 4 []
a, b, *e, c, d = seq
print(a, b, c, d, e)		# 1 2 3 4 []
```
* Errors can still be triggered if there is more than one starred name, if there are too few values and no star (as before), and if the starred name is not itself coded inside a sequence:
>
```python
a, *b, c, *d = seq		# SyntaxError: two starred expressions in assignment
a, b = seq					# ValueError: too many values to unpack (expected 2)
*a = seq					# SyntaxError: starred assignment target must be in a list or tuple
*a, = seq					# [1, 2, 3, 4]
```

#### Application to for loops

```python
for (a, *b, c) in [(1, 2, 3, 4), (5, 6, 7, 8)]:
	...					# a, *b, c = (1, 2, 3, 4)

for (a, b, c) in [(1, 2, 3), (4, 5, 6)]:
	...					# a, b, c = (1, 2, 3)
```


### Multiple-target assignments

Just multiple references.

### Augmented Assignments

> **Caution**: C/C++ programmers take note: although Python now supports statements like `X += Y`, it still does not have C’s auto-increment/decrement operators (e.g., `X++`, `−−X`). These don’t quite map to the Python object model because Python has no notion of in-place changes to immutable objects like numbers.

Augmented assignments have three advantages:
* There’s less for you to type. Need I say more?
* The left side has to be evaluated only once.
* The optimal technique is automatically chosen. That is,for objects that support in-place changes, the augmented forms automatically perform in-place change operations instead of slower copies.

```python
L += [9, 10]			# Mapped to L.extend([9, 10])
```

#### Augmented assignment and shared references

```python
L = [1, 2]
M = L				# L and M reference the same object
L = L + [3, 4]		# Concatenation makes a new object, Changes L but not M
L, M				# ([1, 2, 3, 4], [1, 2])

L = [1, 2]
M = L				# L and M reference the same object
L += [3, 4]		# But += really means extend, M sees the in-place change too!
L, M				# ([1, 2, 3, 4], [1, 2, 3, 4])
```


## Variable Name Rules

* Syntax: (underscore or letter) + (any number of letters, digits, or underscores)
* Case matters: `SPAM` is not the same as `spam`
* Reserved words are off-limits, if you try to use a variable name like class, Python will raise a syntax error

