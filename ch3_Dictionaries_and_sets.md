# 3. Dictionaries and sets

## Generic Mapping Types
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
2. `d.__getitem__(k)`: 等於 `d[k]`
3. `d.__missing__(k)`: 當 `__getitem__`找不到key值, 會呼叫此method
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

方法1: defaultdict
方法2: 繼承 dict 或 其他的 mapping type, 改寫 `__missing__` method

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



