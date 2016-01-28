Statements
==========


## Python's Statement

![s1](https://raw.githubusercontent.com/doomdagger/learn-python/master/res/Statement-1.png)
![s2](https://raw.githubusercontent.com/doomdagger/learn-python/master/res/Statement-2.png)

> **Caution**: `print` is technically neither a reserved word nor a statement in 3.X, but a built-in function call. `yield` is also an expression instead of a statement as of 2.5. As an expression, `yield` is also a reserved word, unlike `print`.

> **Caution**: For python 2.X. 
* `nonlocal` is not available.
* `print` is a statement instead of a built-in function call.


## Why Indentation Syntax?

Consider the following statement in C:
```c
if (x)	if (y)		statement1;else 
	statement2;
```
Which `if` does the `else` here go with? Surprisingly, the `else` is paired with the nested `if` statement (`if (y)`) in C, even though it looks visually as though it is associated with the outer `if (x)`. This is a classic pitfall in the C language.

This cannot happen in Python—because indentation is significant, the way the code looks is the way it will work. Consider an equivalent Python statement:

```python
if x:	if y:		statement1
else:	statement2
```
In this example, the `if` that the `else` lines up with vertically is the one it is associated with logically (the outer `if x`). In a sense, Python is a WYSIWYG language—what you see is what you get—because the way code looks is the way it runs, regardless of who coded it.

## A Few Special Cases

As mentioned previously, in Python’s syntax model:* The end of a line terminates the statement on that line (without semicolons).
* Nested statements are blocked and associated by their physical indentation (without braces).

### Statement rule special cases

Although statements normally appear one per line, it is possible to squeeze more than one statement onto a single line in Python by separating them with semicolons:```python
a = 1; b = 2; print(a + b)
```
This is the only place in Python where semicolons are required: as statement separators.

> **Caution**: This only works, though, if the statements thus combined are not themselves compound statements. **Compound statements** like `if` tests and `while` loops must still appear on lines of their own.

The other special rule for statements is essentially the inverse: you can make a single statement span across ***multiple lines***. To make this work, you simply have to enclose part of your statement in a bracketed pair—parentheses (`()`), square brackets (`[]`), or curly braces (`{}`). Any code enclosed in these constructs can cross multiple lines: your statement doesn’t end until Python reaches the line containing the closing part of the pair. ***(This technique works with compound statements, too, by the way)***

An older rule also allows for continuation lines when the prior line ends in a backslash:
```python
X = A + B + \
		C + D
```
**This alternative technique is dated, though**, and is frowned on today because it’s difficult to notice and maintain the backslashes. 

#### Block rule special caseThe body of a compound statement can instead appear on the same line as the header in Python, after the colon:

```python
if x > y: print(x)
```

This allows us to code single-line if statements, single-line while and for loops, and so on. Here again, though, this will work only if the body of the compound statement itself does not contain any compound statements. That is, only simple statements— assignments, prints, function calls, and the like—are allowed after the colon. 


> **Caution**: Differences between python 2.X and 3.X
* `raw_input` in 2.X
* `input` in 3.X
* `exec` is a statement in 2.X but a function in 3.X

