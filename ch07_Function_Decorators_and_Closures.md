# 7. Function Decorators and Closures
> Function decorators let us “mark” functions in the source code to enhance their behavior
in some way. This is powerful stuff, but mastering it requires understanding closures.
> if you want to implement your own function decorators, you must know closures inside out, and then the need for   ```nonlocal ``` becomes obvious.

## Decorators 101
``` python
@decorate
def target():
    print('running target()')

# Has the same effect as writing this:
def target():
    print('running target()')
target = decorate(target)
```
* 執行完後拿到的 target 不一定是原來的 target 函數, 而是 decorate 返回的函數
``` python
>>> def deco(func):
...     def inner():
...         print('running inner()')
...     return inner  
...
>>> @deco
... def target():
...     print('running target()')
...
>>> target()  
# running inner()
>>> target  # Inspection reveals that target is a now a reference to inner.
# <function deco.<locals>.inner at 0x10063b598>
```
> Strictly speaking, decorators are just syntactic sugar. As we just saw, you can always
simply call a decorator like any regular callable, passing another function. Sometimes
that is actually convenient, especially when doing **metaprogramming**—changing pro‐
gram behavior at runtime.

> To summarize: the first crucial fact about decorators is that they have the power to
replace the decorated function with a different one. The second crucial fact is that they
are executed immediately when a module is loaded.

## When Python Executes Decorators
* Decorator 的特性是在被裝飾的函數**定義**之後會立即執行.

``` python
registry = []
def register(func):
    print('running register(%s)' % func)   
    registry.append(func)   
    return func

@register   
def f1():
    print('running f1()')

@register
def f2():
    print('running f2()')

def f3():
    print('running f3()')

def main():
    print('running main()')
    print('registry ->', registry)
    f1()
    f2()
    f3()


if __name__=='__main__':
    main()
# running register(<function f1 at 0x100631bf8>)
# running register(<function f2 at 0x100631c80>)
# running main()
# registry -> [<function f1 at 0x100631bf8>, <function f2 at 0x100631c80>]
# running f1()
# running f2()
# running f3()
```

## Decorator-Enhanced Strategy Pattern
延伸前章節的code, 略

## Variable Scope Rules
global在function裡的應用, 略

## Closures
``` python
# functional implementation
class Averager():
    def __init__(self):
        self.series = []
    def __call__(self, new_value):
        self.series.append(new_value)
        total = sum(self.series)
        return total/len(self.series)
>>> avg = Averager()
>>> avg(10)
10.0
>>> avg(11)
10.5
>>> avg(12)
11.0

# higher-order function
def make_averager():
    series = []
    def averager(new_value):
        series.append(new_value)
        total = sum(series)
        return total/len(series)
    return averager
>>> avg = make_averager()
>>> avg(10)
10.0
>>> avg(11)
10.5
>>> avg(12)
11.0
```
...
## The nonlocal Declaration
## Implementing a Simple Decorator
### How It Works
## Decorators in the Standard Library
### Memoization with functools.lru_cache
### Generic Functions with Single Dispatch
## Stacked Decorators
## Parameterized Decorators
### A Parameterized Registration Decorator
### The Parameterized Clock Decorator
## Chapter Summary
## Further Reading

