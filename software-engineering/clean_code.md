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
- [Chapter 7: Error Handling](#chapter-7-error-handling)
  - [Conclusion](#conclusion-1)
- [Chapter 8: Boundaries](#chapter-8-boundaries)
  - [Conclusion](#conclusion-2)
- [Chapter 9: Unit Tests](#chapter-9-unit-tests)
  - [Rules for Clean Test](#rules-for-clean-test)
  - [Three Laws of TDD](#three-laws-of-tdd)

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

- Use intention revealing names
    - choosing good names takes time but saves more than it takes
- Avoid disinformation
    - avoid leaving false clues that obscure meaning of code
    - avoid words whose entrenched meanings vary from our intended meaning
    - spelling similar concepts similarly is *information*
- Make meaningful distinctions
    - if names must be different, then they should also mean something different
    - noise words are redundant (adding `variable`, `df` to name)
    - distinguish names in such a way that the reader knows what the differences offer
- Use pronounceable names
- Use searchable names
    - the length of a name should correspond to the size of its scope
    - if a variable or constant might be seen or used in multiple palces in code, it's imperative to give it a search-friendly name
- Avoid encodings
    - type prefixes
- Avoid mental mapping
    - readers shouldn't have to mentally translate your names into other names they already know
- Don't be cute
    - no inside jokes
    - say what you mean, mean what you say
- Pick one word per concept
- Don't pun
- Use solution domain names
    - use computer science terminology in names
- Use problem domain names
    - code that has more to do with problem domain concepts should have names drawn from the problem domain
- Add meaningful context
    - place names in context for your reader by enclosing them in well-named classes, functions, or namespaces
- Don't add gratuitous context
- Class names
    - classes and objects should have noun or noun phrase names like `Customer`, `WikiPage`, `Account`, and `AddressParser`
    - avoid words like `Manager`, `Processor`, `Data`, or `Info` in the name of a class
    - class names should never be verbs
- Method names
    - method should have verb or verb phrase names like `postPayment`, `deletePage`, or `save`

## Chapter 3: Functions

> Functions are the first line of organization in any program

> - Functions should do one thing. They should do it well. They should do it only.

### Function Goals

- small
- should not be large enough to hold nested structures
- Statements within our function should be at same level of abstraction
- want code to have a top down narrative
- avoid switch statements
    - if you have to use switch, do it somewhere and abstract it away with polymorphic classes
- use descriptive names, be consistent
- don't use too many function arguments
- flag arguments are ugly (eh, debatable. I like sane defaults)
- good names can go a long way towards explaining the intent of a function and order and intent of arguments
- have no side effects
- avoid temporal coupling
- if your functions must change the state of something, have it change the state of its owning object
- should either do something or answer something, but not both
- prefer exceptions to returning error codes
    - new exceptions are derivatives of the exception class
- extract try/catch blocks into functions of their own
- duplication is the root of all evil
- write your code, write tests, clean your code with tests in place

## Chapter 4: Comments

> The proper use of comments is to compensate for our failure to express ourself in code

- truth can only be found in one place: the code
- clear and expressive code with few comments is far superior to cluttered and complex code with lots of comments
- it only takes a few seconds of thought to explain most of your intent in code: in most cases, create a function that says the same thing as the comment you want to write

### Good Comments

- legal comments
- informative comments
- explanation of intent
- clarification
- warning of consequence
- `TODO` comments
- amplification
- documenting public api using Javdocs or Sphinx

## Chapter 5: Formatting

> The coding style and readability set precedents that continue to affect maintainability and extensibility long after the original code has been changed beyond recognition. Your style and discipline survives, even though your code does not.

- smaller files are usually easier to understand than larger files
- think of reading code like reading a newspaper (headline with details as you go further)
- groups of lines represent a complete thought, and these should be separated from other blocks with blank lines
- lines of code that are tightly related should appear vertically dense
- concepts thare are closely related should be kept vertically close to each other
- variables should be declared as close to their usage as possible
  - instance variable should be declared at the top of a class
- if one function calls another, they should be vertically close and the caller should be above the callee (gives the program a natural flow)
- certain bits of code want to be near other bits so we should keep them together vertically
- try to keep line lengths short, 80-100-120
- use horizontal whitespace to associate things that are strongly related and disassociate things that are more weakly related
- source code is a hierarchy rather than an outline
  - to make the hierarchy of scopes visible, we indent the lines of source code in proportion to their position in the hiearchy
- team should agree on a consistent style that will be used for formatting

### The Newspaper Metaphor

> Think of a well-written newspaper article. You read it vertically. At the top you expect a headline that will tell you what the story is abvout and allows you to decide whether it is something you want to read. The first paragraph gives you a synopsis of the whole story, hiding all the details while giving you the broad-brush concepts. As you continue downward, the details increase until you have all the dates, names, quotes, claims, and other minutia.
>
> We would like a source file to be like a newspaper article. The name should be simple, but explanatory. The name, by itself, should be sufficient to tell us whether we are in the right module or not. The topmost parts of the source file should provide the high-level concepts and algorithms. Details should increase as we move downward, until the end we find the lowest level functions and details in the source file.
>
> A newspaper is composed of many articles; most are very small. Some are a bit larger. Very few contain as much text as a page can hold. This makes the newspaper *useable*. If the newspaper were just one long story containing a disorganized agglomeration of facts, dates, and names, then we simply would not read it.

## Chapter 6: Objects and Data Structures

> Hiding implementations is not just a matter of putting a layer of functions between the variables. Hiding implementations is about abstractions! A class does not simply push its variables out thru getters and setters. Rather it exposes abstract interfaces that allow its users to manipulate the *essence- of the data, without having to know its implementations.
>
> We do not want to expose the details of our data. Rather we want to express our data in abstract terms. This is not merely accomplished by using interfaces and/or getters and setters. Serious thought needs to be put into the best way to represent the data that an object contains. The worst option is to blithely add getters and setters.

- Objects hide their data behind abstractions and expose functions that operate on that data
  - OO code, on the other hand, makes it easy to add new classes without changing existing functions
  - OO code makes it hard to add new functions because all the classes must change
- Data structures expose their data and have no meaningful functions
  - procedural code (code using data structures) makes it easy to add new functions without changing existing data structures
  - procedural code makes it hard to add new data structures because all the functions must change
- **Law of Demeter*- - a module should not know about the innards of the objects it manipulates
- **Data Transfer Objects*- (DTOs) refer to data structures that are classes with public variables and no functions
  - [`collections.namedtuple`](https://docs.python.org/3/library/collections.html#collections.namedtuple) in Python
  - DTOs are first in a series of translation stages that convert raw data in a database into objects in the application code

### Conclusion

> Objects expose behavior and hide data. This makes it easy to add new kinds of objects without changing existing behaviors. It also makes it hard to add new behaviors to existing objects. Data structures expose data and have no significant behavior. This makes it easy to add new behaviors to existing data structures but makes it hard to add new data structures to existing functions.
>
> In any given system, we will sometimes want the flexibility to add new data types, and so we prefer objects for that part of the system. Other times we will want the flexibility to add new behaviors, and so in that part of the system we prefer data types and procedures. Good software developers understand these issues without prejudice and choose the approach that is best for the job at hand.

## Chapter 7: Error Handling

> Error handling is important, but if it obscures logic, it's wrong

- try to write tests that force exceptions and then add behavior to your handler to satisfy tests
  - this will cause you to build the transaction scope of the `try` block first and will help maintain the transaction nature of that scope
- each exception that you throw should provide enough context to determine the source and location of an error
- create informative error messages and pass them along with your exceptions. Mention the operation that failed and the type of failure. If you are logging in your application, pass along enough information to be able to log the error in your `catch`
- if we are doing the same work in many different `except` blocks, we can simplify our code by wrapping the API we are calling and making sure that it returns a common exception type
  - benefits of wrapping API:
    - minimize dependency upon it
    - move to a different library in the future without much penalty
    - easier to mock when you are testing
    - you can define an API that you feel comfortable with vs having to use the vendor's API
- often a single exception is fine for a particular area of code
  - information sent with exception can distinguish the errors
  - use different classes only if there are types when you want to catch one exception and allow another one to pass thru
- create a class or configure an object so that it handles a special case for you (Fowler -- Special Case Pattern)
  - client code does not have to deal with exceptional behavior, it's encapsulated inside of the special case object
- don't return `null`, return an empty container
  - this isn't the best advice for Python
- do not pass `null` into methods

### Conclusion

> Clean code is readable, but it must also be robust. These are not conflicting goals. We can write robust clean code if we see error handling as a separate concern, something that is viewable independently of our main logic. To the degree that we are able to do that, we can reason about it independently, and we can make great strides in the maintainability of our code.

## Chapter 8: Boundaries

> There is a natural tension between the provider of an interface and the user of an interface. Providers of third-party packages and frameworks strive for broad applicability so tey can work in many environments and appeal to a wide audience. Users, on the other hand, want an interface that is focused on their particular needs. This tension can cause problems at the boundaries of the system.

- if you want to use a boundary interface like `Map`, keep it inside the class, or close family of classes, where it is used. Avoid returning it from, or accepting it as an argument to, public APIs.
- it is pragmatic to write tests for third-party code
  - learning a new codebase is hard, try to write tests to explore our understanding of the library
  - tests focus on what we want out of the API
  - our tests can be used to check if new versions of the third-party library works the way we expect it to work
- define your own interface if the code you want to integrate with has not been written, do it even if it has been written
  - the Adapter Pattern encapsulates the interaction with the API and provides a single place to change when the API evolves

### Conclusion

> Interesting things happen at boundaries. Change is one of those things. Good software designs accommodate change without huge investments and rework. When we use code that is out of our control, special care must be taken to protect our investment and make sure future change is not too costly
>
> Code at the boundaries needs clear separation and tests that define expectations. We should avoid letting too much of our code know about the third-party particulars. It's better to depend on something you control than on something you don't control, lest it end up controlling you.
>
> We manage third-party boundaries by having very few places in the code that refer to them

## Chapter 9: Unit Tests

> Test code is just as important as production code. It is not a second-class citizen. It requires thought, design, and care. It must be kept as clean as production code.

- in the past we would write ad-hoc code to test our classes and methods and run it via a simple driver that we interacted with
- now we have test runners that standardize how we structure and store our tests so they can be used by anybody
- sheer bulk of tests can rival size of production code so it needs to be clean
  - dirty tests make things hard to change
  - the more tangled the test code, the more likely it is that you will spend more time cramming new tests into the suite than it takes to write the new production code
- the higher the test coverage, the less you fear
- tests allow you to change your code with near impunity
  - improve architecture and design without feature
- clean tests are **readable**
- invent domain-specific testing language that is made up of functions and utilities that make tests more convenient to write and easier to read
  - not developed up front, rather it evolves from the continued refactoring of test code that has gotten too tainted by obfuscating detail
  - pytest fixtures that return errors
- test code does not need to be as efficient as production code
- have a `getState()` method in your class that generates the current state which you can compare to expected state
- each test should encapsulate a single concept

### Rules for Clean Test

- **F**ast. Tests should be fast
- **I**ndependent. Tests should not depend on each other.
- **R**epeatable. Tests should be repeatable in any environment
- **S**elf-validating. Tests should have boolean output, either pass or fail
- **T**imely. Tests need to be written in a timely fashion

### Three Laws of TDD

1. You may not write production code until you have written a failing unit test.
1. You may not write more of a unit test than is sufficient to fail, and not compiling is failing.
1. You may not write more production code than is sufficient to pass the currently failing test.
