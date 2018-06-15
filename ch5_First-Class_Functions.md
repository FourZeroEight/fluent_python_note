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
* listcomp, genexp 可以取代 map, filter 的工作
* 用sum()等方法可以取代 reduce

## Anonymous functions
略

## The seven flavors of callable objects
1. User-defined functions
2. Built-in functions
3. Built-in methods
4. Methods
5. Classes
6. Class instances
7. Generator functions

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

## Function 
__call__
...

## 



