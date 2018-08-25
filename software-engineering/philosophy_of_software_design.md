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
