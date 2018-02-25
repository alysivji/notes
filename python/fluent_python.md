# Fluent Python

by Luciano Ramalho

<!-- TOC -->

- [Part 1: Prologue](#part-1-prologue)
    - [Chapter 1: The Python Data Model](#chapter-1-the-python-data-model)
- [Part 2: Data Structures](#part-2-data-structures)
    - [Chapter 2: An Array of Sequences](#chapter-2-an-array-of-sequences)
    - [Chapter 3: Dictionaries and Sets](#chapter-3-dictionaries-and-sets)
    - [Chapter 4: Text versus Bytes](#chapter-4-text-versus-bytes)
- [Part 3: Functions as Objects](#part-3-functions-as-objects)
    - [Chapter 5: First-Class Functions](#chapter-5-first-class-functions)
    - [Chapter 6: Design Patterns with First-Class Functions](#chapter-6-design-patterns-with-first-class-functions)
    - [Chapter 7: Function Decorators and Closures](#chapter-7-function-decorators-and-closures)
- [Part IV: Object-Oriented Idioms](#part-iv-object-oriented-idioms)
    - [Chapter 8: Object References, Mutability, and Recycling](#chapter-8-object-references-mutability-and-recycling)
    - [Chapter 9: A Pythonic Object](#chapter-9-a-pythonic-object)
    - [Chapter 10: Sequence Hacking, Hashing, and Slicing](#chapter-10-sequence-hacking-hashing-and-slicing)
    - [Chapter 11: Interfaces: From Protocols to ABCs](#chapter-11-interfaces-from-protocols-to-abcs)
    - [Chapter 12: Inheritance: For Good or For Worse](#chapter-12-inheritance-for-good-or-for-worse)
    - [Chapter 13: Operator Overloading: Doing It Right](#chapter-13-operator-overloading-doing-it-right)
- [Part V: Control Flow](#part-v-control-flow)
    - [Chapter 14: Iterables, Iterators, and Generators](#chapter-14-iterables-iterators-and-generators)
    - [Chapter 15: Context Managers and `else` Blocks](#chapter-15-context-managers-and-else-blocks)
    - [Chapter 16: Coroutines](#chapter-16-coroutines)
    - [Chapter 17: Concurrency with Futures](#chapter-17-concurrency-with-futures)
    - [Chapter 18: Concurrency with `asyncio`](#chapter-18-concurrency-with-asyncio)
- [Part 4: Metaprogramming](#part-4-metaprogramming)
    - [Chapter 19: Dynamic Attributes and Properties](#chapter-19-dynamic-attributes-and-properties)
- [Todo](#todo)

<!-- /TOC -->

---
---

## Part 1: Prologue

### Chapter 1: The Python Data Model

Python uses special (`__dunder__`) methods which can help make our custom created objects behave like built-in objects.

We can use Python's Data Model to design "Pythonic" APIs for our classes and libraries.

#### Further Reading

* [Python Data Model](https://docs.python.org/3/reference/datamodel.html)

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

### Chapter 6: Design Patterns with First-Class Functions

Design patterns have intricies that make their implementation into various langauges slightly different. We can take advantage of functions being first-class objects and implement some of the patterns using functions instead of classes.

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

#### Decocrator with Arguments

```python
def decorator_factory(param1='a', param2=True):
    def decorator(func):  # the decorator we are returning
        def new_func(*args, **kwargs):  # args and kwargs of fn we are decorating
            result = func(args, kwargs)
            return do_something_else(result)
        return new_func
    return decorator
```

[Example from StackOverflow](https://stackoverflow.com/questions/5929107/decorators-with-parameters):

```python
def decorator(argument):
    def real_decorator(function):
        def wrapper(*args, **kwargs):
            funny_stuff()
            something_with_argument(argument)
            function(*args, **kwargs)
            more_funny_stuff()
        return wrapper
    return real_decorator
```

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
    * ABCs are interface declarations that may optionally provide concrete method implementations

We would define a custom ABC when we have classes that implement methods with the same name which represent different things, i.e. `artist.draw()` vs `gunslinger.draw()`

Note: Re-read this chapter if I ever have to do any work with custom ABCs

* [`@abstractmethod`](https://docs.python.org/3/library/abc.html#abc.abstractmethod) can be use to mark a method in an ABC that needs to be implemented by the class inheriting the custom ABC (i.e. the *concrete class*)
    * can stack `@property`, `@staticmethod`, `@classmethod` decorators on top
* We can have *concrete methods* in the interface as long as they depend on other methods in the interface
    * our *concrete subclasses* can overwrite these methods as needed (better implementation)
    * concrete methods implemented in an ABC should only collaborate with methods of the same ABC and its superclasses

* we can register a class as a virtual subclass of ABC, even though it doesn't inherit from it
    * done using `@register` on the class

```python
from tombola import Tombola

@Tombola.register
class TomboList(list):
    pass
```

* **Polymorphism** is the ability to leverage the same interface for different underlying forms such as data types or classes. This permits functions to use entities of different types at different times ([source](https://www.digitalocean.com/community/tutorials/how-to-apply-polymorphism-to-classes-in-python-3))

#### Other Resources

* [PEP 3119 -- Introducing Abstract Base Classes](https://www.python.org/dev/peps/pep-3119/)
* [PEP 3141 -- A Type Hierarchy for Numbers](https://www.python.org/dev/peps/pep-3141/)

### Chapter 12: Inheritance: For Good or For Worse

* subclassing built-in types is error prone because the built-in methods mostly ignore user-defined overrides; to do it properly we need to subclass from the [`collections`](https://docs.python.org/3/library/collections.html) module
    * The reason for this is because we want the built-in types to be fast so they are optimized via CPython

#### Method Resolution Order (MRO)

* Multiple inheritance can result in ["the dimaond problem"](https://en.wikipedia.org/wiki/Multiple_inheritance#The_diamond_problem) where we have to deal with potential naming conflicts due to unrelated ancestor classes implementing a method by the same name

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/8/8e/Diamond_inheritance.svg/440px-Diamond_inheritance.svg.png" />

* Python follows a specific order, **method resolution order**, when traversing the inheritance graph
    * takes into account the inheritance graph as well as the order in which superclasses are listed in subclass declaration
    * encoded in the `__mro__` attribute

* The recommended way of delgating calls to superclasses is the `super()` uiltin funtion, but we can also bypass the MRO and invoke a method on the superclass directly
    * [Raymond Hettinger on `super()`](https://rhettinger.wordpress.com/2011/05/26/super-considered-super/)

```python
x = A()
x.method()

# is the same ass
x = A()
A.method(x)
```

#### Strategies of Coping with Multiple Inheritance

> "Inheritance is used for different reasons, and multiple inheritance adds alternatives and complexity" - Alan Kay

1. Distinguish Interface Inheritance from Implemtation Inheritance
    * Inheritance of interface creates a subtype, "is-a"
    * Inheritance of implementation avoids code duplication by reuse
    * Inheritance for code reuse is an implemntation detail and it can be replaced by composition and delegation
    * Interface inheritance is the backbone of a framework

1. Make Interfaces Explicit with ABCs

1. Use Mixins for Code Reuse
    * If a class is designed to provide method implementations for reuse by multiple unrelated subclasses without implying an "is-a" relationships, it should be an explicit **mixin** class
    * Mixins do not define a new type, they merely bundle methods for reuse
    * Each mixin should provide a single specific behavior, implementing a few *very* closely related methods

1. Make Mixins Explicit by Naming them (`...Mixin`)

1. An ABC may also be a Mixin; the Reverse is not True

1. Don't Subclass from More Than One Concrete Class
    * [more info](http://www.cems.uwe.ac.uk/~jsa/UMLJavaShortCourse09/CGOutput/Unit9/unit9%280809%29/page_03.htm)

1. Provide Aggregate Classes to Users
    * Give users classes that are a combination of ABCs and/or mixins

1. Favor Object Composition Over Class Inheritance
    * Leads to more flexible designs
    * Composition and Delegation can replace the use of mixins to make behaviors available to different classes, but cannot replace the use of interface inheritance to define a hierarchy of types

### Chapter 13: Operator Overloading: Doing It Right

* operator overloading allows user-defined objects to iteroperate with infix operators or unary operators
* Python does impose some limitations
    * cannot overload operators for the built-in types
    * cannot create new operators
    * a few operators cannot be overloaded (`is`, `and`, `or`, `not`)
* Fundamental rule of operators: always return a new object
    * Do not modify `self`, create a new instance of a suitable type

* Think about what operators mean in the problem domain, ex. `~` could represent a negation of an `SQL WHERE` clause
* Look at what the [Python Data Model](https://docs.python.org/3/reference/datamodel.html) recommends for container types (i.e. sequences should support `+` for concatenation and `*` for repetition)

#### Reverse Methods

* If a + b does not work, i.e. `a.__add__(b)`, python tries `b.__radd__(a)`
    * If not implemented, should return a `NotImplemented` singleton

* If an operator special method cannot return a valid result because of type incompatibility, it should return `NotImplemented` and not raise `TypeError`
    * This leaves the door open for the implementor of the other operand type to perform the operation when Python tries the reversed method call

* duck typing :If an infix operator method raises an exception it aborts the operator dispatch algorithm
    * best to return `NotImplemented`
* goose typing: `isinstance(obj, cls)` with `cls` being an abstract base class

#### Rich Comparison Operators

* `==` both forward and reverse invoke `__eq__`
* forward call to `__gt__` is followed by a reverse call to `__lt__` with reversed arguments
* when `__eq__` is defined and does not return `NotImplemented`, `__ne__` returns that result negated
* special fallback for `==` and `!=`: compare object IDs as last resort
* [`@functools.total_ordering`](https://docs.python.org/3/library/functools.html#functools.total_ordering) automatically generates methods for all rich comparison operators in any class that defines at least a couple of them

#### Augmented Assignment Operators

* If a class does not implement the inplace operator methods, the augmented assignment operators are just syntactic sugar, i.e. `a += b` is treated as `a = a + b`
* If we implement an inplace operator method for a mutable type, we should not create a new object and return it; the user expects us to modify it *inplace*
    * must return `self`
* if a forward infix operator method is designed to work with operands of the same type as `self`, it's useless to implement the corresponding reverse method because that will be invoked only when dealing with an operand of a different type
* `+` usually requires that both operands are of the same type
* `+=` often accepts any iterable as the righthand operand

---

## Part V: Control Flow

### Chapter 14: Iterables, Iterators, and Generators

* every collection is an *iterable*
* all sequences are *iterable*

#### `iter` Function

Whenever the iterpreter needs to iterate over an object, it calls `iter(x)`

Function checks:
* if `__iter__` method is implemented
* if not, is `__getitem__` implemented?
* if not, `TypeError`

**iterable** is any object in which `iter()` can obtain an *iterator*.

* Object is iterable if it returns an iterator
* Calling `next()` on iterator gives next item
    * if the iterator is exhausted, raises `StopIteration` exception

* once exhausted, iterator becomes useless
* not possible to reset an iterator

* `iter` can be called with two arguments to create an iterator from a function or a callable. First argument must be a callable to be invoked repeatedly (with no arguments) to yield values, and the second argument is a sentinel which, when returned by the calalble, causes the iterator to raise `StopIteration` instead of yielding the sentinel

#### Iterator Interface

Standard interface for an iterator has two methods:
* `__next__` returns next available item; raises `StopIterator` when exhausted
* `__iter__` returns self

* Iterator is not a type, but a protocol so we will need to check if it has certain methods to say it's an iterator
    * `isinstance(x, abc.Iterator)`

* **iterator** any object that implements `__next__` that returns the next item in the series or raises a `StopIteration`
    * iterators also implement `__iter__` so iterators are also **iterable**

#### Classic Iterator

* iterables have an `__iter__` method that instantiates a enw iterator every time. Iterators implement a `__next__` method that returns individual items, and an `__iter__` method that returns `self`

> Iterators are also iterable, but iterables are not iterators

#### Generator Function

* any Python function that has a `yield` keyword in its body is a *generator function* which returns a *generator object* when its called
* generators are iterators that produce the values of the expressions passed to yield

> A generator function builds a generator object that wraps the body of the function. When we invoke `next(...)` on the generator object, execution advances to the next `yield` in the function body, and the `next(...)` call evaluates to the value yielded when the function body is suspended. Finally when the function body returns, the enclosing generator object raises `StopIteration`, in accorance with the `Iterator` protocol

#### Lazy Implementation

* lazy implementation postpones producing values to the last possible moment
* saves memory and may avoid useless processing
* Python 3 has lots of lazy implementations, look into this

#### Generator Expression

* syntactic sugar for generator functions
* lazy version of a list comprehension
* instead of eagerly building a list, it returns a genrator that will produce items on demands

#### Generator Functions in the Standard Library

*Filtering Generator Functions* - `yield` items from input iterator without changing it

* [`itertools.compress`](https://docs.python.org/3/library/itertools.html#itertools.compress)(data, selectors)

Consumes two iterables in parallel, yields items from data when selector is truthy

```console
list(itertools.compress('ABCDEF', [1,0,1,0,1,1]))
['A', 'C', 'E', 'F']
```

* [`itertools.dropwhile`](https://docs.python.org/3/library/itertools.html#itertools.dropwhile)(predicate, iterable)

Drops elements from iterable as long as predicate is true, then returns every element

```console
>>> list(itertools.dropwhile(lambda x: x < 5, range(10)))
[5, 6, 7, 8, 9]
```

* [`filter`](https://docs.python.org/3/library/functions.html#filter)(predicate, iterable)

Returns elements from iterable when predicate is `True`

```console
>>> list(filter(lambda x: x < 5, range(10)))
[0, 1, 2, 3, 4]
```

* [`itertools.filterfalse`](https://docs.python.org/3/library/itertools.html#itertools.filterfalse)(predicate, iterable)

Returns elements from iterable when predicate is `False`

```console
>>> list(itertools.filterfalse(lambda x: x < 5 or x > 8, range(10)))
[5, 6, 7, 8]
```

* [`itertools.islice`](https://docs.python.org/3/library/itertools.html#itertools.islice)(iterable, stop)

Yields selected elements from the iterable

* [`itertools.takewhile`](https://docs.python.org/3/library/itertools.html#itertools.takewhile)(predicate, iterable)

Takes elements from iterable as long as predicate is true, then drops every element after

```console
>>> list(itertools.takewhile(lambda x: x < 5, range(10)))
[0, 1, 2, 3, 4]
```

*Mapping Generator Functions* - `yield` items computed from each item in the input iterable

* [`itertools.accumulate`](https://docs.python.org/3/library/itertools.html#itertools.accumulate)(iterable[, func])

Yields accumulated sums; if `func` is supplied, it will perform the binary accumulate of the operator

```console
>>> list(itertools.accumulate(range(10)))
[0, 1, 3, 6, 10, 15, 21, 28, 36, 45]

>>> list(itertools.accumulate(range(1, 10), operator.mul))
[1, 2, 6, 24, 120, 720, 5040, 40320, 362880]

>>> list(itertools.accumulate(range(10), min))
[0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
```

* [`enumerate`](https://docs.python.org/3/library/functions.html#enumerate)(iterable, start=0)

Yields a 2-tuple of the form `(index, item)` where `index` is counted from `start` and `item` is taken from the `iterable`

```console
>>> seasons = ['Spring', 'Summer', 'Fall', 'Winter']
>>> list(enumerate(seasons))
[(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
```

* [`map`](https://docs.python.org/3/library/functions.html#enumerate)(function, iterable, ...)

Applies `function` to every item of `iterable`, yielding the result

```console
>>> list(map(lambda x, y: x + y, range(5), range(1, 6)))
[1, 3, 5, 7, 9]
```

* [`itertools.starmap`](https://docs.python.org/3/library/itertools.html#itertools.starmap)(function, iterable)

If arguments parameteres are grouped into tuples, starmap is function to use.

The primary advantage to using starmap is that it’s [trivial to make parallel](http://n-s-f.github.io/2016/12/23/starmap-pattern.html.

```console
>>> list(itertools.starmap(lambda x, y: x + y, [(2,5), (3,2), (10,3)]))
[7, 5, 13]
```

*Merging Generator Functions* - yield items from multiple input iterables

* [`itertools.chain`](https://docs.python.org/3/library/itertools.html#itertools.chain)(*iterables)

Makes an iterator that returns elements from first iterable until its exhausted, then moves onto the next iterable, until all iterables are exhausted

```console
list(itertools.chain('ABC', 'DEF'))
>>> ['A', 'B', 'C', 'D', 'E', 'F']
```

* [`itertools.chain.from_iterable](https://docs.python.org/3/library/itertools.html#itertools.chain.from_iterable)(iterable)

Alternative constructor, takes a list.

```console
list(itertools.chain.from_iterable(['ABC', 'DEF']))
>>> ['A', 'B', 'C', 'D', 'E', 'F']
```

* [`itertools.product`](https://docs.python.org/3/library/itertools.html#itertools.product)(*iterable, repeat=1)

Cartesian product of input iterables

```console
>>> list(itertools.product('AB', 'ab'))
[('A', 'a'), ('A', 'b'), ('B', 'a'), ('B', 'b')]
```

* [`zip`](https://docs.python.org/3/library/functions.html#zip)(*iterables)

Yields n-tuples built from items taken from the iterables, silently stoppping when the first iterable is exhausted

```console
>>> list(zip('ABCD', 'abc'))
[('A', 'a'), ('B', 'b'), ('C', 'c')]
```

* [`itertools.zip_longest`](https://docs.python.org/3/library/itertools.html#itertools.zip_longest)(*iterables, fillvalue=None)

Yields n-tuples built from items taken from the iterables, stopping only when the last iterable is exhausted, filling blanks with `fillvalue`

```console
>>> list(itertools.zip_longest('ABCD', 'abc', fillvalue=None))
[('A', 'a'), ('B', 'b'), ('C', 'c'), ('D', None)]
```

*Expansion Generator Functions* - yield more than one value per input item

* [`itertools.combination`](https://docs.python.org/3/library/itertools.html#itertools.combinations)(iterable, r)

Yield combinations of `r` length items from the items yielded by the `iterable`

```console
>>> list(itertools.combinations('ABCD', 2))
[('A', 'B'), ('A', 'C'), ('A', 'D'), ('B', 'C'), ('B', 'D'), ('C', 'D')]

>>> list(itertools.combinations(range(4), 3))
[(0, 1, 2), (0, 1, 3), (0, 2, 3), (1, 2, 3)]
```

* [`itertools.combination_with_replacement`](https://docs.python.org/3/library/itertools.html#itertools.combinations_with_replacement)(iterable, r)

Yield combinations of `r` length items from the items yielded by the `iterable`

```console
>>> list(itertools.combinations_with_replacement('ABC', 2))
[('A', 'A'), ('A', 'B'), ('A', 'C'), ('B', 'B'), ('B', 'C'), ('C', 'C')]

>>> list(itertools.combinations_with_replacement(range(2), 3))
[(0, 0, 0), (0, 0, 1), (0, 1, 1), (1, 1, 1)]
```

* [`itertools.count`](https://docs.python.org/3/library/itertools.html#itertools.count)(start=0, step=1)

Yield numbers start at `start` and incrementing by `step`

```console
>>> list(itertools.count(5))
[5, 6, 7, 8, 9, ...]
```

* [`itertools.cycle`](https://docs.python.org/3/library/itertools.html#itertools.cycle)(iterable)

Yield items from iterable storing a copy of each, then yield the entire sequence repeatedly, indefinitely

```console
>>> itertools.cycle('ABCD')
A B C D A B C D A B C D ...
```

* [`itertools.permutations`](https://docs.python.org/3/library/itertools.html#itertools.permutations)(iterable, r=None)

Yield successive r length permutations of elements in the iterable

```console
>>> list(itertools.permutations('ABC', 2))
[('A', 'B'), ('A', 'C'), ('B', 'A'), ('B', 'C'), ('C', 'A'), ('C', 'B')]
```

* [`itertools.repeat`](https://docs.python.org/3/library/itertools.html#itertools.repeat)(object[, times])

Yield the given object repeatedly, indefinitely unless a number of times is given

Common use: providing a fixed argument in map

```console
list(itertools.repeat(10, 3))
>>> [10, 10, 10]
```

*Rearranging Generator Functions* - yield all items in the input iterables, but rearranged in some way

* [`itertools.groupby`](https://docs.python.org/3/library/itertools.html#itertools.groupby)(iterable, key=None)

Yields 2-tuples of the form `(key, group)` where `key` is the grouping criterion and `group` is a generator yielding items in the group

```console
>>> [k for k, g in itertools.groupby('AAAABBBCCDAABBB')]
['A', 'B', 'C', 'D', 'A', 'B']
```

* [`reversed`](https://docs.python.org/3/library/functions.html#reversed)(seq)

Yields items from `seq` in reverse order, from last to first; `seq` must be a sequence or implement the `__reversed__` special method

```console
>>> list(reversed('ABC'))
['C', 'B', 'A']
```

* [`itertools.tee`](https://docs.python.org/3/library/itertools.html#itertools.tee)(iterable, n=2)

Yields a tupe of `n` generators, each yielding the items of the input iterable independently

```console
>>> list(itertools.tee('ABC', 5))
[<itertools._tee at 0x104cdc948>,
 <itertools._tee at 0x104cdc188>,
 <itertools._tee at 0x104ae99c8>,
 <itertools._tee at 0x104ae9708>,
 <itertools._tee at 0x104ae9a08>]
```

[More info](http://code.activestate.com/recipes/305588-simple-example-to-show-off-itertoolstee/)

*Iterable Reducing Functions* - all take an iterable and return a single result

* [`all`](https://docs.python.org/3/library/functions.html#all)(iterable)

Returns `True` if all items in iterable are truthy, otherwise `False`

* [`any`](https://docs.python.org/3/library/functions.html#any)(iterable)

Returns `True` if any item in the iterable is truthy, otherwise `False`

* [`max`](https://docs.python.org/3/library/functions.html#max)(iterable[, key=[, default=]])
* [`min`](https://docs.python.org/3/library/functions.html#min)(iterable[, key=[, default=]])

Returns the largest / smallest item in an iterable

* [`functools.reduce`](https://docs.python.org/3/library/functools.html#functools.reduce)(function, iterable[, initializer])

Returns the result of applying `function` to the first pair of itmes, then to that result and the third item and so on; if given, initial forms the initial pair with the first item

* [`sum`](https://docs.python.org/3/library/functions.html#sum)(iterable[, start])

Returns the sum of all items in iterable

#### Further Reading

* [`Itertools` Recipes](https://docs.python.org/3/library/itertools.html#itertools-recipes)
* [Loop Better: A Deeper Look at Iteration in Python](https://www.youtube.com/watch?v=V2PkkMS2Ack)

### Chapter 15: Context Managers and `else` Blocks

what makes python awesome kenote

#### `else` Blocks Beyond `if`

|block|rules|
|-|-|
|`for`|Will run once the `for` loop runs to completion|
|`while`|Will run once the `while` loop exists because condition becomes *falsey*|
|`try`|Will run if no exception is raised in `try`|

* `else` blocks make code easier to read and saves us from having to use extra `if` statements

#### `with` Statement

[A Gentle Introduction to Context Managers](https://alysivji.github.io/managing-resources-with-context-managers-pythonic.html). My blog post on Context Managers covers a lot of the same topics.

##### Examples from the Standard Library

* managing transacions in the `sqlite3` module
* holding locks, conditions, and semaphores in `threading` code
* setting up environments for arithmetic operations with `Decimal` objects
* Applying temporary patches to objects for testing

##### [`@contextmanager`](https://docs.python.org/3/library/contextlib.html#contextlib.contextmanager) decorator

* Can be used in conjunction with a function generator to create a context manager
* Split into two parts: code before `yield` (`__enter__`) and code after `yield` (`__exit__`)
* You must explicitly re-reaise an exception in the decorated function if you don't want `@contextmanager` to supress it

#### Additional Resources

* [Python `with` Statement by Example](http://preshing.com/20110920/the-python-with-statement-by-example/)
* Raymond Hettinger keynote at PyCon2013 on [What Makes Python Awesome](https://youtu.be/NfngrdLv9ZQ). He gets into context managers around 23 minutes in

### Chapter 16: Coroutines

[PEP 342](https://www.python.org/dev/peps/pep-0342/) -- Coroutines via Enhanced Generators

> A **coroutine** is syntactically like a generator: just a function with the `yield` keyword in its body. However, in a coroutine, `yield` usually appears on the right side of an expression (e.g. `datum = yield`), and it may or may not produce a value - if there is no expression after the `yield` keyword, the generator yields `None`.

* `yield` is a control flow device that can be used to yield control to a centralized scheduler or that other coroutines may be activated

* Coroutine:
    * may receive data from the caller, `.send(data)`
    * can throw exception, `.throw(...)`
    * can terminate, `.close(...)`

* Coroutine can be in one of four states:
    * Waiting to start execution, `GEN_CREATED`
    * Currently being exectued by the interpreter, `GEN_RUNNING`
    * Currently suspended at a yield expression, `GEN_SUSPENDED`
    * Execution has completed, `GEN_CLOSED`

* need to activate a coroutine via `next(my_coro)` or `my_coro.send(None)`
    * initial call primes the coroutine, i.e. advances it to the first `yield`
    * yields first item and then waits for values to be passed in via `.send()`

* execution of the coroutine is suspended exactly at the `yield` keyword. The code to the right of the `=` is evaluated before the actual assignment happens

* we can't do anything with a coroutine until we prime it, `next()`, this is the perfect usecase for a decorator!

#### Coroutine Termination and Exception Handling

* An unhandled exception within a coroutine propagates it to the caller of the `next` or `send` that triggered it

`generator.throw(exc_type[, exc_value[, traceback]])`

> Causes the yield express where the generator was paused to raise the exception given. If the exception is handled by the generator, flow advances to the next `yield`, and the value yielded because the value of the `generator.throw` call. If the exception is not handled by the generator, it propagates to the context of the caller.

`generator.close()`

> Causes the `yield` expression where the generator was paused to raise a `GeneratorExit` exception. No error is reported to the caller if the generator does not handle that exception or raises `StopIteration`. When receiving `GeneratorExit`, the generator must not yield a value, otherwise a `RuntimeError` is raised. If any other exception is raised by the generator, it propagates to the caller.

* if it is necessary that some cleanup code is run no matter how the coroutine ends, you need to wrap the relevant part of the coroutine body in a `try/finally` block

#### Returning a Value from a Coroutine

* can `return` but you have to deal with `StopIteration` exceptions... but the `yield from` keyword introduced in [PEP 380](https://www.python.org/dev/peps/pep-0380/) fixes this

* there are a lot of moving pieces but the basic idea is that you can have a caller which calls a generator that calls a subgenerator using `yield from`

* `yield from` opens a bidirectional channel from the outermost caller to the innermost subgenerator so that values can be sent and yielded back and forth directly from them, and exceptions can be thrown all the way in without adding a lot of exception handling boilerplate code in the intermediate corouties

* `yield from` assumes subgenerators are primed

#### Coroutine Use Case

* coroutines provide natural way of expression many algorithsm such as simulations, games, asynchronous I/O, event-driven programming, or co-operative multitasking

#### `async` and `await` in Python 3.5

* `yield from` syntax is clunky
* [PEP 492](https://www.python.org/dev/peps/pep-0492/) -- Coroutines with async and await syntax, tries to fix this
* [Raymond Hettinger - Keynote on Concurrency](https://www.youtube.com/watch?v=9zinZmE3Ogk)
* [The State of Python Coroutines: Python 3.5 -- async and await](http://www.andy-pearce.com/blog/posts/2016/Jul/the-state-of-python-coroutines-python-35/)

#### Further Reading

* David Beazley has [tons of talks](http://www.dabeaz.com/talks.html) on generators and coroutines
    * [Generator Tricks for Systems Programmers](http://www.dabeaz.com/generators/)
    * [A Curious Course on Coroutines and Concurrency](http://www.dabeaz.com/coroutines/)
    * [Generators: The Final Frontier](http://www.dabeaz.com/finalgenerator/)
    * [Python Concurrency From the Ground Up:](http://pyvideo.org/pycon-us-2015/python-concurrency-from-the-ground-up-live.html)
    * [Keynote: Topics of Interest (Asyncio)](https://www.youtube.com/watch?v=ZzfHjytDceU)
    * [Fear and Awaiting in Async: A Savage Journey to the Heart of the Coroutine Dream](https://www.youtube.com/watch?v=E-1Y4kSsAFc)
    * [The Other Async (Threads + Asyncio = Love)](https://www.youtube.com/watch?v=x1ndXuw7S0s)
* [The State of Python Coroutine](http://www.andy-pearce.com/blog/posts/2016/Jun/the-state-of-python-coroutines-yield-from/)
* [Greedy algorithms with coroutines](http://seriously.dontusethiscode.com/2013/05/01/greedy-coroutine.html)
* [Iterables, Iterators and Generators](http://nbviewer.jupyter.org/github/wardi/iterables-iterators-generators/blob/master/Iterables,%20Iterators,%20Generators.ipynb)

### Chapter 17: Concurrency with Futures

> Both processes and threads are independent sequences of execution. The typical difference is that threads (of the same process) run in a shared memory space, while processes run in separate memory spaces.

(via [StackOverflow](https://stackoverflow.com/questions/200469/what-is-the-difference-between-a-process-and-a-thread))

[PEP 3148 -- futures - execute computations asynchronously](https://www.python.org/dev/peps/pep-3148/)

* **futures** are objects representing the asynchronous execution of an operation
    * represent deferred computation that may or may not have completed
    * encapsulate pending operations so that they can be put in queues, their state of completion can be queried, and their results (or exceptions) can be retrieved when available
    * this package treats threads, processes, and queues as infrastructure not something we have to deal with directly

* to handle network I/O efficiently, you need concurrency
    * it involves high latency so instead of wasting CPU cycles waiting, we can do something else until a response comes back from the network
    * [Threads, processes and concurrency in Python: some thoughts](http://www.artima.com/weblogs/viewpost.jsp?thread=299551)

* every blocking I/O call in the standard library releases the GIL!
    * this makes Python threads perfect for I/O bound systems

* the main features of the `concurrent.futures` package are teh `ThreadPoolExecutor` and `ProcessPoolExecutor` classes which implement an interface that allows you to submit callables for execution in different threads or processes, respectively. The classes manage an internal pool of worker threads or process and a queue of tasks to be executed

* Write concurrent code by refactoring the body of a sequential `for` loop into a function to be called concurrently

|`concurrent.futures.Future`|`asyncio.Future`|
|-|-|
|nonblocking `.done()`|nonblocking `.done()`|
|`.add_done_callback()`|`.add_done_callback()`|
|`.result()` blocks caller's thread until result is ready; can accept timeout|`.result()` doesn't support timeout; need to use `yield from` / `await`|
|`.as_completed(futures_iterable)` results iterator that yields futures as they are done|-|

* `Executor.map` function returns results in the exact same order as the calls are started

* if we want results as they are ready, we need to use the `Executor.submit` method and the `futures.as_completed` function

#### Threading and Multiprocessing Alternatives

* futures are a higher level abstraction
* for lower level wonks, we can dig into [`threading`](https://docs.python.org/3/library/threading.html) and [`multiprocessing`](https://docs.python.org/3/library/multiprocessing.html#module-multiprocessing) modules in Python's Standard Libray

* [PEP 371](https://www.python.org/dev/peps/pep-0371/) -- Addition of the multiprocessing package to the standard library

#### Futher Reading

* [The Future is Soon](http://pyvideo.org/pycon-au-2010/pyconau-2010--the-future-is-soon.html)
* Distributed task queues: celery!

### Chapter 18: Concurrency with `asyncio`

[Docs](https://docs.python.org/3/library/asyncio.html)

> Concurrency is about dealing with lots of things at once.
>
> Parallelism is about doing lots of things at once.
> - [Rob Pike _Concurrency Is Not Parallelism_](https://www.youtube.com/watch?v=cN_DpYBzKso)

* There are two ways to prevent blocking calls from halting the progress of the entire application:
    * Run each blocking operation in a separate thread
    * Turn every blocking operation into a nonblocking asynchronous call

#### `asyncio`

* package that implements concurrency with coroutines driven by an event loop
* uses a stricter definition of *coroutine*
    * needs to use `yield from` not `yield`
    * driven by caller invoking it thru `yield from` or by passing the coroutine to one of the `asyncio` functions
    * add `@asyncio.coroutine` decorate to coroutine
        * not required, but makes it stand out and improves debugging

* with coroutines, everything is protected against interruption by default, you must explicitly yield to let the rest of the program run
    * can only cancel a coroutine when it's suspended at a `yield` point

* Advantage of coroutines: functions can be suspended and resumed

* blocking operations are implemented as coroutines, code delegates to them via `yield from` so they can run asynchronously
* always need to use asychronous version of functions because they will ceed control back to the event loop. When the coroutine is done, it returns a result to the suspended coroutine, resuming it

> **TIP**: quint and pretend the `yield from` keywords are not there. Code will read like sequential code

#### [`asyncio.Task`](https://docs.python.org/3/library/asyncio-task.html#asyncio.Task)

* like a [green thread](https://en.wikipedia.org/wiki/Green_threads) in libraries that implement cooperative multitasking
* don't instantiate `Task` objects yourself, you get them by passing a coroutine to `asyncio.aync(...)` or `loop.create_task(...)`
* when you get a `Task` object, it is already scheduled to run
* `Task.cancel()` instance method raises a `CancelledError` inside the coroutine
    * can deal with this by catching exception in `yield` where its suspended

#### [`asyncio.Future`](https://docs.python.org/3/library/asyncio-task.html#asyncio.Future)

* Using `yield from` with a future automaticlly takes care of waiting for it to finish
    * `yield from` is used to give control back to the event loop
    * `result = yield from my_future`

#### Yielding from Futures, Tasks, and Coroutines

* in order to execute, a coroutine must be scheduled and then it's wrapped in an `asyncio.Task`. Two main ways of obtaining a `Task`:
    * [`asyncio.ensure_future()`](https://docs.python.org/3/library/asyncio-task.html#asyncio.ensure_future)
    * [`BaseEventLoop.create_task(coro)`](https://docs.python.org/3/library/asyncio-eventloop.html#asyncio.AbstractEventLoop.create_task)

* several `asyncio` functions accept coroutines and wrap them in `asyncio.Task` objects automatically, using `asyncio.async`

* can pass an iterable of coroutines to [`asyncio.wait`](https://docs.python.org/3/library/asyncio-task.html#asyncio.wait), which when driven by `loop.run_until_complete`, would return results when **all** are complete

* __semaphore__ is a variable or abstract data type used to control access to a common resource by multiple processes in a concurrent system such as a multiprogramming operating system (source: [wikipedia](https://en.wikipedia.org/wiki/Semaphore_(programming)))
    * ['asyncio.Semaphore`](https://docs.python.org/3/library/asyncio-sync.html#asyncio.Semaphore)
    * used to control how many concurrent "downloads" occur at any given time

* [`asyncio.as_completed(fs, timeout=None)`](https://docs.python.org/3/library/concurrent.futures.html#concurrent.futures.as_completed) takes in a list of futures and returns an iterator of futures as they are completed

* can use [`EventLoop.run_in_executor`](https://docs.python.org/3/library/asyncio-eventloop.html#asyncio.AbstractEventLoop.run_in_executor) to avoid blocking the event loop for I/O

* coroutines are a lot cleaner than callbacks
* coroutines only do things when driven
    * drive coroutine using either `yield from` or pass it to `asyncio`

---

## Part 4: Metaprogramming

### Chapter 19: Dynamic Attributes and Properties


---

## Todo

questions

* combining thread and process. figure out use cases for both and figure out how to do what you want

3 approaches.. sequential, threaded and asynchronous
 have an example for the blog... count to 10

 work with asyncio. pep
