# Fluent Python

by Luciano Ramalho

## Part 1: Prologue

## Chapter 1: The Python Data Model

Python uses special (`__dunder__`) methods which can help make our custom created objects behave like built-in objects.

We can use Python's Data Model to design "Pythonic" APIs for our classes and libraries.

### Further Reading

* [Python Data Model](https://docs.python.org/3/reference/datamodel.html)

---
---

## Part 2: Data Structures

### Chapter 2: An Array of Sequences

__sequence__ - ordered set of objects; all sequences support iteration
* use `collections.MutableSequence` as the base class

#### List Comprehensions

* build lists from sequences or other iterable types by filtering and transforming items
* great for generating cartesian product of 2+ iterables

* easy to read
* make sure you are actually doing something with the list otherwise this is a waste
* have their own local scope (in Python 3, aka the only Python)

#### Generator Expressions

* Save memory as it yields items one by one using an iterator protocol instead of feeding the whole list to another constructor

#### Tuples

* can be used to hold records, i.e. each item in the tuple holds data for one field and its position has meaning
* can also be used as an immutable `list`; supports a lot of the same methods (except `__reversed__`)
* unpacking syntax: `first_item, second_item = tuple(1, 2)`
* Python 3 adds the * operator to pull excess items when performing [iterable unpacking](https://www.python.org/dev/peps/pep-3132/)
* nested data structures can also be unpacked

#### NamedTuples

* `collections.namedtuple`
* factory function that produces a subclass of `tuple` enhanced with a class name & field names

#### Slicing

* all sequences support slicing
* `[start:stop:step]` produces a `slice(start, stop, step)` object which we can treat like any other object
* `__getitem__` and `__setitem__` can be used to deal with the [] operators
* we can do actions to the mutable sequences (replace, extract, etc) using the slice notation

#### Augmented Assignments (`+=`, `*=`)

* uses `__iadd__` and falls back to `__add__`

#### Sorting

* `sorted()` tkes in `key` that can be used to a

#### Other

* **`array`** - use for lists of numbers; as lean as C
* **`memoryviews`**
* **`deque`** - double-ended queue where we can append / remove from both sides
* **`queue`** - regular queue
* **`asyncio`** - asynchronous programming queues
* **`heapq`** - heap-based queue

---

### Chapter 3: Dictionaries and Sets

#### Dictionaries

* hash tables power Python dict
* tuples are only hashable if all its itemss are hashable
    * tuples with lists are not hashable

* dictionary comprehensions build `dict` instances from any iterable

* `my_dic.setdefault(key, []).append(value)` is a quick way to append a `value` to a dict of `key: list` pairs
* can also use a `defaultdict` to create items on demand whenever a key is missing    : `my_dict[key].append(value)`
* `__mising__` is called when key is not found instead of raising a `KeyError`
    * cutomize what happens when a key is not found
    * use to design intuitive APIs

* `collections.OrderedDict` maintains keys in insertion order, allowing iteration over items in predictable order
* `collections.ChainMap` holds a list of mappings that can be searched as one, successful if key is found in any one of them
* `collections.Counter` holds an integer count for each key
* `collections.UserDict` mapping we should sub-class; `dict` has a lot of implementation shortcuts that we have to override
* `types.MappingProxyType` can be used to create an immutable mapping

#### Sets

* collection of unique objects
* empty set: `set()`
* syntax: `foo = {1, 2, 3}` also `foo = set([1, 2, 3])` (but this is slower)

* `frozenset` is immutable
* syntax: `foo = frozenset(range(10))`

* set comprehensions are a thing

#### Hashing

* Python uses a hash table behind the scenes for dictionaries (and sets)
* dict keys need to be hashable objects
* dicts have significant memory overhead
* key searches are fast

#### Further Reading

* [`collections`](https://docs.python.org/3/library/collections.html)
* [Raymond Hettinger - Modern Dictionaries](https://www.youtube.com/watch?v=npw4s1QTmPg) (PyCon 2017)
* [Brandon Rhodes - The Mighty Dictionary](https://www.youtube.com/watch?v=oMyy4Sm0uBs) (PyCon 2016)
* [Brandon Rhodes - The Dictionary Even Mighter](https://www.youtube.com/watch?v=66P5FMkWoVU) (PyCon 2017)

---

### Chapter 4: Text versus Bytes

What is a character? In Python 3, a __character__ is a unicode character.

Characters can be stored as __bytes__. Bytes are stored with a specific _encoding_, we _decode_ bytes to get human readable text.

* [`memoryview`](https://docs.python.org/3/c-api/memoryview.html)
    * [more](http://python-reference.readthedocs.io/en/latest/docs/memoryview/)
* [`struct`](https://docs.python.org/3/library/struct.html)

* when converting text to bytes, if a character is not defined in the target encoding, will raise a `UnicodeEncodeError`
* when you assume an encoding while converting a binary sequence to text, unexpected bytes can trigger a `UnicodeDecodeError`

> The best practice for handling text is the "Unicode sandwich". This means that bytes should be decode to __str__ as early as possible on input. The "meat" of the sandiwch is the business logic of your program, where text handling is done exclusive on __str__ objects. You should never be encoding or decoding in the middle of other processing. On output, the __str__ are encoded to __bytes__ as late as possible.

* text comparisons are complicated because Unicode can have multiple ways of representing some characters
    * normalizing is a prerequesite to text matching

---
---

## Part 3: Functions as Objects

### Chapter 5: First-Class Functions

Python functions are "first-class objects"
* created at runtime
* assigned to a variable or element in a data structure
* passed as an argument to a function
* returned as the result of a function

* __higher order functions__ take a function as argument or returns a function as the result
* __anonymous functions__ are functions without a name, written using `lambda`

#### Seven Flavors of Callable Objects

1. User-defined functions - `def` or `lambda`
1. Built-in functions - function implemented in C
1. Built-in methods - methods implemented in C
1. Methods - functions in the body of a class
1. Classes - using as a function invokes `__new__` and `__init__`  then returns back to caller
1. Class instances - if class has `__call__`, its instance may be invoked as a function
1. Generator functions - use `yield` to return generator objects

#### Interesting Use Cases of Functions

* use `callable()` to determine if something is callable
* class implementing `__call__` is an easy way to create function-like object that can have internal state across invocations (i.e. memoization decorator)

* can assign attribute to `__dict__` of function
* pass iterables and mappings into function argument using `*args` and `**kwargs` respectively
* we can use the [`inspect` module](https://docs.python.org/3/library/inspect.html) to introspect functions
* functions support type annotations that we can access via `inspect` module, useful for IDEs and linters

#### Functional Programming

* [`operator` module](https://docs.python.org/3/library/operator.html)
* `functools.partials` is a way of freezing some of the keyword arguments to produce a simpler API

---

## Chapter 6: Design Patterns with First-Class Functions

Design patterns have intricies that make their implementation into various langauges slightly different. We can take advantage of functions being first-class objects and implement some of the patterns using functions instead of classes.

---

### Chapter 7: Function Decorators and Closures

#### Decorators

```python
def decorate(func):
    def inner():
        print('running inner')
    return inner()

# decorator
@decorate
def target():
    print('running target')

# is equivalent to
def target():
    print('running target')

target = decorate(target)
```

* Decorators have the power to replace decorated functions with different functions
* Executed immediately when a module is loaded, i.e. at import time
* Usually defined in one module and applied to functions in another module
* Most decorators define an inner function and return it

#### Closures

* Python does not require you to declare variables but assumes that a variable assigned in the body of a function is local

* __closure__ - a function that retains the bindings of the free variable that exist when the function is defined so it can be used later when the function is invoked and the defining scope is no longer available

* __free variable__ - variable not bound to local scope
* flag a free variable when it is assigned to a new value using `nonlocal`

#### Decorators in Depth

* When you decorate a function, it's signature and reference refers to the decorator. Use `functools.wraps` to copy the relevant attributes so the original function looks as we expect it to

* [`functools.lru_cache`](https://docs.python.org/3/library/functools.html#functools.lru_cache) (Least Recntly Used) is a memoization decorator that can save results of previous invocations of an expensive function
    * `maxsize` sould be a power of 2

* [`functools.singledispatch`](https://docs.python.org/3/library/functools.html#functools.singledispatch) was added via [PEP443](https://www.python.org/dev/peps/pep-0443/) allows us to define a generic function that allows us to have the same operation for different types

* Decorators with parameters requires use to use decorator factories that takes arguments and returns a decorator that is then applied to the function we are decorating
    * every problem in computer since can be solved by another layer of abstraction

```python
def decorator_factory(param1='a', param2=True):
    def decorator(func):  # the decorator we are returning
        def new_func(*args, **kwargs):  # args and kwargs of fn we are decorating
            result = func(args, kwargs)
            return do_something_else(result)
        return new_func
    return decorator
```

---
---

## Part IV: Object-Oriented Idioms

### Chapter 8: Object References, Mutability, and Recyling

* blah
