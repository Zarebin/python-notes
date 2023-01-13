# Asynchronous Functions and await  

Python provides a number of language features related to the **asynchronous** execution of code.  
An asynchronous function, or coroutine function, is defined by prefacing
a normal function definition with the extra keyword *async*. *For example*:  

```python
async def greeting(name):
    print(f'Hello {name}')
```

To make the function run, it must execute under the supervision of other code. A common option is asyncio. *For example:*  

```python
>>> import asyncio
>>> asyncio.run(greeting('Guido'))
Hello Guido
```
### Note:  
Async functions can call other async functions using an *await* expression
like this:  

```python
async def make_greeting(name):
    return f'Hello {name}'
async def main():
    for name in ['Paula', 'Thomas', 'Lewis']:
        a = await make_greeting(name)
        print(a)

# Run it. Will see greetings for Paula, Thomas, and Lewis
asyncio.run(main())
```

Combining async and non-async functionality in the same application is
a complex topic, especially if you consider some of the programming
techniques involving higher-order functions, callbacks, and decorators. In
most cases, support for asynchronous functions has to be built as a special
case.
Python does precisely this for the iterator and context manager protocols.
*For example*, an asynchronous context manager can be defined using
__aenter__() and __aexit__() methods on a class like this:  

```python
class AsyncManager(object):
    def __init__(self, x):
        self.x = x
    async def yow(self):
        pass
    async def __aenter__(self):
        return self
    async def __aexit__(self, ty, val, tb):
        pass
```

# Generators  

Generator functions are one of Python’s most interesting and powerful
features. Generators are often presented as a convenient way to define new
kinds of iteration patterns.  


## Generators and yield  

If a function uses the yield keyword, it defines an object known as a
generator. The primary use of a generator is to produce values for use in
iteration. Here’s an *example:*  

```python
def countdown(n):
    print('Counting down from', n)
    while n > 0:
        yield n
        n -= 1


# Example use
for x in countdown(10):
print('T-minus', x)
```

### Note:  
A generator function produces items until it returns—by reaching the end
of the function or by using a return statement. This raises a *StopIteration*
exception that terminates a for loop. If a generator function returns a non-
None value, it is attached to the *StopIteration* exception. *For example*, this
generator function uses both **yield** and **return**:  

```python
def func():
    yield 37
    return 42

>>> f = func()
>>> f
<generator object func at 0x10b7cd480>
>>> next(f)
37
>>> next(f)
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
StopIteration: 42
>>>
```













