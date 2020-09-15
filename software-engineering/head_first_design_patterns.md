# Head First Design Patterns

By Eric Freeman and Elisabeth Robson

#### Table of Contents

<!-- TOC -->

- [Resources](#resources)
- [Introduction](#introduction)
- [Object-Oriented Principles](#object-oriented-principles)
  - [Abstraction](#abstraction)
  - [Encapsulation](#encapsulation)
  - [Inheritence](#inheritence)
  - [Polymorphism](#polymorphism)
- [Design Principles](#design-principles)
  - [General Notes](#general-notes)
  - [Identify the aspect of your application that vary and separate them from what stays the same](#identify-the-aspect-of-your-application-that-vary-and-separate-them-from-what-stays-the-same)
  - [Program to an interface, not an implementation](#program-to-an-interface-not-an-implementation)
  - [Favor composition over inheritance](#favor-composition-over-inheritance)
  - [Strive for loosely coupled designs between objects](#strive-for-loosely-coupled-designs-between-objects)
  - [Classes should be open for extension, but closed for modification](#classes-should-be-open-for-extension-but-closed-for-modification)
  - [Dependency Inversion Principle](#dependency-inversion-principle)
  - [Principle of Least Knowledge](#principle-of-least-knowledge)
  - [Hollywood Principle](#hollywood-principle)
  - [Single Responsibility Principle](#single-responsibility-principle)
- [Patterns](#patterns)
  - [Types of Patterns](#types-of-patterns)
  - [Strategy Pattern](#strategy-pattern)
  - [Observer Pattern](#observer-pattern)
  - [Decorator Pattern](#decorator-pattern)
  - [Factory Pattern](#factory-pattern)
  - [Abstract Factory](#abstract-factory)
  - [Singleton Pattern](#singleton-pattern)
  - [Command Pattern](#command-pattern)
  - [Adapter Pattern](#adapter-pattern)
  - [Facade Pattern](#facade-pattern)
  - [Template Method Pattern](#template-method-pattern)
  - [Iterator Pattern](#iterator-pattern)
  - [Composite Pattern](#composite-pattern)
  - [State Pattern](#state-pattern)
  - [Proxy Pattern](#proxy-pattern)
  - [Compound Patterns](#compound-patterns)
  - [null Object](#null-object)
  - [Bridge Pattern](#bridge-pattern)
  - [Builder Pattern](#builder-pattern)
  - [Chain of Responsibility](#chain-of-responsibility)
  - [Flyweight Pattern](#flyweight-pattern)
  - [Interpreter Pattern](#interpreter-pattern)
  - [Memento](#memento)
  - [Prototypes](#prototypes)
  - [Visitor](#visitor)
- [Conclusion](#conclusion)

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

## Object-Oriented Principles

### Abstraction
### Encapsulation
### Inheritence
### Polymorphism

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

### Hollywood Principle

> Don't call us, we'll call you

- prevents dependency rot, i.e. when high-level components depend on low-level components depending on high-level components and so on
- allow low-level components to hook themselves into a sytem, but the high-level components determine when they are needed and how
- high-level components give the low-level components a "don't call us, we'll call you" treatment
- technique for building frameworks or components so that lower-level components can be hooked into the computation, but without creating dependencies between the lower-level components and the higher-level layers
- goal is to decouple modules
  - creates designs that allow low-level structures to interoperate while preventing other classes from becoming to dependent on them
- a low-level component will often end up calling a method defined above it in the inheritance hierarchy purely through inheritance
  - avoid creating explicit circular dependencies between the low-level component and the high-level ones

### Single Responsibility Principle

> A class should only have one reason to change

- we know that modifying code provides all sort of opportunities for problems to creep in
  - having two ways to change increases the probability the class will change in the future, and when it does, its going to affect two aspects of your design
- this is not easy as our brains are too good at seeing behaviors and group them together even when there are actually two or more responsibilities
  - the only way to succeed is to be dilligent in examining your design and to watch out for signals that a class is changing in more than one way as your system grows
- **cohesion** is a measure of how closely a class or a module supports a single purpose or responsibility
  - we say that a module or class has high cohesion when it is designed around a set of related functions, and we say it has low cohesion when it is designed around a set of unrelated functions

## Patterns

### Types of Patterns

#### Creational Patterns

> Involve object instantiation and all provide a way to decouple a client from the ojbects it needs to instantiate

- Abstract Factory
- Builder
- Factory Method
- Prototype
- Singleton

#### Behavioral Pattern

> Concerned with how classes and objects interact and distribute responsibility

- Chain of Responsibility
- Command
- Interpreter
- Iterator
- Mediator
- Memento
- State
- Strategy
- Template Method
- Visitor

#### Structural

> Compoose classes or objects into larger structures

- Adapter
- Bridge
- Composite
- Decorator
- Facade
- Flyweight
- Proxy

#### Class

> Describe how relationshps between classes are defined via inheritance. Relationships in class patterns are established at compile time

- Adapter
- Factory Method
- Interpreter
- Template Method

#### Object

> Describe relationships between objects and are primarily defined by composition. Relationships in object patterns are typically created at runtime and are more dynamic and flexible.

- Abstract Factory
- Bridge
- Builder
- Chain of Responsibility
- Command
- Composite
- Decorator
- Facade
- Flyweight
- Iterator
- Mediator
- Memento
- Observer
- Prototype
- Proxy
- Singleton
- State
- Strategy
- Visitor

### Strategy Pattern

- Defines a familiy of algorithms, encapsulates each one, and makes them interchangeable.
- Lets the algorithm vary independently from clients that use it.
- offers clients a choice of algorithm implementation through object composition

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
- do not instantiate objects; just add behavior

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

### Template Method Pattern

> Defines the skeleton of an algorithm in a method, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure

- ensures the algorithm's structure stays unchanged, while subclasses provide some part of the implementation
  - defines the outline of an algorithm, but lets subclasses do some of the work
  - allows for different implemtnations of an algorithm's individual steps, but keep control over the algorithm's structure
- `template method` defines the steps of an algorithm, deferring to subclasses for its implementation of those steps
- `hook method` is declared in the abstract class without a default implementation
  - this gives subclasses the ability to "hook into" the algorithm at various points
  - hooks give the subclass a chance to react to some step in the template method that is about to happen, or just happened
  - provide a subclass with the ability to make a decision for the abstract class
- use abstract methods when your subclass MUST provide an implementation of the method or step in the algorithm
  - use hooks when that part of the algorithm is optional
  - with hooks, a subclass may choose to implement that hook, but it doesn't have to
- leverage the "Hollywood Principle" as the main class has control of the algorithm, but calls on subclasses only when they're needed for an implementation of a method
- this pattern is a great design tool for creating frameworks where the framework controls how something gets done, but leaves you (the person using the framework to specify your own details about what is actually happening at each step of the framework's algorithm)
- provides a fundamental method for code reuse that allows subclasses to specify behavior

### Iterator Pattern

> Provides a way to access the elements of an aggregate object sequentially without exposing its underlying representation

- we can implement iterators for any kind of collection of objects
  - client does not need to know how collections are maintained or implemented, it just needs to use the iterator to step through the items in the menu
  - iterators encapsulate the collection, i.e. it allows the implementation of the interator to live outside of the aggregate
  - we can handle any collection of items polymorphically as long as it implements the Iterator protocol
- gives us a way to step through the elements of an aggregate without forcing the aggregate to clutter its own interface with a bunch of methods to support traversal of its elements
- once you have a uniform way of accessing the elements of all your aggregate objects, you can write polymorphic code that works with any of these methods
- makes the responsiblility of traversing elements and giving that responsibility to the iterator object, not the aggregate object
  - this skeeps the aggregate interface and impleme ntation simpler
  - it removes the responsibility for iteration from the aggregate and keeps the aggregate focused on the things it should be focused on (managing a collection of objects), not on iteration
- iterators do not imply ordering; make no assumption about ordering unless the Collection documentation indicates otherwise
- an *external iterator* must maintain its position in the iteration so that an outside client can drive the iteration by calling `hasNext()` and `next()`
  - our code also needs to maintain that position over a composite, recursive structure; this is why people use stacks
- Can leverage the `Null Object` pattern with a `Null Iterator` that is a "no op"

### Composite Pattern

> Allows you to copose objets into tree structures to represent part-whole hierarchies. Composite lets clients treat individual objects and compositions of objects uniformly

- by putting menus and items in the same structure, we create a part-whole hierarchy; that is, a tree of objects that is made of parts (menus and menu items) but that can be treated as a whole, like our big uber menu
- allows us to build structures of objects in the form of trees that contain both compositions of objects and individual objects as nodes
- use a composite structure, we can apply the same operations over both composites and individual objects; in most cases, we can ignore the differences between compositions of objects and individual objects
- the **Client** uses the **Component** interface to maniuplate the objects in the **Composition**
- the **Component** defines an interface all objects in the **Composition**: both the **Composite** and the **Leaf Nodes**
- a **Leaf Node** defines the behavior for the elements in the **Composition**. It does this by implementing the operations the **Composite** supports
  - a **Leaf Node** has no children
- the **Composite**'s role is to define behavior of the **Components** having children and to store children components
  - the **Composite** implements the **Leaf**-related operations. Note that some of these may not make sense on a **Composite** so in that case an exception might be generated

> We could say that the Composite Pattern takes the Single Responsibility design principle and trades it for *transparency*. What's transparency? Well by allowing the Component interface to contain the child management operations and the leaf operations, a client can treat both componsites and leaf nodes uniformly; so whether an element is a composite or leaf node becomes transparent to the client
>
> Now given we have both types of operations in the Component class, we lose a bit of *safety* because a client might try to do something inappropriate or meaningless on an element (like try to add a menu to a menu item). This is a design decision; we could take the design in the other direction and separate out the responsibliities into interfaces. This would make our design safe, in the sense that any inappropriate calls on elements would be caught at compile time or runtime, but we'd lose transparency and our code would have to use conditionals and the `instanceof` operator
>
> So, to return to your qeustion, this is a classic case of tradeoff. We are guided by design principles, but we always need to observe the effect they have on our designs. Sometimes we purposely do things in a way that seems to violate the principle. In some cases, however, this is a matter of perspective; for instance, it might seem incorrect to have child management operations in the leaf node (like `add()`, `remove()`, and `getChild()`), but then again you can always shift your perspective and see a leaf as a node with zero children.

- use this pattern when you have a collection of objects with whole-part relationships and you want to be able to treat those objects uniformly
- GUI is a good real-world example: you tell the top level component to display and count on that component to display all its parts
- **composite objects** - components that contain other components are called
- **leaf objects** - components that do not contain other components
- in order for the composite to work transparently to the client, you must implement the same interface for all objects in the composite; otherwise, the client has to worry about which interface each object is implementing, which kind of defeats the purpose
  - some clients require separate interfaces for different objects, this is a safer version of the same pattern
- if your composite needs to keep its children in a particular order, then you'll need a sophisticated management scheme for adding and removing children and how you traverse the hierarchy
- composites simplify life for clients; don't have to worry about whether they're dealing with a composite object or a leaf object
  - helps reduce conditionals as we can make one method call and execute an operation over an entire structure

### State Pattern

> Allows an object to alter its behavior when its internal state changes. The object will appear to change its class.

- want to localize the behavior for each state so that if we make changes to one state, we don't mess up other code
  - encapsulate what varies from what stays the same
- define an interface for all states; the methods map directly to actions that could happen to the State Machine
  - take each state in our design and encapsulate it in a class that implements the State interface
  - implement the behaviors that are appropriate for the state we're in
- encapsulate state objects in their own classes and then delegate to the current state when an action occurs
  - this causes behavior to change along with the internal state
- object appears to change class as it has completely changed its behavior
  - we use composition to give the appearance of a class change by simply referencing different state objects
- the **context** is the class that can have a number of internal states
- the **state interface** defines a common interface for all concrete states, the states all implement the same interface so they are interchangeable
- whenever the `request()` is made on the **context**, it is delegated to the state to handle
- **ConcreteStates** handle requests from the **context**. Each ConcreteState provides its own implementation for a request. In this way, when the Context changes state, its behavior will change as well

> With the State Pattern, we have a set of behaviors encapsulated in state objects; at any time the context is delegating to one of those states. Over time, the current state changes across the set of state objects to reflect the internal state of the context, so the context's behavior changes over time as well. The client usually knows very little, if anything, about the state objects.
>
> With Strategy, the client usually specifies the strategy object that the context is composed with. Now, while the pattern provides the flexibility to change the strategy object at runtime, often there is a strategy object that is more appropriate for a context object.
>
> In general, think of the Strategy Pattern as a flexible alternative to subclassing: if you use inheritance to define the behavior of a class, then you're stuck with that behavior even if you need to change it. With Strategy you can change the behavior by composing with a different object.
>
> Think of the State Pattern as an alternative to putting lots of conditions in your context; by encapsulating the behaviors within state objects, you can simply change the state object in context to change its behavior.

- you can either let ConcreteStates decide what state to go to or let the Context decide on the flow of the state transitions
  - when the state transitions are fixed they are appropriate for putting in the Context; however, when the transitions are more dynamic, they are typically placed in the state classes themselves.
  - the disadvantage of having state transitions in the state classes is that we create dependencies between the state classes
  - you have to make a decision as to which classes are closed for modification -- the context or the state classes -- as the system evolves
- clients never interact directly with the states
  - the states are used by the context to represent its internal state and behavior, so all requests to the states come from the Context. Clients don't directly change the state of the Context. It is the Context's job to oversee its state, and you don't usually want a client changing the state of a Context without the Context's knowledge
- you can share state objects across many instnaces of the Context
  - you will assign each state to a static instance variable
  - if your state needs to make use of methods or instance variables in your Context, you'll also have to give it a reference to the Context in each handler() method
- by encapsulating the state behavior into separate state classes, you'll always end up with more classes in your design. That's the price of flexibility
  - alternatively you can have large conditional blocks which are hard to understand and maintain
- state pattern helps classes exhibit different behaviors in different states
  - when created, they have an initial state, but they change their own state over time
  - while the class diagrams are the same, its intent is different than the Strategy Pattern

### Proxy Pattern

> Provides a surrogate or placeholder for another object to control access to it

- creates a representative object that controls access to another object, which may be remote, expensive to create, or in need of securing
- How it Work
  - both the `Proxy` and the `RealSubject` implement the `Subject` interface. This allows any client to treat the proxy just like the `RealSubject`
  - the `RealSubject` is usually the object that does most of the real work; the Proxy controls access to it
  - the `Proxy` often instantiates or handles the creation of the `RealSubject`
  - the `Proxy` keeps a refernece to the `Subject`, so it can forward reqeusts to the `Subject` when necessary
  - can make clients use the `Proxy` rather than the `RealSubject` by providing a factory that instantiates and returns the subject
- there are many variations of the Proxy Pattern -- they all intercept a method invocation that the client is making on the subject
  - this level of indrection allows us to do many things: dispatching requests to a remote server, providing a representative for an expensive object as it is created, or providing some level of protection that can determine which clients should be calling which methods
- similiar to the Decorator Pattern, but it doesn't just add behavior, it also controls access

#### Remote Proxy

- remote proxy acts as a *local representative to a remote object*, i.e. it's an object that you can call local methods on and have them forwarded on to the remote object
- clients can call a method on the client helper, as if the client helper were the actual service; the client helper then takes care of forwarding that request for us
- the client object thinks it's calling a method on the rmote service, because the client helper is pretending to be the service object
- the client helper doesn't have any of the actual method logic the client is expecting; instead, the client helper contracts the server; transfer information about the method call, and waits for a return from the server
- on the server side, the service helper receives the request from the client helper (thru a Socket connect), unpacks the information about the call and then invokes the real method on the real service object; to a service object, the call is local -- it's coming from the service helper, not a remote client
- the service ehelper getse the return value from the service, packts it up, and ships it back (over a Socket's output stream) to the client helper
- the client helper unpacks the information and returns the value to the client object
- networking and I/O methods are risky, can fail, and they throw exceptions all over the place; as a result, the client does have to acknowledge the risk

#### Virtual Proxy

- acts as a representative for an object taht may be expensive to create
- often defers the creation of the object until it is needed
- acts as a surrogate for the object before and while it is being created; after, it delegates requests directly to the `RealSubject`

#### Other Proxy Examples

- **Firewall Proxy** controls access to a set of network resources protecting the subject from "bad" clients
- **Smart Reference Proxy** provides additional actions whenever a subject is referenced, such as counting the number of references to an object
- **Caching Proxy** provides temporary storage for results of operations that are expensive. It can also allow multiple clients to share the results to reduce computation or network latency
- **Synchronization Proxy** provides safe access to a subject from multiple threads
- **Complexity Hiding Proxy** hides the complexity of and controls access to a complex set of classes. This is sometimes called the Facde Proxy for obvious reasons. The Complexity Hiding Proxy differs from the Facade Pattern in that the proxy controls access, while the Facade Pattern just provides an alternative interface
- **Copy-On-Write Proxy** congtrols the copying of an object by deferring the copying of an object until it is requried by a client. This is a variant of the Virtual Proxy

#### Additional Reading

- [Proxy Pattern in Python](https://rednafi.github.io/digressions/python/2020/06/16/python-proxy-pattern.html)

### Compound Patterns

> Combines 2 or more patterns into a solution that solves a recurring or general problem

- patterns are often used together and combined within the same design solution
- a compound pattern combines two or more patterns into a solution that solves a recurring or general problem

#### MVC: Model-View-Controller

- controller separates the logic of control from the view and decouples the view from the model
  - by keeping the view and controller loosely coupled, you are building a more flexible and extensible design, one that can more easily accommodate change down the road
- the model uses the `Observer Pattern` to keep the view and controllers updated on the latest state change
  - model does not know anything about the view or controllers; it is totally decoupled
  - just knows it has observers to notify
  - also provides an interface the views and controllers can use to get and set its state
- the view and controller implement the `Strategy Pattern`
  - view is an object that is configured with a strategy
  - the controller provides the strategy
  - the view is concerned only with the visual aspects of the application and delegates to the controller for any decisions about the interface behavior
- view can use the `Composite Pattern` to update the components of the display
- in the browser model: the view only needs an update of state information when an HTTP response is returned to the browser

### `null` Object

- useful for when you don 't have a meaningful object to return, but what to remove the reponsibility of handling null from the client
- i.e. `NoCommand` object

### Bridge Pattern

> Vary not only our implementations, but also your abstractions

### Builder Pattern

> Encapsulate the construction of a product and allow it to be constructed in steps

### Chain of Responsibility

> Given more than one object a chance to handle a request

- create a chain of objects to examine reqeusts. Each object in turn examines a request and either handles it or passes it on to the next object in the chain

### Flyweight Pattern

> One instance of a class can be used to provide many "virtual instances"

- what if you could redesign your system so that you've got only one instance of Tree and a client object that maintains the state of ALL your trees?

### Interpreter Pattern

> Build an interpreter for a language

- this is good for a simple language
- defines a class-based representation for its grammar along with an interpreter to interpret its sentences
- each class represents each rule in the language

### Memento

> Need to be able to return an object to one of its previous states; for instance, if your user requests an "undo"

- memento has two goals:
  - saving the important state of a system's key object
  - maintaining the key object's encapsulation

### Prototypes

> It is expensive or complicated to create an instance of a class

### Visitor

> Add capabilities to a composite of objects and encapsulation is not important

- works hand in hand with a Traverser
- Traverser knows how to navigate to all of the objects in a Composite
- Traverser guides the Visitor through the Composite so that the Visitor can collect state as it goes
- Once state has been gathered, the Client can have the Visitor perform various operations on the state
- New functionality will requires changes in the Visitor only

## Conclusion

> Center your thinking on design, not on patterns. Use patterns when there is a natural need for them. If something simpler will work, then use that instead.

- when you design, solve things in the simplest way possible
- goal is simplicity, not how can I apply a pattern to the problem
- design patterns aren't a magic bullet
- to use patterns, you also need to think through the consequences for the rest of your design
- if a simple solution might work, give that consideration before you commit to using a pattern
- once you've found a pattern that ppears to be a good match, make sure it has a set of consequences you can live with and study its effect on the rest of your design
- there is one situation in which you'll want to use a pattern even if a simpler solution would work
  - when you expect aspects of your system to vary
  - make sure it's practical change versus hypothetical change
- remove a pattern when the system has become complex and the flexibility you planned for isn't needed
- patterns can introduce complexity and we never want complexity where it is not needed
- Design Patterns often introduce additional classes and objects
  - they increase design complexity
  - add more layers to your design -- inefficency
