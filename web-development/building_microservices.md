# Building Microservices

By Sam Newman

## Chapter 1: Microservices

> Microservices are small, autonomous services that work together.

* focused on doing one thing well (cohesive)
    * single responsibility principle
* business boundaries define the service boundaries
* how small? small enough that we don't think it's big

* each service is a seprate entity that communicate via network calls
* services need to be able to change independently of each other
* should be able to change independently without requiring consumers to change

> Without decoupling, everything breaks down for us. The golden rule: can you make a change to a service and deploy it by itself without changing anything else? If the answer is no, then many of the advantages we discuss throughout this book will be hard for you to achieve.

### Key Benefits

* We can pick the right tool for the job
* Leads to resilient architeture
    * if one component fails we can isolate the problem while the rest of the system runs
* Can scale individuals parts of the system as required
* Easier deployment story
    * The larger the app, the harder it is to deploy
* Align architecture to our organization
    * smaller teams working on smaller code bases
* Composability of solution delivery
    * we have to hit many platforms with the same information, can we reuse the work we've done?
* Replaceable as needed
    * No big scary monster sitting in the corner because people are too afraid to mess with it

### Service-Oriented Architecture (SOA)

> Service-oriented architecture (SOA) is a design approach where multiple services collaborate to provide some end set of capabilities. A service here typically means a completely separate operating system process. Communication between these services occurs via calls across a network rather than method calls within a process boundary.

* Microservices is just __one way__ to do SOA

### Disadvantages

* complicated
    * distributed systems are hard
* requires specialized expertise

## Chapter 2: The Evolutionary Architect

> For most things we create, we have to accept that once the software gets into the hands of our customers we will have to react and adapt, rather than it being a never-changing artifact. Thus, our architects need to shift their thinking away from creating the perfect end product, and instead focus on helping create a framework in which the right systems can emerge, and continue to grow as we learn more.

> So our architects as town planners need to set direction in broad strokes, and only get involved in being highly specific about implementation detail in limited cases. They need to ensure that the system is fit for purpose now, but also a platform for the future. And they need to ensure that it is a system that makes users and developers equally happy.

* Architect should defining service boundaries and how services can talk to each other
> “be worried about what happens between the boxes, and be liberal in what happens inside”

### A Principled Approach to Building Microservices

1. Understand Organization's Strategic Goals
    * speak to where the company is going and how it wants to make customers happy
    * high-level goals that may not be related to technology
    * key is to build technology to support organization's strategy goals
1. Create a set of architectural "principles" that can help us align with goals
1. Write set of detailed, practical guidance ("practices") for performing tasks to support principles from previous step

### Required Standard

* What is a good citizen service in your system
* ensure all services emit health and general monitoring-related metrics in the same way
* Standardize your interface (verbs/nouns? pagination? versioning?)

* design a template service, but not a template as it as to be used, which incoporates all the good microservice design practies you want people to use which is also configured to your development environment (i.e. logs go where they should, uses preferred testing libraries)
    * Make sure this exemplar is actually used in Prod

> By tailoring such a service template for your own set of development practices, you ensure that teams can get going faster, and also that developers have to go out of their way to make their services badly behaved.

### Core responsibilities of the Evolutionary Architect

* Vision
    * Ensure there is a clearly communicated technical vision for the system that will help your system meet the requirements of your customers and organization
* Empathy
    * Understand the impact of your decisions on your customers and colleagues
* Collaboration
    * Engage with as many of your peers and colleagues as possible to help define, refine, and execute the vision
* Adaptability
    * Make sure that the technical vision changes as your customers or organization requires it
* Autonomy
    * Find the right balance between standardizing and enabling autonomy for your teams
* Governance
    * Ensure that the system being implemented fits the technical vision

## Chapter 3: How to Model Services

### Loose Coupling

> When services are loosely coupled, a change to one service should not require a change to another.

> A loosely coupled service knows as little as it needs to about the services with which it collaborates. This also means we probably want to limit the number of different types of calls from one service to another, because beyond the potential performance problem, chatty communication can lead to tight coupling.

### High Cohesion

> We want related behavior to sit together, and unrelated behavior to sit elsewhere.

### Bounded Context

> The idea is that any given domain consists of multiple bounded contexts, and residing within each are thingsthat do not need to be communicated outside as well as things that are shared externally with other bounded contexts. Each bounded context has an explicit interface, where it decides what models to share with other contexts.

> By thinking clearly about what models should be shared, and not sharing our internal representations, we avoid one of the potential pitfalls that can result in tight coupling. We have also identified a boundary within our domain where all like-minded business capabilities should live, giving us the high cohesion we want. These bounded contexts, then, lend themselves extremely well to being compositional boundaries.

> We have the option of using modules within a process boundary to keep related code together and attempt to reduce the coupling to other modules in the system. When you’re starting out on a new codebase, this is probably a good place to begin. So once you have found your bounded contexts in your domain, make sure they are modeled within your codebase as modules, with shared and hidden models. These module boundaries are excellent candidates for microservices.

* Don't decide on service boundaries too soon, we need to let the monolith build out so we can see how the various components interact.

> When considering the boundaries of your microservices, first think in terms of the larger, coarser-grained contexts, and then subdivide along these nested contexts when you’re looking for the benefits of splitting out these seams.

> Another reason to prefer the nested approach could be to chunk up your architecture to simplify testing. For example, when testing services that consume the warehouse, I don’t have to stub each service inside the warehouse context, just the more coarse-grained API. This can also give you a unit of isolation when considering larger-scoped tests. I may, for example, decide to have end-to-end tests where I launch all services inside the warehouse context, but for all other collaborators I might stub them out.

> It’s also important to think of the communication between these microservices in terms of the same business concepts. The modeling of your software after your business domain shouldn’t stop at the idea of bounded contexts. The same terms and ideas that are shared between parts of your organization should be reflected in your interfaces. It can be useful to think of forms being sent between these microservices, much as forms are sent around an organization.

## Chapter 4: Integration

> With synchronous communication, a call is made to a remote server, which blocks until the operation completes. With asynchronous communication, the caller doesn’t wait for the operation to complete before returning, and may not even care whether or not the operation completes at all.

### REST and HTTP

> HTTP itself defines some useful capabilities that play very well with the REST style. For example, the HTTP verbs (e.g., GET, POST, and PUT) already have well-understood meanings in the HTTP specification as to how they should work with resources. The REST architectural style actually tells us that methods should behave the same way on all resources, and the HTTP specification happens to define a bunch of methods we can use. GET retrieves a resource in an idempotent way, and POST creates a new resource. This means we can avoid lots of different createCustomer or editCustomer methods. Instead, we can simply POST a customer representation to request that the server create a new resource, and initiate a GET request to retrieve a representation of a resource. Conceptually, there is one endpoint in the form of a Customer resource in these cases, and the operations we can carry out upon it are baked into the HTTP protocol.

* HATEOAS (Hypermedia As The Engine Of Application State)
	* look into more

* keep your middleware dumb, and keep the smarts in the endpoints.

### Services as State Machines

* services are fashioned around bounded contexts
* don't make services that are just CRUD wrapper

### Microservices and the DRY Principle

> DRY is what leads us to create code that can be reused. We pull duplicated code into abstractions that we can then call from multiple places. Perhaps we go as far as making a shared library that we can use everywhere! This approach, however, can be deceptively dangerous in a microservice architecture. One of the things we want to avoid at all costs is overly coupling a microservice and consumers such that any small change to the microservice itself can cause unnecessary changes to the consumer. Sometimes, however, the use of shared code can create this very coupling.

* too much coupling is worse than problems caused by code duplication

### Access by Reference

* When we retrieve something from a service, we have a record of the object at that point in time. The resource could have been updated since we got our copy.

> Sometimes this memory is good enough. Other times you need to know if it has changed. So whether you decide to pass around a memory of what an entity once looked like, make sure you also include a reference to the original resource so that the new state can be retrieved.

* Be cognizant of data freshness when we are passing information around

### Versioning

* Defer as long as possible
	* Postel's Law: "Be conservative in what you do, be liberal in what you accept from others"
* Catch breaking changes early
* Semantic versioning
	* MAJOR.MINOR.PATCH
	* major - breaking changes
	* minor - no breaking changes, new functionality added
	* patch - bug fixes made to existing functionality
* Coexist different endpoints
	* deploy new version with breaking changes to a different endpoint
	* expand and contract pattern
		* Expand the capabilities we offer, supporting both old and new ways of doing something. Once the old consumers do things in the new way, we contract our API, removing the old functionality.
* Have multiple concurrent service versions
	* use this sparingly

### User Interfaces

* Constraints
	* think of how your interface is being presented and used

* Backends for Frontend Pattern
<img src="https://www.safaribooksonline.com/library/view/building-microservices/9781491950340/assets/bdms_0410.png" />

### Third Party Software

"Build if it is unique to what you do, and can be considered a strategic asset; buy if your use of the tool isn’t that special."

* Lack of control
* Customization
* Integration Spaghetti
