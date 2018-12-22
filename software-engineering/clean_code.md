# Clean Code
A Handbook of Agile Software Craftsmanship

By Robert C. Martin (aka "Uncle Bob")

<!-- TOC -->

- [Chapter 1: Clean Code](#chapter-1-clean-code)
  - [Boy Scout Rule](#boy-scout-rule)
- [Chapter 2: Meaningful Names](#chapter-2-meaningful-names)
- [Chapter 3: Functions](#chapter-3-functions)
  - [Function Goals](#function-goals)
- [Chapter 4: Comments](#chapter-4-comments)
  - [Good Comments](#good-comments)
- [Chapter 5: Formatting](#chapter-5-formatting)
  - [The Newspaper Metaphor](#the-newspaper-metaphor)
- [Chapter 6: Objects and Data Structures](#chapter-6-objects-and-data-structures)
  - [Conclusion](#conclusion)

<!-- /TOC -->

---
---

## Chapter 1: Clean Code

> You cannot write code if you cannot read the surrounding code. The code you are trying to write today will be hard or easy to write depending on how hard or eqasy the surrounding code is to read. So if you want to go fast, if you want to get done quickly, if you want your code to be easy to write, make it easy to read.

### Boy Scout Rule

Leave the campground cleaner than you found it

> If we all checked-in our code a little cleaner than when we checked it out, the code simply could not rot. The cleanup doesn't have to be something big. Change one variable name for the better, break up one function that's a little too large, eliminate one small bit of duplication, clean up one composite `if` statement

## Chapter 2: Meaningful Names

> Hardest thing about choosing good nmaes is that it requires good descriptive skills and a shared cultural background

* Use intension revealing names
    * choosing good names takes time but saves more than it takes
* Avoid disinformation
    * avoid leaving false clues that obscure meaning of code
    * avoid words whose entrenched meanings vary from our intended meaning
    * spelling similar concepts similarly is *information*
* Make meaningful distincions
    * if names must be different, then they should also mean something different
    * noise words are redundant (adding `variable`, `df` to name)
    * distinghuish names in such a way that the reader knows what the differences offer
* Use pronouncable names
* Use searchable names
    * the length of a name should correspond to the size of its scope
    * if a variable or constant might be seen or used in multiple palces in code, it's imperative to give it a search-friendly name
* Avoid encodings
    * type prefixes
* Avoid mental mapping
    * readers shouldn't have to mentally translate your names into other names they already know
* Don't be cute
    * no inside jokes
    * say what you mean, mean what you say
* Pick one word per concept
* Don't pun
* Use solution domain names
    * use computer science terminology in names
* Use problem domain names
    * code that has more to do with problem domain concepts should have names drawn from the problem domain
* Add meaningful context
    * place names in context for your reader by enclosing them in well-named classes, functions, or namespaces
* Don't add gratuitous context

* Class names
    * classes and objects should have noun or noun phrase names like `Customer`, `WikiPage`, `Account`, and `AddressParser`
    * avoid words like `Manager`, `Processor`, `Data`, or `Info` in the name of a class
    * class names should never be verbs
* Method names
    * method should have verb or verb phrase names like `postPayment`, `deletePage`, or `save`

## Chapter 3: Functions

> Functions are the first line of organization in any program

> * Functions should do one thing. They should do it well. They should do it only.

### Function Goals

* small
* should not be large enough to hold nested structures
* Statements within our function should be at same level of abstraction
* want code to have a top down narrative
* avoid switch statements
    * if you have to use switch, do it somewhere and abstract it away with polymorphic classes
* use descriptive names, be consistent
* don't use too many function arguments
* flag arguments are ugly (eh, debatable. I like sane defaults)
* good names can go a long way towards explaining the intent of a function and order and intent of arguments
* have no side effects
* avoid temporal coupling
* if your functions must change the state of something, have it change the state of its owning object
* should either do something or answer something, but not both
* prefer exceptions to returning error codes
    * new exceptions are derivatives of the exception class
* extract try/catch blocks into functions of their own
* duplication is the root of all evil
* write your code, write tests, clean your code with tests in place

## Chapter 4: Comments

> The proper use of comments is to compensate for our failure to express ourself in code

* truth can only be found in one place: the code
* clear and expressive code with few comments is far superior to cluttered and complex code with lots of comments
* it only takes a few seconds of thought to explain most of your intent in code: in most cases, create a function that says the same thing as the comment you want to write

### Good Comments

* legal comments
* informative comments
* explanation of intent
* clarification
* warning of consequence
* `TODO` comments
* amplification
* documenting public api using Javdocs or Sphinx

## Chapter 5: Formatting

> The coding style and readability set precedents that continue to affect maintainability and extensibility long after the original code has been changed beyond recognition. Your style and discipline survives, even though your code does not.

* smaller files are usually easier to understand than larger files
* think of reading code like reading a newspaper (headline with details as you go further)
* groups of lines represent a complete thought, and these should be separated from other blocks with blank lines
* lines of code that are tightly related should appear vertically dense
* concepts thare are closely related should be kept vertically close to each other
* variables should be declared as close to their usage as possible
  * instance variable should be declared at the top of a class
* if one function calls another, they should be vertically close and the caller should be above the callee (gives the program a natural flow)
* certain bits of code want to be near other bits so we should keep them together vertically
* try to keep line lengths short, 80-100-120
* use horizontal whitespace to associate things that are strongly related and disassociate things that are more weakly related
* source code is a hierarchy rather than an outline
  * to make the hierarchy of scopes visible, we indent the lines of source code in proportion to their position in the hiearchy
* team should agree on a consistent style that will be used for formatting

### The Newspaper Metaphor

> Think of a well-written newspaper article. You read it vertically. At the top you expect a headline that will tell you what the story is abvout and allows you to decide whether it is something you want to read. The first paragraph gives you a synopsis of the whole story, hiding all the details while giving you the broad-brush concepts. As you continue downward, the details increase until you have all the dates, names, quotes, claims, and other minutia.
>
> We would like a source file to be like a newspaper article. The name should be simple, but explanatory. The name, by itself, should be sufficient to tell us whether we are in the right module or not. The topmost parts of the source file should provide the high-level concepts and algorithms. Details should increase as we move downward, until the end we find the lowest level functions and details in the source file.
>
> A newspaper is composed of many articles; most are very small. Some are a bit larger. Very few contain as much text as a page can hold. This makes the newspaper *useable*. If the newspaper were just one long story containing a disorganized agglomeration of facts, dates, and names, then we simply would not read it.

## Chapter 6: Objects and Data Structures

> Hiding implementations is not just a matter of putting a layer of functions between the variables. Hiding implementations is about abstractions! A class does not simply push its variables out thru getters and setters. Rather it exposes abstract interfaces that allow its users to manipulate the *essence* of the data, without having to know its implementations.
>
> We do not want to expose the details of our data. Rather we want to express our data in abstract terms. This is not merely accomplished by using interfaces and/or getters and setters. Serious thought needs to be put into the best way to represent the data that an object contains. The worst option is to blithely add getters and setters.

- Objects hide their data behind abstractions and expose functions that operate on that data
  - OO code, on the other hand, makes it easy to add new classes without changing existing functions
  - OO code makes it hard to add new functions because all the classes must change
- Data structures expose their data and have no meaningful functions
  - procedural code (code using data structures) makes it easy to add new functions without changing existing data structures
  - procedural code makes it hard to add new data structures because all the functions must change
- **Law of Demeter** - a module should not know about the innards of the objects it manipulates
- **Data Transfer Objects** (DTOs) refer to data structures that are classes with public variables and no functions
  - [`collections.namedtuple`](https://docs.python.org/3/library/collections.html#collections.namedtuple) in Python
  - DTOs are first in a series of translation stages that convert raw data in a database into objects in the application code

### Conclusion

> Objects expose behavior and hide data. This makes it easy to add new kinds of objects without changing existing behaviors. It also makes it hard to add new behaviors to existing objects. Data structures expose data and have no significant behavior. This makes it easy to add new behaviors to existing data structures but makes it hard to add new data structures to existing functions.
>
> In any given system, we will sometimes want the flexibility to add new data types, and so we prefer objects for that part of the system. Other times we will want the flexibility to add new behaviors, and so in that part of the system we prefer data types and procedures. Good software developers understand these issues without prejudice and choose the approach that is best for the job at hand.
