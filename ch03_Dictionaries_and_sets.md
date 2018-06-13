# 3. Dictionaries and sets

## Generic Mapping Types
### 要再花時間了解之間的繼承關係 ???
> The collections.abc module provides the Mapping and MutableMapping ABCs to
formalize the interfaces of  dict and similar types

* 有用到 dict 的 mapping types, 都必須遵守 key 必須是 hashable 的限制.
> All mapping types in the standard library use the basic dict in their implementation,
so they share the limitation that the keys must be hashable (the values need not be
hashable, only the keys).

## What is hashable?
> An object is hashable if it has a hash value which never changes during its lifetime (it
needs a  __hash__()  method), and can be compared to other objects (it needs an
__eq__()  method). Hashable objects which compare equal must have the same hash
value. […]
* tuple 裡面如果存有mutable type, 就不能算是 hashable
> The atomic immutable types ( str ,  bytes , numeric types) are all hashable. A frozen
set is always hashable, because its elements must be hashable by definition. A  **tuple** is
hashable only if all its items are hashable.

* 自定義的 type 也是 hashable
> **User-defined types** are hashable by default because their hash value is their  id()  and
they all compare not equal. If an object implements a custom  __eq__  that takes into
account its internal state, it may be hashable only if all its attributes are immutable.

## dict Comprehensions
略

## Overview of Common Mapping Methods
1. `d.get(k, [default])`
2. `d.__getitem__(k)`: 等於 `d[k]` operator
3. `d.__missing__(k)`: 當 `__getitem__`找不到key值, 會呼叫此method
4. `d.setdefault(k, [default])`
4. `d.__setitem__(k, v)`: 等於`d[k] = v` operator
4. `d.default_factory`: Callable invoked by `__missing__` to set missing values

## Handling Missing Keys with setdefault
``` Python
"""Build an index mapping word -> list of occurrences"""
# 1. 直覺的寫法, 每次迴圈至少搜尋2次, 如果key找不到, 就要搜尋3次
if key not in my_dict:
    my_dict[key] = []
my_dict[key].append(new_value)

# 2. 一樣的結果, 但只需要一行, 每次只需搜尋1次
my_dict.setdefault(key, []).append(new_value)
```

## Mappings with Flexible Key Lookup
- 方法1: defaultdict
- 方法2: 繼承 dict 或 其他的 mapping type, 改寫 `__missing__` method

##  還不太能理解 default_factory 如何解釋 ??
> The  **default_factory** of a defaultdict is only invoked to providedefault values for
`__getitem__` calls, and not for the other methods. For example, if  dd  is a  defaultdict,
and  k  is a missing key,  dd[k]  will call the  default_factory  to create a default value,
but dd.get(k)  still returns  None .

## The `__missing__` Method
``` python
"""方法2"""
class StrKeyDict0(dict):   
    def __missing__(self, key):
        if isinstance(key, str):   
            raise KeyError(key)
        return self[str(key)]   
    def get(self, key, default=None):
        try:
            return self[key]   
        except KeyError:
            return default   
    def __contains__(self, key):
        return key in self.keys() or str(key) in self.keys()
```

## Variations of dict
略

## Subclassing UserDict
* 如果要override內建dict的method, 避免直接繼承, 而是要用UserDict實作並修改
> The main reason why it’s preferable to subclass from UserDict than dict is that the
built-in has some implementation shortcuts that end up forcing us to override methods
that we can just inherit from  UserDict with no problems

* 原因寫在第12章: **Subclassing built-in types is tricky**
> Subclassing built-in types like  dict  or  list  or  str  directly is errorprone
because  the  built-in  methods  mostly  ignore  user-defined overrides.
Instead of subclassing the built-ins, derive your classes from UserDict,
UserList  and  UserString  from the  collections module, which are designed to be easily extended.

``` python
import collections
class StrKeyDict(collections.UserDict):
    def __missing__(self, key):
        if isinstance(key, str):
            raise KeyError(key)
        return self[str(key)]
    def __contains__(self, key):
        return str(key) in self.data
    def __setitem__(self, key, item):
        self.data[str(key)] = item
```

## 不懂這段??
> Because  UserDict  subclasses  MutableMapping,  the  remaining  methods  that  make
StrKeyDict a full-fledged mapping are inherited from UserDict, MutableMapping, or
Mapping. The latter have several useful concrete methods, in spite of being abstract base
classes (ABCs).

## Immutable Mappings
``` python
from types import MappingProxyType
```
略

## Set Theory
* '集合' 並非 hashable, 但是 '集合' 裡的 '元素' 必須是 hashable !
*  The  'set' type is not hashable, but  'frozenset' is.

> Set elements must be hashable. The  set type is not hashable, but  frozenset is, so you
can have  frozenset elements inside a  set.

## set Literals
* 建 '集合' 用 {1, 2, 3} 比 set([1, 2, 3]) 有效率, 可讀性也高 
``` python
"""
Literal set syntax like {1, 2, 3} is both faster and more readable than calling the
constructor (e.g., set([1, 2, 3])).
"""
>>> from dis import dis
>>> dis('{1}')                                   
  1           0 LOAD_CONST               0 (1)
              3 BUILD_SET                1       
              6 RETURN_VALUE
>>> dis('set([1])')                              
  1           0 LOAD_NAME                0 (set) 
              3 LOAD_CONST               0 (1)
              6 BUILD_LIST               1
              9 CALL_FUNCTION            1 (1 positional, 0 keyword pair)
             12 RETURN_VALUE
```

## Set Comprehensions
``` python
>>> from unicodedata import name  
>>> {chr(i) for i in range(32, 256) if 'SIGN' in name(chr(i),'')}  
{'§', '=', '¢', '#', '¤', '<', '¥', 'µ', '×', '$', '¶', '£', '©',
'°', '+', '÷', '±', '>', '¬', '®', '%'}
```

## Set Operations
略

## dict and set Under the Hood
* How efficient are Python  dict and  set?
* Why are they unordered?
* Why can’t we use any Python object as a  dict key or  set element?
* Why does the order of the dict keys or set elements depend on insertion order,
and may change during the lifetime of the structure?
* Why is it bad to add items to a  dict or  set while iterating through it?

## A Performance Experiment
實驗 略

## Hash Tables in Dictionaries

### Hashes and equality
> If two objects compare equal, their hash values must
also be equal, otherwise the hash table algorithm does not work. For example, because
1 == 1.0 is true, hash(1) == hash(1.0) must also be true, even though the internal
representation of an  int and a  float are very different.

### The hash table algorithm
略, 查表流程之後需要再花時間看

## Practical Consequences of How dict Works
### Keys must be hashable objects
1. 必須要有 `__hash__`, `__eq__`
2. If  a == b is  True then  hash(a) == hash(b) must also be  True.

### dicts have significant memory overhead
* 存大量的紀錄, 用 list of tuples 或 named tuples 比 list of dicts 還省空間
* 移除每筆記錄的 hash table占用的空間
* 不需要每個dict都存一樣的欄位名稱

### Key search is very fast
看實驗數據, 略

### Key ordering depends on insertion order
當增加新的元素給dict, 直譯器會判斷 hash table有加大的需求, 而搬動整個 table 到新的table,
導致dict內的keys排序**可能**會有變化.

所以, 在iteration過程中, 不建議直接更動dict內容.
解決方法: 在iteration過程中, 將需要更新的值存在另個 dict, 最後再 update 原本的 dict

## How Sets Work—Practical Consequences
* Set elements must be hashable objects.
* Sets have a significant memory overhead.
* Membership testing is very efficient.
* Element ordering depends on insertion order.
* Adding elements to a set may change the order of other elements.

## Chapter Summary
略
