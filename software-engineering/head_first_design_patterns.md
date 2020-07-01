# Head First Design Patterns

By Eric Freeman and Elisabeth Robson

#### Table of Contents

<!-- TOC -->

- [Resources](#resources)
- [Introduction](#introduction)
- [Design Principles](#design-principles)
  - [General Notes](#general-notes)
  - [Identify the aspect of your application that vary and separate them from what stays the same](#identify-the-aspect-of-your-application-that-vary-and-separate-them-from-what-stays-the-same)
  - [Program to an interface, not an implementation](#program-to-an-interface-not-an-implementation)
  - [Favor composition over inheritance](#favor-composition-over-inheritance)
  - [Strive for loosely coupled designs between objects](#strive-for-loosely-coupled-designs-between-objects)
  - [Classes should be open for extension, but closed for modification](#classes-should-be-open-for-extension-but-closed-for-modification)
  - [Dependency Inversion Principle](#dependency-inversion-principle)
  - [Principle of Least Knowledge](#principle-of-least-knowledge)
- [Patterns](#patterns)
  - [Honourable Mentions](#honourable-mentions)
  - [Strategy Pattern](#strategy-pattern)
  - [Observer Pattern](#observer-pattern)
  - [Decorator Pattern](#decorator-pattern)
  - [Factory Pattern](#factory-pattern)
  - [Abstract Factory](#abstract-factory)
  - [Singleton Pattern](#singleton-pattern)
  - [Command Pattern](#command-pattern)
  - [Adapter Pattern](#adapter-pattern)
  - [Facade Pattern](#facade-pattern)

<!-- /TOC -->

## Resources

- [YouTube playlist about book](https://www.youtube.com/playlist?list=PLrhzvIcii6GNjpARdnO4ueTUAVR9eMBpc)

## Introduction

- all patterns provide a way to let some part of your system vary independently of all other parts
- design patterns give you a shared vocabulary with other developers. Once you've got the vocabulary, you can more easily communicate with other develoeprs and inspire those who don't know patterns to start learning them. It also elevates your thinking about architectures by letting you think at the pattern level, not the nitty-gritty object level
- patterns allow you to say more with less. When you use a pattern in a description, other developers quickly know precisely the design you have in mind
- talkign at the pattern level allows you to stay in the design longer; high-level discussions make design meetings easier to understand
- shared vocabularies can turbo-charge your development team
- design patterns don't go directly int your code, they first go into your brain. Once you've got a working knowledge of patterns, you can apply them to new designs and rework old code

## Design Principles

### General Notes

- everything is a suggestion
- if you internalize these principles and have them in the back of your mind when you design, you'll know when you are violating the principle and you'll have a good reason for doing so

### Identify the aspect of your application that vary and separate them from what stays the same

- take parts that vary and encapsulate them so that later you can alter or extend the parts that vary without affecting those that don't

### Program to an interface, not an implementation

- program to a supertype
- when you use `new` you are instantiating a concrete class to an implementation
- by coding to an interface, we can insulate ourselves from a lot of changes that might happen to a ystem down the road
- if your code is written to an interface, then it will work with any new classes implementing that interface through polymorphism

### Favor composition over inheritance

- `HAS-A` is better than `IS-A`
- behavior can be "inherited" at runtime thru composition and delegation
  - inheriting behavior by subclassing means behavior is set statically at compile time
  - inheriting behavior thru composition means behavior is set dynamically at runtime
- composition allows us to add new functionality by writing new code rather than altering existing code
  - prevents us from introducing bugs to existing code

### Strive for loosely coupled designs between objects

- when two objects are loosely coupled, they can interact, but have very little knowledge of each other
- loosely coupled designs allow us to build flexible OO systems that can handle cahnge because they minimize the interdependency between objects
- factory patterns promote loose coupling by reducing the dependency of your application on concrete classes

### Classes should be open for extension, but closed for modification

- our goal is to allow classes to be easily extended to incorporate new behavior without modifying existing code
- this makes our designs resilient to change and flexible enough to take on new functionality to meet changing requirements
- following the Open-Closed Principle usually introduces new levels of abstraction - this increases code complexity
  - apply this principle in parts of code that are most likely to change

### Dependency Inversion Principle

- depend upon abstractions, do not depend upon concrete classes
- high-level components should not depend on low-level components, rather , they should both depend on abstractions
- Guidelines
  - no variable should hold a reference to a concrete class
  - no class should derive from a concrete class
  - no method should override an implemented method of any of its base classes

### Principle of Least Knowledge

> Talk only to your friends

- when you are designing a system, for any object, be careful of the number of classes it interacts with anad also how it comes to interact with those classes
- prevents us from creating designs that have a large number of classes coupled together so that changes in one part of the system cascade to other parts
- when you bulid a lot of dependencies between many classes, you are building a fragile system that will be xcostly to maintain and complex for others to understand
- only invoke methods that belong to:
  - object iself
  - objects passed in as a parameter to the method
  - any object the method creates or instantiates
  - any components of the object

## Patterns

### Honourable Mentions

#### `null` Object

- useful for when you don 't have a meaningful object to return, but what to remove the reponsibility of handling null from the client
- i.e. `NoCommand` object

### Strategy Pattern

- Defines a familiy of algorithms, encapsulates each one, and makes them interchangeable.
- Lets the algorithm vary independently from clients that use it.

### Observer Pattern

- like a newspaper subscription
- defines a one-to-many dependency between objects so that when one object changes state, all of its dependents are notified and updated automatically
- sub and observers define the one-to-many relationship
  - observers are dependent on the subject such that when the subject's tate changes, the observers get notified
  - depending on the style of notirication, the observer may also be updated with new values
- because the subject is the sole owner of the data, the observers are dependent on the subject to update them when the date changes
  - this leads to cleaner OO design than allowing many objects to control the same data
- the only thing the subject knows about an observer is that it imiplements a certain interface
- we can add new interfaces at anytime
- we never need to modify the subject to add new types of observers
- we can reuse subjects or observers independently of each other
- changes to either the subject or an observer will not affect the other

### Decorator Pattern

- attaches additional responsibilities to an object dynamically
- decorators provide a flexible alternative to subclassing for extending functionality
- decorators use inheritance to achieve type matching
- behavior comes through the composition of decorators with the base components as well as other decorators
- with composition, we can mix and match decorators any way we like... at runtime
- decorators are typically created by using other patterns like `Factory` and `Builder`
- can think of `Decorator Pattern` as recursion
  - decorator classes are recursive case
  - base class is base case

#### Drawbacks

- can add lots of small classes to a design and this can result in a design that's less than straightforward for others to understand
- can usually insert decorators transparently and the client never has to know it's dealing with a decorator
  - if code is dependent on a specific type, bad things can happen
- can increase the complexity of the code needed to instantiate the component
  - have to instantiate the component and wrap it with however many decorators as needed

#### Additional Notes

- decorators have the same supertype as the objects they decorate
- you can use one or more decorators to wrap an object
- given the decorator object has the same supertype as the object it decorates, we can pass around a decorated object in place of the original (wrapped) object
- decorator adds its own behavior either before and/or after delegating to the object it decorates to do the rest of the job
- objects can be decorated at any time, so we can decorate objects dynamically at runtime with as many decorators as we like

### Factory Pattern

> Defines an interface for creating an object, but lets subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses

- create objects through inheritance
- move creation code to another object that is only going to be concerned with create objects
- encapsulates object creation into one class
  - we have only one place to make modifications
- clients depend on interface versus implementation
- one of the most powerful techniques for adhering to the Dependency Inversion Principle
- use to decouple client code from concrte classes you need to instantiate, or if you don't know ahead of time all the concrete classes you are going to need
- relies on inheritance: object creation is delegated to factory method

### Abstract Factory

> Provides an interface for creating families of related or dependent objects without specifying their concrete classes

- framework to encapsulate product knowledge into each creation (composition > inheritence)
- has an **abstract creator class** that defines an abstract method that subclasses implement to produce products
- encapsulates object creation
  - we have only one place to make modifications
- clients depend on interface versus implementation
- gives us an interface for creating a family of products
- by writing code that uses an interface, we decouple our code from the actual factory that creates the products
- we can implement a variety of factories that produce products meant for different contexts (users, regions, systems, different look / feel)
- often methods of an `Abstract Factory` are implemented as `Factory Methods`
- provide an abstract type for creating a family of products
  - subclasses of this type define how the products are produced
  - to use, you inistantiate one and pass it into some code written against the abstract type
- allows a client to use an abstract interface to create a set of related products without knowing (or caring) about the concrete products that are actually produced
  - decouple client from any of the specifics of the concrete products
- changing interface means you ahve to change the interface for every subclass
- use when clients need to create products that belong together
- relies on object composition: object creation is implemented in methods exposed in the factory interface

### Singleton Pattern

> Ensures a class has only one instance, and provides a global point of access to it

- convention for ensuring one and only one object is instantiated for a given class
- gives us a global point of access, just like a global variable
- create objects only when they are needed
- contain registry settings, manage pools of resources (connections, threads)
- can assure that every object in our application is making use of the same global resource
- class is managing a single instance of itself
  - prevent other classes from creating a new instance
  - call the `.getInstance()` method to create / get the class itself
    - initialize Singleton in a lazy manner
- Singleton is not only responsible for managing its one instance (and providing global access), it is also responsible for whatever its main role is in your application
- like most patterns, Singleton is not necessarily meant to be a solution that can fit into a library
- this pattern is trivial to add to any existing class
- should be used sparingly
  - large numbers of Singletons means we may have done something wrong

#### To Read

- https://stackoverflow.com/a/2085988
- https://stackoverflow.com/questions/6760685/creating-a-singleton-in-python

### Command Pattern

> Encapsulates a request as an object, thereby letting you parameterize other objects with different requests, queue or log reports, and support undoable operations

- by encapsulating method invocation, we can crystalize pieces of computation so that the object invoking the computation doesn't need to worry about how to do things, it just uses our crystallized method to get it done
- if we store encapsulated method invocations, we can add interested abilities to our program (undo)
- allows us to deccouple the requester of an action from the object that actually performs the action
- receiver of the request gets bound to the command it encapsulates
- strive for "dumb" command objects that just invoke an action on a receiver
  - can have "smart" command objects that can implement logic to carry out a request (reduces decoupling)
- command is a packaged-up computation and pass it around like a first-class object
  - provides a way to have a common interface to the behavior of many different receivers
- based upon Dependency Injection
- allows us to set how how invokers and receiver are bound to each other at runtime versus compile-time

#### Client

- responsible for creating commannd object
- client performs a `.setCommand()` to store the command object in the invoker
- can create command objects and invoke the computation on them at some later datetime

#### Command Object

- encapsulates a request by binding together a set of actions on a specific receiver
  - packages actions and receiver up into an object that exposes the `.execute()` method
- all commands implement the same interface, typically `.execute()`
- `.execute()` method that encapsulates the action and can be called to invoke the action on the receiver

#### Invoker

- holds command, which is set via `.setCommand()`
- calls `Command.execute()` to invoke actions on the Receiver

#### Receiver

- performs actions when `Client.execute()` is called
- the set of objects we want to decouple from the invoker

#### To Read

- https://stackoverflow.com/questions/1494442/general-command-pattern-and-command-dispatch-pattern-in-python
- https://olvlvl.com/2018-04-command-dispatcher-pattern

#### Things I'm pondering

- `EventEmitter` dispatch is very similar to command pattern
  - yes, it is a python implementation
- what is the difference between the command pattern and the dispatch command pattern?

### Adapter Pattern

> Converts the interface of a class into another interface the clients expect. Adaptee lets classes work together that couldn't otherwise because of incompatible interfaces.

- job of implementing an adapter is proportional to the size of the interface you need to support as your target interface
- adapter acts to decouple the client from the implemented interface
- if we expect the itnerface to change over time, the adapter encapsulates the change so that the client doesn't have to be modified each time it needs to operate against a different interface
- clients do not know the adapter is sitting in front of the adaptee
- can use new libraries and subsets without changing any code
- `intent` alter interface so that it matches one client is expected

### Facade Pattern

> Provides a unified interface to a set of interfaces in a subsytem. Facade defines a higher-level itnerface that makes the subsystem easier to use.

- simplify the interface
- hide all the complexity of one or more modules behind a well-lit facade
- take a complex subsystem and make it easier to use by implementing a Facade class that provides provides a more reasonable interface
- the underlying subsystem is availble if you need low level access
- facades don't encapsulate the subsystem classes, they merely provide a simplified interface to their functinality
- subsystem classes remain available to direct use by client that need to use more specific interfaces
- facade is feel to add its own "smarts"
- intent of the adapter p
- `intent` provide a simplified interface to a subsystem
- allows us to avoid tight coupling between clients and subsytems
