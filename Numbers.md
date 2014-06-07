Numbers
=======

### Overview

```python
123 + 222
1.5 * 4
2 ** 100

len(str(2 ** 1000000))

3.1415 * 2

import math
math.pi
math.sqrt(85)

import random
random.random()
random.choice([1,2,3,4])
```


### Numeric Literals

```python
# Integers(unlimited size)
1234, -24, 0, 999999999999999999

# Floating-point numbers
1.23, 1., 3.14e-10, 4E210, 4.0e+210

# Octal, hex, and binary literals in 2.6
0177, 0x9ff, 0b101010

# Octal, hex, and binary literals in 3.0
0o177, 0x9ff, 0b101010

# Complex number literals
3+4j, 3.0+4.0j, 3J
```

> Different Integer definitions in 2.6 & 3.0:
* Integers in Python 2.6: normal(32 bits) and long(unlimited precision)
* Integers in Python 3.0: a single type(unlimited precision)

```python

# integer to hex string
hex(32)

# integer to octal string
oct(32)

# integer to binary string
bin(32)

# x string to integer
int('0b1010', 2)
int('0o77', 8)
int('0xab', 16)

# complex numbers
1.2 + 2.1j
complex(1.2, 2.1)
```

### Built-in Numeric Tools

```python
# Expression operators
+, -, *, /, >>, **, &, etc.

# Built-in mathematical functions
pow, abs, round, int, hex, bin, etc.

# Utility modules
random, math, etc.
```

### Python Expression Operators

![pic](http://git.candylee.cn/doomdagger/learn-python/raw/master/Numbers-1.jpg "")

> Difference in division between 2.6 & 3.0:
* The X // Y floor division expression always truncates fractional remainders in both Python 2.X and 3.X. The X / Y expression performs true division in 3.X (retaining remainders) and classic division in 2.X (truncating for integers).

```python
# Comparison operators may be chained:
if x < y < z:
    print("hello")

# floor division in 3.0 & 2.6
3 // 2    # 1
3.0 // 2  # 1.0

# true division in 3.0
3 / 2     # 1.5 
3.0 / 2   # 1.5

# truncating for integers in 2.6
3 / 2     # 1
3.0 / 2   # 1.5

# number type converting
int(3.1415)    # 3
float(3)       # 3.0
```

> Caution in Python!
* `'str' + 1      # error!`
* Python does not convert across any other type boundaries automatically. Adding a string to an integer, for example,results in an error, unless you manually convert one or the other


> Caution in Python 3.0:
* In 3.X, nonnumeric mixed-type magnitude comparisons are never allowed and raise exceptions. Note that this applies to comparison operators such as > only; other operators like + do not allow mixed nonnumeric types in either 3.X or 2.X

