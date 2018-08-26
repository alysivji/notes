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
    * **interface** consists of everything that a developer working in a different module must know in order to use the given module
    * **implementation** consists of code that carries out the promises made by the interface
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
    2. Easier to evolve system as design change related to information will only affect one module
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
