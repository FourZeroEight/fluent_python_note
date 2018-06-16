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




