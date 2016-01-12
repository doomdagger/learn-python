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

![pic](https://raw.githubusercontent.com/doomdagger/learn-python/master/res/Numbers-1.jpg "")

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

### Str and Repr Display Formats

* repr (and the default interactive echo) produces results that look as though they were code
* str (and the print operation) converts to a typically more user-friendly format if available

```python
num = 1 / 3

repr(num)    # '0.3333333333333331'

str(num)     # '0.333333333333333'
```

### Comparisons: Normal and Chained

Python allows us to chain multiple comparisons together to perform range tests

```python

1 < 2 < 3.0 < 4    # True

```

### Division: Classic, Floor, and True

#### x / y
```python
# in python 2.6: classic division - not trunc
1 / 2    # 0
1 / 2.0  # 0.5
5 / -2   # -3

# in python 3.0: true division
1 / 2    # 0.5
1 / 2.0  # 0.5
5 / -2   # -2.5
```

#### x // y : floor division
```python
# in python 2.6 & 3.0
1 // 2    # 0
1 // 2.0  # 0.0
5 // -2   # -3
5 // -2.0 # -3.0
```

> Support either Python:
```python
y // z    # Always truncates, always an int result for ints in 2.X and 3.X
y / float(z)    # Guarantees float division with remainder in either 2.X or 3.X
```

### Floor versus truncation

```python
import math

math.floor(2.5)        # 2

math.floor(-2.5)       # -3

math.trunc(2.5)        # 2

math.trunc(-2.5)       # -2
```

### Hexadecimal, Octal, and Binary Notation

#### literal
* 0o1, 0o20, 0o377
* 0x01, 0x10, 0xFF
* 0b1, 0b10000, 0b1111

#### built-in functions
* oct(64)
* hex(64)
* bin(64)
* int('0b11'|'0o77'|'0xFF', 2|8|16)
* eval('should be python code')

### Bitwise Operations
```python
x = 1

x << 2

x | 2

x & 1
```
> Tips:
> * use bit_length method to query the number of bits required to represent a number's value in binary.
`99.bit_length() == len(bin(99))-2`

### Other Built-in Numeric Tools
```python
import math

math.pi
math.e
math.sin(2 * math.pi / 180)
math.sqrt(144)
math.floor(122.6)
math.trunc(-2.567)

pow(2, 4)
round(122.4)
abs(-1)
sum((1,2,3,4))    # accept a tuple
min(3,1,2,4)
max(3,1,2,3)

import random

random.random()
random.randint(1, 4)    # 1, 2, 3, 4 
random.choice(list('1234567890'))

```

### Decimal Type

#### Why
```python
0.1 + 0.1 + 0.1 - 0.3    # 5.551115123125783e-17

from decimal import Decimal
Decimal('0.1')
Decimal('0.2')
Decimal('0.3')

```

#### Setting precision globally
```python
import decimal

decimal.Decimal(1) / decimal.Decimal(2)    # Decimal('0.1428571428571428571428571429')

# setting precision globally
decimal.getcontext().prec = 4

decimal.Decimal(1) / decimal.Decimal(2)    # Decimal('0.1429')

```

#### Setting precision locally
```python
import decimal

with decimal.localcontext() as ctx:
    ctx.prec = 2
    decimal.Decimal(1.00) / decimal.Decimal(3.00)
```

### Fraction Type

#### Basics
```python
from fractions import Fraction

x = Fraction(1, 3)
y = Fraction(4, 6)

z = Fraction('.25')    # Fraction(1, 4)
```

#### Conversions and mixed types
```python
(2.5).as_integer_ratio()    # return a tuple: (5, 2)

# one way
Fraction(*(2.5).as_integer_ratio())

# another
Fraction.from_float(1.75)

# convert back to float
float(Fraction(1, 4))

# mixtures

Fraction(1, 3) + 2        # Fraction(7, 3)

Fraction(1, 3) + 2.0      # 2.333333333335

Fraction(1, 3) + Fraction(2, 1)    # Fraction(7, 3)

```
> Tips: Avoid Precision Loss
* `Fraction(*(4.0 / 3).as_integer_ratio())` can result in `Fraction(6004799503160661, 4503599627370496)`
* To overcome this condition, try this: `fraction_type_object = fraction_type_object.limit_denominator(10)`, the result above then will be `Fraction(4, 3)`

### Set

#### Basics
```python
# one way to initialize a set
{1, 2, '3', 4}

# another
set([1, 2, '3', 4])

```

#### Operations
```python
x = {1, 2, 3, 4, 'a', 'b'}
y = {1, 2, 3}

2 in x

x - y    # difference

x | y    # union -> x.union(y)

x & y    # intersection -> x.intersection(y)

x ^ y    # symmetric difference -> (x | y) - (x & y)

x > y    # is x a superset of y -> x.issuperset(y)

x < y    # is x a subset of y -> x.issubset(y)

```

##### In-place operations
```python
x.add(3)

x.update({0, 9})

x.remove(1)
```

> Notion: Set literals in Python2.6 & Python 3.0
```python
# in 2.6
set([1,2,3,4])
# in 3.0
{1, 2, 3, 4}
```
* But `{}` is still a dictionary in Python. Empty sets must be created with the set built-in `set()`

> Notion: sets can only contain immutable (a.k.a. ¡°hashable¡±) object types. Hence, lists and dictionaries cannot be embedded in sets, but tuples can if you need to store compound values.

### Booleans

#### Nature

Internally, the names True and False are instances of bool, which is in turn just a subclass (in the objectoriented sense) of the built-in integer type int. True and False behave exactly like the
integers 1 and 0, except that they have customized printing logic¡ªthey print themselves as the words True and False, instead of the digits 1 and 0. bool accomplishes this by redefining str and repr string formats for its two objects.

```python

type(True)    # <class 'bool'>

isinstance(True, int)    # True

True == 1    # same value

True is 1    # But different object

True + 4     # 5

```
