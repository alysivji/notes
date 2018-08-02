# Clean Code
A Handbook of Agile Software Craftsmanship

By Robert C. Martin (aka "Uncle Bob")

<!-- TOC -->

- [Chapter 1: Clean Code](#chapter-1-clean-code)
    - [Boy Scout Rule](#boy-scout-rule)
- [Chapter 2: Meaningful Names](#chapter-2-meaningful-names)

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
