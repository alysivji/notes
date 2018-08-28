# A Philosophy of Software Design

by John Ousterhout

<!-- TOC -->

- [Preface](#preface)
- [Chapter 1: Introduction](#chapter-1-introduction)
- [Chapter 2: The Nature of Complexity](#chapter-2-the-nature-of-complexity)
    - [Symtoms of Complexity](#symtoms-of-complexity)
    - [Causes of Complexity](#causes-of-complexity)
    - [Complexity is Incremental](#complexity-is-incremental)
- [Chapter 3: Working Code Isn't Enough](#chapter-3-working-code-isnt-enough)
- [Chapter 4: Modules Should Be Deep](#chapter-4-modules-should-be-deep)
    - [Modular Design](#modular-design)
    - [Interface](#interface)
    - [Abstractions](#abstractions)
    - [Deep Modules](#deep-modules)
    - [Classitis](#classitis)
- [Chapter 5: Information Hiding (and Leakage)](#chapter-5-information-hiding-and-leakage)
    - [Information Hiding](#information-hiding)
    - [Information Leakage](#information-leakage)
    - [Temporal Decomposition](#temporal-decomposition)
    - [Other Topics](#other-topics)
- [Chapter 6: General-Purpose Modules are Deeper](#chapter-6-general-purpose-modules-are-deeper)
    - [Questions to Ask](#questions-to-ask)
- [Chapter 7: Different Layer, Different Abstraction](#chapter-7-different-layer-different-abstraction)
    - [Pass-through Methods](#pass-through-methods)
    - [When is interface duplication OK?](#when-is-interface-duplication-ok)
    - [Decorators](#decorators)
    - [Interface versus Implementation](#interface-versus-implementation)
    - [Pass-through Variables](#pass-through-variables)
- [Chapter 8: Pull Complexity Downwards](#chapter-8-pull-complexity-downwards)
    - [Configuration Parameters](#configuration-parameters)
    - [Rule of Thumb](#rule-of-thumb)
- [Chapter 9: Better Together Or Better Apart?](#chapter-9-better-together-or-better-apart)
    - [Bring Together: Simplify the Interface](#bring-together-simplify-the-interface)
    - [Bring Together: Eliminate Duplication](#bring-together-eliminate-duplication)
    - [Separate General-Purpose and Special-Purpose Code](#separate-general-purpose-and-special-purpose-code)
    - [Splitting and Joining Methods](#splitting-and-joining-methods)
- [Chapter 10: Define Errors Out of Existence](#chapter-10-define-errors-out-of-existence)
    - [Why exceptions add complexity](#why-exceptions-add-complexity)
    - [Too many exceptions](#too-many-exceptions)
    - [Define errors out of existence](#define-errors-out-of-existence)
    - [Mask Exceptions](#mask-exceptions)
    - [Exception aggregation](#exception-aggregation)
    - [Crash Application](#crash-application)
    - [Design special cases out of existence](#design-special-cases-out-of-existence)
    - [Taking it too far](#taking-it-too-far)
- [Chapter 11: Design it Twice](#chapter-11-design-it-twice)
    - [Ego](#ego)

<!-- /TOC -->

---
---

## Preface

* The most fundamental problem in computer science is problem decomposition: how to take a complex problem and divide it up into pieces that can be solved independently
* Overall goal is to reduce complexity; this is more important than any particular principle or idea you read here

---

## Chapter 1: Introduction

* Writing computer software is one of the purest creative activities in the history of the human race
* All programming requires is a creative mind and the ability to organize your thoughts. If you can visaulize a system, you can probably implement it in a computer program
* If we want to make it easier to write software, we msut find ways to make software simplier

Eliminating Complexity
1. Make code simpler and more obvious
2. Encapsulate complexity so that the programmer can work on a system without being exposed to all of its complexity at once (modular design)

* Software design is a continuous process that spans the entire lifecycle of a software system

* Waterfall model - divide project into discrete phases that need to be complete before starting the next phase
* Agile model - iterate your design so that each iteration exposes problems with the exiting design, which are fixed before the enxt set of features is designed

* Incremental development also means continuous redesign
    * Initial design is never the best one
    * Always look for opportunities to ipmrove the design of the system youa re working on

> Beautiful design reflects a balance between competing ideas and approaches

---

## Chapter 2: The Nature of Complexity

* The ability to reocgnize complexity is a crucial design skill. It allows you to identify problems before you invest a lot of effort in them, and it allows you to make good chocies among alternatives
* Complexity is anything related to the structure of a software system that makes it hard to understand and modify the system
* Complexity is more apparent to readers than writers
    * ask other people why they think your code is complex

> Your job as a developer is not just to create code that you can work with easily, but to create code that others can also work with easily

### Symtoms of Complexity

> One of the most important goals of good design is for a system to be obvious. In an obvious system, a developer can quickly understand how the existing code works and what is required to make a change

#### Change Amplification

* simple change requires code modifications in many different places

#### Cognitive Load

* how much a developer needs to know in order to complete a task

> Sometimes an approach that requires more lines of code is actually simpler because it reduces cognitive load

#### Unknown Unknowns

* it is not obvious which pieces of code must be modified to complete a task, or what information a developer must have to carry out the task successfully

> With unknown unknowns, it is unclear what to do or whether a proposed solution will will work

### Causes of Complexity

#### Dependencies

* exists when a given piece of code cannot be understood and modified in isolation
* fundamental part of software and cannot be completely eliminated; we purposely introduce dependencies as part of the software design process
* leads to change applification and high cognitive load

#### Obsecurity

* important information is not obvious
* creates unknown unknowns and contributes to high congnitive load

### Complexity is Incremental

* complexity isn't caused by a single catastrophic error, it accumulates in lots of small chunks
* incremental nature of complexity makes it hard to control

> It's easy to convince yourself that a little bit of complexity introcued by your current change is no big deal. However, if every developer takes this approach for every change, complexity accumulates rapidly

---

## Chapter 3: Working Code Isn't Enough

> Many organizations encourage a tactical mindset, focused on getting features working as quickly as possible. However if you want a good design, you must take a more strategic approach when you invest time to produce clean designs and fix problems

* tactical programming is short-sighted as you are rushing to get work done
* most import thing is the long-term structure of the system
* strategic programming requires an investment mindset
    * invest time to improve the design of the system
* it's worth taking a little extra time to find a simple design for each new class; rather than implementing the first idea that comes to mind, try a couple of alternative designs and pick the cleanest one
* make lots of small investments on a continual basis, adding up to about 10-20% of your total development time
* best way to lower development costs is to hire great engineers
    * they are much more productive than medicore engineers
* if your code base is a wreck, word will get out and this will make it harder to recruit
* it's a lot more fun to work in a company that cares about software design and has a clean code base

---

## Chapter 4: Modules Should Be Deep

> Design systems so that developers only need to face a small fraction of the overall compelxity at any given time

### Modular Design

* decomposing a software system into a collection of modules that are relatively independent
* modules must work together by calling each other's functions or methods, which results in modules having to know something about each other
* goal of modular design is to minimize the dependencies between modules
* module has two parts:
    1. **interface** consists of everything that a developer working in a different module must know in order to use the given module
    1. **implementation** consists of code that carries out the promises made by the interface
* best modules are those whose interfaces are much simplier than their implementation
    * simple interface minimizes complexity that a module imposes on the rest of the system
    * modifying a module without changing its interface means no other module is affected by the change

### Interface

* consists of two kinds of imformation: formal and informal
* formal parts of an interface are specified explicitly in the code
* informal parts are not specified in a way that can be understood or enforced by the programming language; these can only be described in the comments
* clearly specified interfaces indicate exactly what developers need to know in order to use the associated module

### Abstractions

* simplified view of an entity, which omits important details
* in modular programming, each module provides an abstraction in form of its interface
* can go wrong in two ways:
    * include details that are not really important, making the abstraction more complicated than necessary
    * omit important details which results in obscurity

### Deep Modules

> The best modules are deep: they have a lot of functionality behind a smiple interface

* think about depth as cost vs benefit; cost is interface, benefit is functionality

### Classitis

* conventional wisdom says that classes should be small, not deep
* results in classes that are individually simple, but produce tremendous complexity from the accumulated interfaces
* if an interface has many features, most developers will only be aware of a few of them

---

## Chapter 5: Information Hiding (and Leakage)

> Technique for achieving deep modules where the implementation is hidden and not exposed in the interface

### Information Hiding

* information hiding reduces complexity in two ways:
    1. Simplier interface to a module reduces cognitive load
    1. Easier to evolve system as design change related to information will only affect one module
* always think about the information hidden in a module
* `private` declarations are not hiding anything
* partial information hiding can also have value

### Information Leakage

> Same knowledge used in multiple places

* occurs when a design decision is reflected in multiple modules
* creates dependencies where changes require touching many different modules
* information can be leaked even if it's not in the interface
    * two functions expect the same file format
* if classes are small and closely tied to leak information, merge into single class
* pull shared information out of all affected classes and encapsulate in a new class

### Temporal Decomposition

* structure of system corresponds to the time order in which operations will occur
* when designing modules focus on the knowledge that's needed to perform each task, not the order in which tasks occur

### Other Topics

* interfaces should be designed to make the common case as simple as possible
    * best featuers are ones you get without even knowing they exist
* design private methods within a class so that each method encapsulates some information or capability and hides it from the rest of the class
* don't hiding information that is needed outside the module
    * goal is to minimize the amount of information that is needed outside of a module
    * recognize what is required and make sure it is exposed

---

## Chapter 6: General-Purpose Modules are Deeper

* generalize-purpose solution might include facilities that are never actually needed
* special-purpose approach is consistent with an incremental approach to software development
* general-purpose approach results in simpler and deeper interfaces than special-purpose approach
* general-purpose interfaces reduce cognitive load

### Questions to Ask

1. What is the simplierst interface that will cover all my current needs?
    * reducing number of methods in an API without reducing its overall capabilities makes it more general. Make sure you are not increasing number of method arguments
1. In how many situations will this method be used?
    * can you replace several special-purpose methods with a single general-purpose method
1. Is this API easy to use for my current needs?
    * if you have to write a lot of additional code to use a class for your current purpose, it probably doesn't have the right functionality

---

## Chapter 7: Different Layer, Different Abstraction

> Software systems are composed in layers, where higher layers use the facilities provided by lower layers. In a well-designed system, each layer provides a different abstraction from the layers above and below it. If adjacent layers have similar abstractions, we signals that we could have a problem with class decomposition

### Pass-through Methods

* method that does little except invoke another method, whose signature is similar or identical to that of the calling method
* make classes shallower: increase interface complexity, but do not increase total functionality
* create dependencies between classes
* indicate confusion over the division of responsiblities
* How to eliminate pass-thru methods:
    1. Expose lower level class directly to the callers of the higher level class
    1. Remove all responsibility for feature from higher level class
    1. Redistribute functionality between classes
    1. If classes can't be disentangled, merge them

### When is interface duplication OK?

* each new method should contribute significant functionality
* dispatcher method that uses arguments to figure out which method to invoke
* when several methods provide different implemtnations of the same interface, it reduces cognitive load

### Decorators

> take an existing object and extend its functionality by providing an API similar to the underlying object

* create a special-purpose extension of a class from a more generic core
* decorators classes tend to be shallow
* before creating a decorator, ask:
    * Can we add the new functionality directly to the underlying class?
    * If the new functionality is specialized, would it make sense to merge it with the use case?
    * Could the functionality be merged with an existing decorator?
    * Does new functionality need to wrap existing functionality?

### Interface versus Implementation

> The interface of a class should normally be different from its implementation: the representations used internally should be different from the abstractions that appear in the interface

### Pass-through Variables

* variable that is passed down thru a long chain of methods
* adds complexity because all intermediate methods must be aware of existence
* How to eliminate pass-through variables:
    1. Is there an object shared between the topmost and bottommost methods?
    1. Store information in a global variable, but has its own problems
    1. Context object that stores application's global state
        * allows multiple instances of the system to coexist in a single process, each with its own context
        * context will be needed many places so try to save it in system's major objects (include in constructors)
        * makes it easy to identify and manage global state of system
        * simplifies testing
        * make context variable immutable to make threadsafe

---

## Chapter 8: Pull Complexity Downwards

> More important for a module to have a simple interface than a simple implementation

* as a module developer, you should strive to make life as easy as possible for the users of your module, even that means extra work fo ryou
* if you solve the easy problems and punt the hard ones to somebody else, you amplify the complexity so that more people have to deal with the problem rather than just you

### Configuration Parameters

* rather than determining a particular behavior internally, a class can pull in a few parameters that control its behavior
* ask yourself if users are better able to find a value than what you can determine by yourself
    * if you still need a config value, try to infer sane a sane default

### Rule of Thumb

* Pull down complexity if it:
    1. is closely related to the class' existing functionality
    1. will result in many simplifications elsewhere in the applciation
    1. simplifies the class' interface

---

## Chapter 9: Better Together Or Better Apart?

> Given two pieces of functionality, should they be implemented together in the same place, or should their implemtnations be separated?

* the goal is to reduce complexity of the system as a whole and improve its modularity
* beware of dividing system into large number of smaller components
    * more interfaces and every new interface adds complexity
    * additional code to manage components
    * separation between components might make it hard to be aware of existence
    * code duplication
* it is beneficial to bring together pieces of code that are closely related
* indications that two pieces of code are closely related
    * share information
    * used together
    * overlap conceptually (share higher-level category)
    * hard to understand one piece of code without looking at other

### Bring Together: Simplify the Interface

* may be possible to define an interface for the new module that is simpler or easier to use than the origin interfaces.
* may be possible to perform functions automatically so that users are not aware of the difference

### Bring Together: Eliminate Duplication

* reorganize code to eliminate reptition
* this is effective if the repeated snippet is long and the replacement method has a simple signature
* another way to eliminate duplication is to refactor so the snippet is only excited in one place (goto aren't always harmful)

### Separate General-Purpose and Special-Purpose Code

* if a module contains a mechanism that can be used for several different purposes than it should provide one general-purpose method and allow other modules to contain special-purpose code, as required
* in general, lower layers of system tend to be more general-purpose and upper layers more special-purpose
* separate by pulling special-purpose code upwards into higher layers, leaving the lower layers general-purpose

### Splitting and Joining Methods

> When designing methods, the most important goal is to provide clean and simple abstractions

* length by itself is rarely a good reason for splitting up a method as we now have additional interfaces, which add complexity
* shouldn't break up a method unless it makes the overall system simplier
* if method had complex interactions within its blocks, it's even more important to keep them together
* method should be deep with a clean and simple abstraction
* Split a method if it results in a cleaner abstraction:
    * factor out a subtask into a separate method which is invoked by the parent method. Revert if you are jumping back and forth a lot
    * split method into two separate methods with interfaces that are simplier than the interface of the original method

---

## Chapter 10: Define Errors Out of Existence

> Exception handling is one of the worst sources of complexity in software systems

* **exception** - any uncommon condition that alters the normal flow of control in a program

### Why exceptions add complexity

* Ways exceptions can add complexity:
    * caller may provide bad arguemnts or configuration information
    * invoked method may not be able to complete a requested operation
    * in distributed systems, network packets may be lost or delayed, servers may not respond in a teimely fashion, or peers may communicate in unexpected ways
    * code may detect bugs, internal inconsistencies, or situations it is not prepared to handle
* exception handling can account for a significant fraction of all the code in a system
* exception handling code is inherently more difficult to write than normal-case code
* when an exception occurs, programmer can deal with two ways:
    1. move forward and complete the work in progress in spite of the exception
    1. abort the operation in progress and report the exception upwards
        * aborting can result in an inconsistent system state so the exception handling code must restore consistency
* handling exceptions can result in more exceptions
    * secondary exceptions occuring during recovery are often more subtle and complex than the primary exceptions
    * find a way to handle exceptions without introducing more exceptions
* exceptions don't occur very often in running systems, so exception handling code is rarely executed
    * write tests to make sure the code functions the way you think it should

### Too many exceptions

* problems are exacerbated when too many unnecessary exceptions are defined. This results in an over-defensive coding style
* exceptions are a way to punt the problem to the caller
* exceptions are part of your interface, keep them simple
* exceptions also affect multiple layers as they can propogate through several stacks before being caught
* best way to reduce complexity is to reduce the number of places exceptions have to be handled

### Define errors out of existence

* define APIs so there are no exceptions to handle and reduce the amount of code that needs to be written to handle errors

### Mask Exceptions

* when an exceptional condition is detected and handled at a low level in the system so that higher levels of software do not need to be aware of the condition
* doesn't work in all situations, but the reduced interface allows classes to be deeper
* example of pulling complexity downward

### Exception aggregation

* handle many exceptions with a single piece of code, rather than writing distinct handlers for many individual exceptions
* exception handling design pattern
    * if system processes a series of requests, it's useful to define an exception that aborts the current request, cleans up the system's state, and contnues with the next request
    * the exception is caught in a single place near the top of the system's request-handling loop
    * exception can be thrown at any point in the processing of a request to abort the request
    * different subclasses of the exception can be defined for different conditions
* aggregation works best if an exception propagates several levels up the stack before it is handled; results in more exceptions from more methods to be handled in the same place
* form of general-purpose mechanism over special-purpose

### Crash Application

* certain errors are not worth tryign to handle
* print diagnostic information and abort application
* `ckalloc` vs `malloc`

### Design special cases out of existence

* special cases can result in code with lots of if statements which make it hard to understand
* design normal case in a way that automatically handles special case without extra code

### Taking it too far

* defining away exceptions or masking them makes sense if the exception information isn't needed outside the moduel
* with exceptions, we must determine what is important and what is not important. Things that are not important should be hidden, and the more of them the better. If something is important, it must be exposed

---

## Chapter 11: Design it Twice

> designing software is hard, so it's unlikely that your first thought about how to structre a module or system will produce the best design

* you will end up with a much better result if you consider multiple options for each major decison decision
* don't need to pin down every detail, high-level sketch of the important methods is more than enough
* try to pick radically different approaches
* even if there is only one obvious approach, try another one
* after you have sketched out all the alternatives, make a list of pros and cons for each one
* consider
    * which version has the simpler interface?
    * is one interface more general-purpose than another?
    * does one interface enable a more efficient implementation?
* once you have compared the designs, you can make an informed decision
* can apply this principle to many levels of a system
    * goals are different for the interface and implementation so consider the best approach
* this should not take a lot of extra time and it will pay off in the long run as you will not to deal with the extra complexity and cognitive load that poor design brings

### Ego

* smart people feel pressure to get things right the firs ttime
* solving hard problems requires complex solutions where the first design is rarely the best one
