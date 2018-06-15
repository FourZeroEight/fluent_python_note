# 5. First-class functions (一階函數)

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


