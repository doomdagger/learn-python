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

![pic](http://git.candylee.cn/doomdagger/learn-python/raw/master/res/ListsDictionaries-1.jpg "")