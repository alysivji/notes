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

* [`operator` module](https://docs.python.org/3/library/operator.html) provides the functionality of all Python infix operators in functional form to reduce the use of anonymous `lambda` functions
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

### Chapter 8: Object References, Mutability, and Recycling

* every Python object has an address (`id`), a type, and a value

* variables are labels attached to objects
* variables are assigned to objects, objects are not assigned to variables
* an object can have several variables that reference it
* `==` compares values of objects (data they hold)
    * define custom `__eq__` method to specify how your objects deal with the `==` operator
* `is` compares identity (do they refer to the same object?)
    * used to compare value to singleton. i.e. `x is None`

* think about how to copy deeply nested objects, do we copy everything?
    * by default, copies are shallow
* [`copy`](https://docs.python.org/3/library/copy.html) module has `copy.copy()` and `copy.deepcopy()` which implement the `__copy__` and `__deepcopy__` protocols

* function parameters are passed by value, but the values are references

---

### Chapter 9: A Pythonic Object

(from [Michael Foord](http://www.voidspace.org.uk/python/articles/duck_typing.shtml))
* Python is strongly typed because object types __don't change type__
    * if the language rarely performs implicit conversions of types, it is strongly typed
* Python is dynamically typed because we can pass references around and we don't check type until the last possible minute
    * type checking performed at runtime means dynamically typed
* duck typing - it doesn't matter what type my data is, just that it can do what I want with it
    * i.e. if it has a quack method, it doesn't matter it's a duck. We can treat it like a duck.
    * this is a consequence of the object defining what it can or can't do
    * "The principle of duck typing says that you shouldn't care what type of object you have - just whether or not you can do the required action with your object"

* user-dfined types can implement the methods needed for objects to behave as expected

* `__repr__` returns object representation developer wants to see
* `__str__` returns object user wants to see

* `@classmethod` receives the class as the first argument and is used to create alternative constructor

```python
@classmethod
def from_date_string(cls, *args, **kwargs):
    return cls(*args, **kwargs)
```

* `@staticmethod` receives no special first argument and is keep functions that are related to the class within the class
* [further reading](https://julien.danjou.info/blog/2013/guide-python-static-class-abstract-methods) on static and class methods

* `__format__(formatspec)` can take in [format specification](https://docs.python.org/3/library/string.html#formatspec)
    * we can create custom format specifications, but make sure to not overlap current syntax or we might confuse users

* implement `__hash__` to make method hashable ([more information](https://docs.python.org/3/reference/datamodel.html#object.__hash__))
    * might have to make instances immutable which we can do using the `@property` decorator on attributes (making them read-only)

* class attributes and methods that start with `__` (double underscore) get name mangled, i.e. `_Class.__method`

#### `__slots__`

* Python stores instance attributes in each instance's `__dict__`
* This takes a lot of memory so we can use the `__slots__` class attribute to store the instance attributes in a tuple instead of a dict
* if we add `__dict__` to slots, we can also support dynamically created attributes which will be stored in `__dict__`, but use this feature with caution
* useful when we have millions of instances (not just thousands)

#### Overriding Class Attribute

```python
class Foo(object):
    my_attribute = 'value'

    def bar(self, x):
        self.my_attribute
        # this is slower because it has to go to instance and then class because this is not in the instance
        # BUT we can override Foo just to write a different class attribute

class SpecializedFoo(Foo):
    my_attribute = 'value2'
    # that's it, that's all that is needed
```

---

### Chapter 10: Sequence Hacking, Hashing, and Slicing

* sequence protocol: `__len__` & `__getitem__`
* [`reprlib`](https://docs.python.org/3/library/reprlib.html) module can be used to produce safe representations of large or recursive structures by limiting the length of the output string (i.e. not showing all elements of a vector)

#### Protocol

* in OOP, a **protocol** is an informal interface that is defined in documentation but not in code
* if a object implements certain dunder methods, it can support a protocol which means that we can use that object wherever an object following that protocol is required (aka *duck typing*)

* Just need to implement `__getitem__` to support iteration

#### Slices

* slices have an `indices` method that produces a 'normalized' tuple of nonnegative `start`, `stop`, and `stride` integers adjusted to fit the bounds of a sequence of a given length

#### Dynamic Attribute Access

* `__getattr__` is invoked by the interpreter whenever the attribute lookup fails
* This can cause inconsistencies if we set an attribute on the instance, but expect it to fail back to the `__getattr__` method
* very often we will implement `__setattr__` along with `__getattr__` to keep object behavior consistent

#### Hashing

* Not really a protocol, but implementing `__hash__` and `__eq__` make our instances hashable

---

### Chapter 11: Interfaces: From Protocols to ABCs

* Languages with dynamic typing use **protocols** to define informal interfaces
* An **interface** is the subset of methods that enable it to play a specific role in the system
* A class may implement several protocols which allows instaces to fulfill several roles
* Protocols can be partially implemented as required

* Python will fall back to using `__getitem__` to implement the behavior of a sequence if `__iter__` and `__contains__` are not defined
    * starts at 0 and goes up by integer indices
* mutable sequences implement the `__setitem__` method

* protocols are related to the explicit interfaces that are documented in abstract base classes (ABCs)

* **monkeypatching** refers to changing a class or module at runtime without touching the source code

#### Abstract Base Classes (ABC)

* goosetyping: `isinstance(obj, cls)` with `cls` being an abstract base class
* whenever you are implementing a class embodying any of the concepts represented in the ABCs, be sure to subclass from it or register it into the corresponding ABC
* excessive `isinstance` checks are a symptom of bad OO design unless we are explicitly checking against an ABC
* ABCs are useful for plugin architectures

* if we subclass an ABC, we have to implement required methods

* [`collections.abc`](https://docs.python.org/3/library/collections.abc.html)
* [`numbers` base class](https://docs.python.org/3/library/numbers.html)
* [`abc` - Abstract Base Class](https://docs.python.org/3/library/abc.html)
    * use to define custom abstract base classes (ABCs)

We would define a custom ABC when we have classes that implement methods with the same name which represent different things, i.e. `artist.draw()` vs `gunslinger.draw()`

Note: Re-read this chapter if I ever have to do any work with custom ABCs

* [`@abstractmethod`](https://docs.python.org/3/library/abc.html#abc.abstractmethod) can be use to mark a method in an ABC that needs to be implemented by the class inheriting the custom ABC (i.e. the *concrete class*)
    * can stack `@property`, `@staticmethod`, `@classmethod` decorators on top
* We can have *concrete methods* in the interface as long as they depend on other methods in the interface
    * our *concrete subclasses* can overwrite these methods as needed (better implementation)

* we can register a class as a virtual subclass of ABC, even though it doesn't inherit from it
    * done using `@register` on the class

```python
from tombola import Tombola

@Tombola.register
class TomboList(list):
    pass
```

#### Other Resources

* [PEP 3119 -- Introducing Abstract Base Classes](https://www.python.org/dev/peps/pep-3119/)
* [PEP 3141 -- A Type Hierarchy for Numbers](https://www.python.org/dev/peps/pep-3141/)
