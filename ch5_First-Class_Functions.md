# 5. First-class functions (一級函數)
一級物件的定義:
* created at runtime;
* assigned to a variable or element in a data structure;
* passed as an argument to a function;
* returned as the result of a function.

## Treating a function like an object
>  the function object itself is an instance of the  function class

## Higher-order functions (高階函數)
* 可以把function視為輸入參數, 或是可以回傳function的function可以視為高階函數
> A function that takes a function as argument or returns a function as result is a higher-order function

* examples: map(), filter(), reduce(), sorted()
* apply() was removed in Python3

``` python
>>> def factorial(n):  
...     '''returns n!'''
...     return 1 if n < 2 else n * factorial(n-1)
>>> map(factorial, range(11))
<map object at 0x...>
>>> list(map(fact, range(11)))
[1, 1, 2, 6, 24, 120, 720, 5040, 40320, 362880, 3628800]
```
``` python
>>> def reverse(word):
...     return word[::-1]
>>> reverse('testing')
'gnitset'
>>> sorted(fruits, key=reverse)
['banana', 'apple', 'fig', 'raspberry', 'strawberry', 'cherry']
```

## Modern replacements for map, filter and reduce
* map, filter 會回傳 generator, 所以用 genexp可以直接替代很多工作, 又能提高可讀性
* 後面章節會細談 reduce

## Anonymous functions
略

## The seven flavors of callable objects
> The Python Data Model documentation lists seven callable types:
1. User-defined functions: len, time.strftime
2. Built-in functions: dict.get
3. Built-in methods
4. Methods
5. Classes
當類別被呼叫時, 會執行它的\_\_new\_\_, 再執行\_\_init\_\_把他初始化, 最後把instance回傳給呼叫方. 
6. Class instances
如果類別有定義\_\_call\_\_方法, 那他的instance就可以被呼叫
7. Generator functions
使用 yield keyword, 當被呼叫會回傳一個 generator object

``` python
# the safest way to  determine  whether  an  object  is  callable  is  to  use  the  callable() built-in:
>>> abs, str, 13
(<built-in function abs>, <class 'str'>, 13)
>>> [callable(obj) for obj in (abs, str, 13)]
[True, True, False]
```

## User defined callable types
``` python
def __init__(self, items):
    self._items = list(items)   
    random.shuffle(self._items)   
def pick(self):   
    try:
        return self._items.pop()
    except IndexError:
        raise LookupError('pick from empty BingoCage')   
def __call__(self):
    return self.pick()
```

## Function introspection (函式自我檢查)
* 在 function 裡有的 attributes 在 user-defined object 不見得都有 
``` python
>>> class C:
        pass
>>> obj = C()
>>> def func():
        pass
>>> sorted(set(dir(func)) - set(dir(obj)))
['__annotations__', '__call__', '__closure__', '__code__', '__defaults__',
'__get__', '__globals__', '__kwdefaults__', '__name__', '__qualname__']
```
## From positional to keyword-only parameters
太雜了, 略

## Retrieving information about parameters
* 利用 inspect 取代內建特殊方法來取參數值, 需要再看, 略

## Function Annotations
* 函式註解會存在__annotations__, 但Python不會主動檢查, 驗證
* 需要手動檢查(inspect.signature())或是提供給IDE檢查

> The  only  thing  Python  does  with  annotations  is  to  store  them  in  the  **\_\_annota
tions\_\_** attribute of the function. Nothing else: no checks, enforcement, validation, or
any other action is performed. In other words, annotations have no meaning to the
Python interpreter. They are just metadata that may be used by tools, such as **IDEs**,
**frameworks**, and **decorators**. 

``` python
def clip(text:str, max_len:'int > 0'=80) -> str:
    pass
clip.__annotations__
# {'text': <class 'str'>, 'max_len': 'int > 0', 'return': <class 'str'>}
```

## Packages for Functional Programming
* operator
* functools

### The operator Module
* 儘管前面章節提到reduce可以被內建operator(e.g. sum())取代, 但仍有許多情境需要用到(e.g. mul)
``` python
from functools import reduce
def fact(n):
    return reduce(lambda a, b: a*b, range(1, n+1))
```
*   operator module 提供 functions 可以不必寫 anonymous finctions
``` python
from functools import reduce
from operator import mul
def fact(n):
    return reduce(mul, range(1, n+1))
```
* operator.itemgetter, operator.attrgetter 應用在排序上
``` python
>>> from operator import itemgetter
>>> for city in sorted(metro_data, key=itemgetter(1)):
...     print(city)
```
> itemgetter supports not only sequences but also mappings and any class that implements  \_\_getitem\_\_.
* operator.attrgetter, 範例看不太懂, 略
* operator.methodcaller, 範例看不太懂, 略

### Freezing Arguments with functools.partial
* 可以把function的多個參數綁住, 使用者只需要輸入部分參數就好
> This is useful to adapt a function that takes
one  or  more  arguments  to  an  API  that  requires  a  callback  with  fewer  arguments.
``` python
>>> from operator import mul
>>> from functools import partial
>>> triple = partial(mul, 3)
>>> triple(7)  
21
>>> list(map(triple, range(1, 10)))  
[3, 6, 9, 12, 15, 18, 21, 24, 27]
```
``` python
>>> import unicodedata, functools
>>> nfc = functools.partial(unicodedata.normalize, 'NFC')
>>> s1 = 'café'
>>> s2 = 'cafe\u0301'
>>> s1, s2
('café', 'café')
>>> s1 == s2
False
>>> nfc(s1) == nfc(s2)
True
```
## Chapter Summary
略

