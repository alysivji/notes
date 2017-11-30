# Fluent Python

by Luciano Ramalho

## Part 1: Prologue

## Chapter 1: The Python Data Model

Python uses special (`__dunder__`) methods which can help make our custom created objects behave like built-in objects.

We can use Python's Data Model to design "Pythonic" APIs for our classes and libraries.

### Further Reading

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
