# An Array of Sequences
## Overview of Built-In Sequences
Container sequences
* list,  tuple, and  collections.deque can hold items of different types.

Flat sequences
* str,  bytes,  bytearray,  memoryview, and  array.array hold items of one type.

> Keeping in mind these common traits—mutable versus immutable; container versus
flat—is helpful to extrapolate what you know about one sequence type to others.

## List Comprehensions and Generator Expressions
### List Comprehensions and Readability
略

### Listcomps Versus map and filter
ListComps 不並不一定優於 mpa/filter

### Cartesian Products
略

### Generator Expressions
Genexps 比 Listexps 還省記憶體空間, 他並非建立整個list而是透過iterator protocol逐個地產出元素

## Tuples Are Not Just Immutable Lists
### Tuples as Records
略
### Tuple Unpacking
* 當要把tuple內的元素當作function的輸入參數時, 在前面加上*
``` python
# example1
lax_coordinates = (33.9425, -118.408056)
latitude, longitude = lax_coordinates  # tuple unpacking

# example2
>>> divmod(20, 8)
(2, 4)
>>> t = (20, 8)
>>> divmod(*t) # 當要把tuple內的元素當作function的輸入參數時, 在前面加上*
(2, 4)
```

### Using * to grab excess items
* parallel assignment
``` python
>>> a, b, *rest = range(5)
>>> a, b, rest
(0, 1, [2, 3, 4])
>>> a, b, *rest = range(3)
>>> a, b, rest
(0, 1, [2])
>>> a, b, *rest = range(2)
>>> a, b, rest
(0, 1, [])
```
``` python
>>> a, *body, c, d = range(5)
>>> a, body, c, d
(0, [1, 2], 3, 4)
>>> *head, b, c, d = range(5)
>>> head, b, c, d
([0, 1], 2, 3, 4)
```

## Named Tuples
如果要做為儲存records用途的話, name tuple 比 tuple 還增加 field name 和 class name 特性
> The collections.namedtuple function is a factory that produces subclasses of tuple
enhanced with field names and a class name—which helps debugging.

``` python
>>> from collections import namedtuple
>>> City = namedtuple('City', 'name country population coordinates')  
>>> tokyo = City('Tokyo', 'JP', 36.933, (35.689722, 139.691667))  
>>> tokyo
City(name='Tokyo', country='JP', population=36.933, coordinates=(35.689722,
139.691667))
>>> tokyo.population  
36.933
>>> tokyo.coordinates
(35.689722, 139.691667)
>>> tokyo[1]
'JP'
```

## Tuples as Immutable Lists
略

## Slicing
> A common feature of  list,  tuple,  str, and all sequence types in Python is the support
of slicing operations, which are more powerful than most people realize.

### Why Slices and Range Exclude the Last Item
略
### Slice Objects
略

### Multidimensional Slicing and Ellipsis
略, 主講 numpy 的應用

### Assigning to Slices
略

## Using + and * with Sequences
> Both  + and  * always create a new object, and never change their operands.

### Building Lists of Lists
``` python
# 實驗1:
>>> board = [['_'] * 3 for i in range(3)]  
>>> board
[['_', '_', '_'], ['_', '_', '_'], ['_', '_', '_']]
>>> board[1][2] = 'X'  
>>> board
[['_', '_', '_'], ['_', '_', 'X'], ['_', '_', '_']]

# 實驗2:
>>> weird_board = [['_'] * 3] * 3  
>>> weird_board
[['_', '_', '_'], ['_', '_', '_'], ['_', '_', '_']]
>>> weird_board[1][2] = 'O' 
>>> weird_board
[['_', '_', 'O'], ['_', '_', 'O'], ['_', '_', 'O']]
```

## Augmented Assignment with Sequences
### A += Assignment Puzzler
* 特別案例, 所以, 盡量避免在 tuple 裡放 mutable type
``` python
>>> t = (1, 2, [30, 40])
>>> t[2] += [50, 60]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
>>> t
(1, 2, [30, 40, 50, 60])
```

* 檢查 bytebode的流程
``` python
import dis
dis.dis('s[a] += b')
  1           0 LOAD_NAME                0 (s)
              3 LOAD_NAME                1 (a)
              6 DUP_TOP_TWO
              7 BINARY_SUBSCR                      
              8 LOAD_NAME                2 (b)
             11 INPLACE_ADD                        
             12 ROT_THREE
             13 STORE_SUBSCR                       
             14 LOAD_CONST               0 (None)
             17 RETURN_VALUE
```
### list.sort and the sorted Built-In Function
* list.sort() -> 原地排序
* sorted() -> 回傳新的物件

### Managing Ordered Sequences with bisect
## Searching with bisect
略
## Inserting with bisect.insort
略

### When a List Is Not the Answer
* array: 適合存放大量浮點數
* deque: 常常需要在頭尾做更動 as FIFO or LIFO

## Arrays
略

## Memory Views
要再花時間深究, 略

## NumPy and SciPy
略
## Deques and Other Queues
略





