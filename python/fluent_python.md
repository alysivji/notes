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
