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

---

## Chapter 2: The Evolutionary Architect

> For most things we create, we have to accept that once the software gets into the hands of our customers we will have to react and adapt, rather than it being a never-changing artifact. Thus, our architects need to shift their thinking away from creating the perfect end product, and instead focus on helping create a framework in which the right systems can emerge, and continue to grow as we learn more.
>
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

---

## Chapter 3: How to Model Services

### Loose Coupling

> When services are loosely coupled, a change to one service should not require a change to another.
>
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

---

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

---

## Chapter 5: Splitting the Monolith

> The problem with the monolith is that all too often it is the opposite of both. Rather than tend toward cohesion, and keep things together that tend to change together, we acquire and stick together all sorts of unrelated code. Likewise, loose coupling doesn’t really exist: if I want to make a change to a line of code, I may be able to do that easily enough, but I cannot deploy that change without potentially impacting much of the rest of the monolith, and I’ll certainly have to redeploy the entire system.

* __seam__ - portion of the code that can be treated in isolation and worked on without impacting the rest of the codebase
    * Make seams into service boundaries
        * find bounded contexts (boom, all coming together)
* incremental approach is best (limit impact of things going wrong)

### Refactoring Databases

* When dealing with database seams where one table has a ForeignKey relationship with another table, we can expose the data via an API call
* Shared static data (countries, states) should be kept in code or configuration files
* Shared mutable data when two different bounded contexts have access to read from and write to the same table, make the model more concrete and create a bounded context around it that can be exposed via API
    * can also split tables

#### Best to refactor database in stages

1. Monolith service + monolith schema
1. Monolith service + split schema
    * increased number of `SELECT` calls, in-memory `JOIN`s
    * keep it in this stage for some time to see what happens, can revert if it doesn't work out
1. Split services + split schema

<img src="https://www.safaribooksonline.com/library/view/building-microservices/9781491950340/assets/bdms_0509.png" />

#### Transactional Boundaries

> Transactions are useful things. They allow us to say these events either all happen together, or none of them happen. They are very useful when we’re inserting data into a database; they let us update multiple tables at once, knowing that if anything fails, everything gets rolled back, ensuring our data doesn’t get into an inconsistent state. Simply put, a transaction allows us to group together multiple different activities that take our system from one consistent state to another—everything works, or nothing changes.

> With a monolithic schema, all our create or updates will probably be done within a single transactional boundary. When we split apart our databases, we lose the safety afforded to us by having a single transaction.

If transaction is capture in one place, but not the other:

* we can try reinserting it into the second table at a later time via a task queue (eventual consistency)
* abort entire transaction, might need to unwind operation via a compensating transaction (can also clean up later via batch job)
* distributed transactions which are governed by a transaction manager that orchestrates various transactions being done by underlying systems (two-phase commit: voting phase where a single no sends rollback to all parties)
    * lots of things can go wrong here (locks, transaction manager going down, etc)

### Reporting

* We can't break all workflows so we'll need to compromise with the people using the reporting database
* Have data pushed to reporting system
* Push to pub-sub where services that care can pull events and put them in the right location
    * Kafka or RabbitMQ message streams

---

## Chapter 6: Deployment

### Continuous Integration (CI)

> CI, the core goal is to keep everyone in sync with each other, which we achieve by making sure that newly checked-in code properly integrates with existing code. To do this, a CI server detects that the code has been committed, checks it out, and carries out some verification like making sure the code compiles and that tests pass.

* single CI build per microservice allows us to quickly make and deploy the change into production without affecting other services

### Continuous Delivery (CD)

* create a build pipeline for different stages of the build process; one stage for the faster tests, one for the slower tests.

> In CD, we do this by extending the idea of the multistage build pipeline to model each and every stage our software has to go through, both manual and automated.

<img src="https://www.safaribooksonline.com/library/view/building-microservices/9781491950340/assets/bdms_0604.png" />

> As a version of our code moves through the pipeline, if it passes one of these automated verification steps it moves to the next stage. Other stages may be manual. For example, if we have a manual user acceptance testing (UAT) process I should be able to use a CD tool to model it. I can see the next available build ready to be deployed into our UAT environment, deploy it, and if it passes our manual checks, mark that stage as being successful so it can move to the next.

> In a microservices world, where we want to ensure we can release our services independently of each other, it follows that as with CI, we’ll want one pipeline per service. In our pipelines, it is an artifact that we want to create and move through our path to production. As always, it turns out our artifacts can come in lots of sizes and shapes. We’ll look at some of the most common options available to us in a moment.

* Material is a little dated, but basically have a setup so you can push and have things build and deploy for you
    * Make servers immutable and disable SSH
        * Only way to change is by deploying a new image 
    Kubernetes brah

> As you move from your laptop to build server to UAT environment all the way to production, you’ll want to ensure that your environments are more and more production-like to catch any problems associated with these environmental differences sooner. This will be a constant balance. Sometimes the time and cost to reproduce production-like environments can be prohibitive, so you have to make compromises.

* Configuring services, properties file for each environment / different parameter passed into install process

---

## Chapter 7: Testing

### Types of Tests

<img src="https://www.safaribooksonline.com/library/view/building-microservices/9781491950340/assets/bdms_0701.png" />
(credit Brian Marick’s testing quadrant)

* __technology-facing__ - tests that aid the developers in creating a systemm

### Testing Pyramid

* __unit test__ - tests that typically test a single function or method cal. Lots of these. Should catch our mistakes as we refactor
* __service tests__ - tests that go just underneath the user interface and test services directly. Test a single service by itself, stub out external collaborators so we are only testing the service in isolation
* __end-to-end tests__ - aka functional tests. Tests run against the entire system that can be automated thru a framework like Selenium

* Be aware of tradeoffs:
     * As we go up the pyramid, test scope increases but so does test time and complexity so if something breaks we might not be sure where
     * Going down the pyramid means we are testing less, but tests are faster and we can pinpoint exactly where something broke (if our unit tests are good)

* If we find a bug, fix it, and write a unit test to confirm so we can make sure we never regress

### Implementing Service Tests

> Our service tests want to test a slice of functionality across the whole service, but to isolate ourselves from other services we need to find some way to stub out all of our collaborators.

> Our service test suite needs to launch stub services for any downstream collaborators (or ensure they are running), and configure the service under test to connect to the stub services. We then need to configure the stubs to send responses back to mimic the real-world services. For example, we might configure the stub for the loyalty points bank to return known points balances for certain customers.

* stub just returns a known quanity, doesn't care how many times it's called
* mock returns a value and we can check to see how it was invoked
    * leads to more brittle tests

* stub service (Mountebank) which listens for requests and sends back information it was programmed to send back
    * can also use as a mock

### Implementing End-to-End Tests

> To implement an end-to-end test we need to deploy multiple services together, then run a test against all of them. Obviously, this test has much more scope, resulting in more confidence that our system works! On the other hand, these tests are liable to be slower and make it harder to diagnose failure.

* get rid of flaky tests, tests shouldn't sometimes work. Anti-pattern
* test suite should run fast or nobody will run tests

* Microservices require a shift in thinking, we want small changes we can deploy quickly; so make sure tests are fast and the end-to-end pipeline doesn't get jammed

* (Very) Small number of core "journeys" (not stories, but stories... consultant speak) that test the whole system; if functionality is not covered in the above tests, we should do service level tests in isolation to ensure that piece is covered

### Consumer-Driven Contract (CDC)

* do not need to test against real service

> With CDCs, we are defining the expectations of a consumer on a service (or producer). The expectations of the consumers are captured in code form as tests, which are then run against the producer. If done right, these CDCs should be run as part of the CI build of the producer, ensuring that it never gets deployed if it breaks one of these contracts. Very importantly from a test feedback point of view, these tests need to be run only against a single producer in isolation, so can be faster and more reliable than the end-to-end tests they might replace.

> As an example, let’s revisit our customer service scenario. The customer service has two separate consumers: the helpdesk and web shop. Both these consuming services have expectations for how the customer service will behave. In this example, you create two sets of tests: one for each consumer representing the helpdesk’s and web shop’s use of the customer service. A good practice here is to have someone from the producer and consumer teams collaborate on creating the tests, so perhaps people from the web shop and helpdesk teams pair with people from the customer service team.

> Because these CDCs are expectations on how the customer service should behave, they can be run against the customer service by itself with any of its downstream dependencies stubbed out. From a scope point of view, they sit at the same level in the test pyramid as service tests, albeit with a very different focus. These tests are focused on how a consumer will use the service, and the trigger if they break is very different when compared with service tests. If one of these CDCs breaks during a build of the customer service, it becomes obvious which consumer would be impacted. At this point, you can either fix the problem or else start the discussion about introducing a breaking change in the manner

* Use CDCs to identify breaking change without having to run a slow and expensive end-to-end test
* microservices testing has moved from end-to-end to something like CDC

* to catch bugs which happen in production and write tests against them to make sure they don't run again, we can create a _smoke test suite_ which will run against newly deployed software to make sure the deployment worked as expected

* testing closer to production
    * blue / green deployment is what k8s takes care of
    * canary releasing allows you to A/B test your deployment only send a portion to the new site, we can fully rollout into production if we meet certain metrics

* microservices mean more calls across the network so we should also look into _peformance testing_ (aka load testing) to make sure we are hitting the latency and responsiveness metrics we are aiming for

---

## Chapter 8: Monitoring

> We now have multiple servers to monitor, multiple logfiles to sift through, and multiple places where network latency could cause problems.

> The answer here is pretty straightforward: monitor the small things, and use aggregation to see the bigger picture.

> The secret to knowing when to panic and when to relax is to gather metrics about how your system behaves over a long-enough period of time that clear patterns emerge.

* collect metrics that make sense and can help us make decisions down the line, maybe we'll look at how many times a service was used; if it's not used we can remove it or if it's being used a lot, we can improve it
* use synthetic transactions to conduct semantic monitoring ("uses tests to continuously evaluate your application, combining test-execution and realtime monitoring") to make sure things are behaving in a manner that is expected
    * use end-to-end tests as a basis for implementing semantic monitoring

* generate UUID for the first call and pass this information in all calls so we can trace messages in the logs
    * can we attach with middleware, propagate uuid in headers?

* monitor integration points between systems, each service instance should track and expose the health of its downstream dependencies

* standardizing monitoring across all services is important (standard format, naming convention, metrics, etc)
    * when designing monitoring features, figure out what kind of data is needed right now, what kind of data is needed later, and how data will be consumed (alerts, formats, etc)

> Many organizations are moving in a fundamentally different direction: away from having specialized tool chains for different types of metrics and toward more generic event routing systems capable of significant scale. These systems manage to provide much more flexibility, while at the same time actually simplifying our architecture.

---

## Chapter 9: Security

> We need to think about what protection our data needs while in transit from one point to another, and what protection it needs at rest. We need to think about the security of our underlying operating systems, and our networks too.

* __authentication__ is the process by which we confirm that a party (principal) is who they say they are
* __authorization__ is the mechanism by which we map from a principal to the action we are alloweding them to do
    * Based on authentication, we can decide what they can and can't do

> When a principal tries to access a resource (like a web-based interface), she is directed to authenticate with an identity provider. This may ask her to provide a username and password, or might use something more advanced like two-factor authentication. Once the identity provider is satisfied that the principal has been authenticated, it gives information to the service provider, allowing it to decide whether to grant her access to the resource.

> This identity provider could be an externally hosted system, or something inside your own organization. Google, for example, provides an OpenID Connect identity provider. For enterprises, though, it is common to have your own identity provider, which may be linked to your company’s directory service.

* this seems like a good thing to include as part of middleware or if we have externally facing services, use a gateway to act as a proxy and populate headers for downstream services

> Instead, favor coarse-grained roles, modeled around how your organization works. Going all the way back to the early chapters, remember that we are building software to match how our organization works. So use your roles in this way too.

### Service-to-Service Authentication and Authorization

* __Allow Everything Inside the Perimeter__ - Our first option could be to just assume that any calls to a service made from inside our perimeter are implicitly trusted.
* __HTTP(S) Basic Authentication__ - HTTP Basic Authentication allows for a client to send a username and password in a standard HTTP header. The server can then check these details and confirm that the client is allowed to access the service. Since it's all plaintext, we should only do this over HTTPS
* __SAML or OpenID Connect__
* __Client Certificates__
* __HMAC Over HTTP__
* __API Keys__

### Confused Deputy Problem

> There is a type of vulnerability called the confused deputy problem, which in the context of service-to-service communication refers to a situation where a malicious party can trick a deputy service into making calls to a downstream service on his behalf that he shouldn’t be able to. For example, as a customer, when I log in to the online shopping system, I can see my account details. What if I could trick the online shopping UI into making a request for someone else’s details, maybe by making a call with my logged-in credentials?

### Security tips

* Use common sense

> If there is nothing else you take away from this chapter, let it be this: don’t write your own crypto. Don’t invent your own security protocols. Unless you are a cryptographic expert with years of experience, if you try inventing your own encoding or elaborate cryptographic protections, you will get it wrong. And even if you are a cryptographic expert, you may still get it wrong.

> Many of the tools previously outlined, like AES, are industry-hardened technologies whose underlying algorithms have been peer reviewed, and whose software implementation has been rigorously tested and patched over many years. They are good enough! Reinventing the wheel in many cases is often just a waste of time, but when it comes to security it can be outright dangerous.

---

## Chapter 10: Conway’s Law and System Design

> Conway's Law: "Any organization that designs a system (defined more broadly here than just information systems) will inevitably produce a design whose structure is a copy of the organization’s communication structure."

* Good read about programming team issues and how to handle. TBH, this stuff isn't that useful for me at the moment so I'm skimming it to get the book done.

---

## Chapter 11: Microservices at Scale

> Even for those of us not thinking at extreme scale, if we can embrace the possibility of failure we will be better off. For example, if we can handle the failure of a service gracefully, then it follows that we can also do in-place upgrades of a service, as a planned outage is much easier to deal with than an unplanned one.

> Knowing how much failure you can tolerate, or how fast your system needs to be, is driven by the users of your system. That in turn will help you understand which techniques will make the most sense for you. That said, your users won’t always be able to articulate what the exact requirements are. So you need to ask questions to help extract the right information, and help them understand the relative costs of providing different levels of service.

* When you are trying to scale out systems, understand:
    * Response time/latency - how long should something take to do?
    * Availability - can you expect a service to be down?
    * Durability of data - how much data loss is acceptable? How long should we keep data for?

> What we need to do is understand the impact of each outage, and work out how to properly degrade functionality. If the shopping cart service is unavailable, we’re probably in a lot of trouble, but we could still show the web page with the listing. Perhaps we just hide the shopping cart or replace it with an icon saying “Be Back Soon!”

> When you get down to it, we discovered the hard way that systems that just act slow are much harder to deal with than systems that just fail fast. In a distributed system, latency kills.

* Timeouts - how long can I wait before I consider the downsteram service to be down
* Circuit breaker - after a threshold of requests to the downstream resource have failed, the breaker is blown and all further requests fail. Health checks are sent to see if the system has recovered
* Bulkhead - allow us to isolate yourself from failure
    * circuit breaker can trigger the sealing of a bulkhead

* Isolation means that services can do their job without depending on other services being up or not
* Idempotent operations mean we can replay the tape and not have anything change

> There is a danger that people will see the need to rearchitect when certain scaling thresholds are reached as a reason to build for massive scale from the beginning. This can be disastrous. At the start of a new project, we often don’t know exactly what we want to build, nor do we know if it will be successful. We need to be able to rapidly experiment, and understand what capabilities we need to build. If we tried building for massive scale up front, we’d end up front-loading a huge amount of work to prepare for load that may never come, while diverting effort away from more important activities, like understanding if anyone will want to actually use our product.

### Scaling Databases

* Availability of Service Versus Durability of Data
* Scaling for reads (read replicas, eventual consistency)
    * caching might be faster
* Scaling for writes (sharding)
    > With sharding, you have multiple database nodes. You take a piece of data to be written, apply some hashing function to the key of the data, and based on the result of the function learn where to send the data
    * complexity with sharding for writes comes from handling queries that span multiple nodes 

#### Command-Query Responsibility Segregation (CQRS)

* Alternate model to CRUD

> The Command-Query Responsibility Segregation (CQRS) pattern refers to an alternate model for storing and querying information. With normal databases, we use one system for performing modifications to data and querying the data. With CQRS, part of the system deals with commands, which capture requests to modify state, while another part of the system deals with queries.
>
> Commands come in requesting changes in state. These commands are validated, and if they work, they will be applied to the model. Commands should contain information about their intent. They can be processed synchronously or asynchronously, allowing for different models to handle scaling; we could, for example, just queue up inbound requests and process them later.
>
> In many ways, we get the same benefits of read replicas that we discussed earlier, without the requirement that the backing store for the replicas be the same as the data store used to handle data modifications.

### Caching

> Caching is a commonly used performance optimization whereby the previous result of some operation is stored, so that subsequent requests can use this stored value rather than spending time and resources recalculating the value. More often than not, caching is about eliminating needless round-trips to databases or other services to serve results faster. Used well, it can yield huge performance benefits. The reason that HTTP scales so well in handling large numbers of requests is that the concept of caching is built in.

#### Client-Side, Proxy, and Server-Side Caching

> In client-side caching, the client stores the cached result. The client gets to decide when (and if) it goes and retrieves a fresh copy. Ideally, the downstream service will provide hints to help the client understand what to do with the response, so it knows when and if to make a new request. With proxy caching, a proxy is placed between the client and the server. A great example of this is using a reverse proxy or content delivery network (CDN). With server-side caching, the server handles caching responsibility, perhaps making use of a system like Redis or Memcache, or even a simple in-memory cache.
>
> Which one makes the most sense depends on what you are trying to optimize. Client-side caching can help reduce network calls drastically, and can be one of the fastest ways of reducing load on a downstream service. In this case, the client is in charge of the caching behavior, and if you want to make changes to how caching is done, rolling out changes to a number of consumers could be difficult. Invalidation of stale data can also be trickier, although we’ll discuss some coping mechanisms for this in a moment.
>
> With proxy caching, everything is opaque to both the client and server. This is often a very simple way to add caching to an existing system. If the proxy is designed to cache generic traffic, it can also cache more than one service; a common example is a reverse proxy like Squid or Varnish, which can cache any HTTP traffic. Having a proxy between the client and server does introduce additional network hops, although in my experience it is very rare that this causes problems, as the performance optimizations resulting from the caching itself outweigh any additional network costs.
>
> With server-side caching, everything is opaque to the clients; they don’t need to worry about anything. With a cache near or inside a service boundary, it can be easier to reason about things like invalidation of data, or track and optimize cache hits. In a situation where you have multiple types of clients, a server-side cache could be the fastest way to improve performance.

* in HTTP we can use `cache-control` in responses to tell clients how to cache and for how long in seconds
* `Expires` header specifies how long we should hold onto the resource
* Conditional HTTP verbs

> Be careful about caching in too many places! The more caches between you and the source of fresh data, the more stale the data can be, and the harder it can be to determine the freshness of the data that a client eventually sees. This can be especially problematic with a microservice architecture where you have multiple services involved in a call chain. Again, the more caching you have, the harder it will be to assess the freshness of any piece of data. So if you think a cache is a good idea, keep it simple, stick to one, and think carefully before adding more!
>
> Those pages with Expires: Never stuck in the caches of many of our users, and would never be invalidated until the cache became full or the user cleaned them out manually. Clearly we couldn’t make either thing happen; our only option was to change the URLs of these pages so they were refetched.

### CAP Theorem

It is impossible for a distributed data store to simultaneously provide more than two out of the following three guarantees:
* Consistency (do we get the same answer if we go to multiple nodes)
* Availability (every request receives a response)
* Partition tolerance (communicate between over the network)

* Cassandra is interesting, explore later

### Service Discovery

> All of the solutions we’ll look at handle things in two parts. First, they provide some mechanism for an instance to register itself and say, “I’m here!” Second, they provide a way to find the service once it’s registered. Service discovery gets more complicated, though, when we are considering an environment where we are constantly destroying and deploying new instances of services. Ideally, we’d want whatever solution we pick to cope with this.

* DNS
* Dynamic Service Registries (DSR)

### Documentation

* There are a few different ways to do this, look into when we get deeper into the game

---

## Chapter 12: Bringing It All Together

### Principles of Microservices

<img src="https://www.safaribooksonline.com/library/view/building-microservices/9781491950340/assets/bdms_1201.png" />

#### Model Around Business Concepts

> Experience has shown us that interfaces structured around business-bounded contexts are more stable than those structured around technical concepts. By modeling the domain in which our system operates, not only do we attempt to form more stable interfaces, but we also ensure that we are better able to reflect changes in business processes easily. Use bounded contexts to define potential domain boundaries.

#### Adopt a Culture of Automation

> Microservices add a lot of complexity, a key part of which comes from the sheer number of moving parts we have to deal with. Embracing a culture of automation is one key way to address this, and front-loading effort to create the tooling to support microservices can make a lot of sense. Automated testing is essential, as ensuring our services still work is a more complex process than with monolithic systems. Having a uniform command-line call to deploy the same way everywhere can help, and this can be a key part of adopting continuous delivery to give us fast feedback on the production quality of each check-in.
> 
> Consider using environment definitions to help you specify the differences from one environment to another, without sacrificing the ability to use a uniform deployment method. Think about creating custom images to speed up deployment, and embracing the creation of fully automated immutable servers to make it easier to reason about your systems.

#### Hide Internal Implementation Details

> To maximize the ability of one service to evolve independently of any others, it is vital that we hide implementation details. Modeling bounded contexts can help, as this helps us focus on those models that should be shared, and those that should be hidden. Services should also hide their databases to avoid falling into one of the most common sorts of coupling that can appear in traditional service-oriented architectures, and use data pumps or event data pumps to consolidate data across multiple services for reporting purposes.
>
> Where possible, pick technology-agnostic APIs to give you freedom to use different technology stacks. Consider using REST, which formalizes the separation of internal and external implementation details, although even if using remote procedure calls (RPCs), you can still embrace these ideas.

#### Decentralize All the Things

> To maximize the autonomy that microservices make possible, we need to constantly be looking for the chance to delegate decision making and control to the teams that own the services themselves. This process starts with embracing self-service wherever possible, allowing people to deploy software on demand, making development and testing as easy as possible, and avoiding the need for separate teams to perform these activities.
>
> Ensuring that teams own their services is an important step on this journey, making teams responsible for the changes that are made, ideally even having them decide when to release those changes. Making use of internal open source ensures that people can make changes on services owned by other teams, although remember that this requires work to implement. Align teams to the organization to ensure that Conway’s law works for you, and help your team become domain experts in the business-focused services they are creating. Where some overarching guidance is needed, try to embrace a shared governance model where people from each team collectively share responsibility for evolving the technical vision of the system.
>
> This principle can apply to architecture too. Avoid approaches like enterprise service bus or orchestration systems, which can lead to centralization of business logic and dumb services. Instead, prefer choreography over orchestration and dumb middleware, with smart endpoints to ensure that you keep associated logic and data within service boundaries, helping keep things cohesive.

#### Independently Deployable
> We should always strive to ensure that our microservices can and are deployed by themselves. Even when breaking changes are required, we should seek to coexist versioned endpoints to allow our consumers to change over time. This allows us to optimize for speed of release of new features, as well as increasing the autonomy of the teams owning these microservices by ensuring that they don’t have to constantly orchestrate their deployments. When using RPC-based integration, avoid tightly bound client/server stub generation such as that promoted by Java RMI.
>
> By adopting a one-service-per-host model, you reduce side effects that could cause deploying one service to impact another unrelated service. Consider using blue/green or canary release techniques to separate deployment from release, reducing the risk of a release going wrong. Use consumer-driven contracts to catch breaking changes before they happen.
>
> Remember that it should be the norm, not the exception, that you can make a change to a single service and release it into production, without having to deploy any other services in lock-step. Your consumers should decide when they update themselves, and you need to accommodate this.

#### Isolate Failure

> A microservice architecture can be more resilient than a monolithic system, but only if we understand and plan for failures in part of our system. If we don’t account for the fact that a downstream call can and will fail, our systems might suffer catastrophic cascading failure, and we could find ourselves with a system that is much more fragile than before.
>
> When using network calls, don’t treat remote calls like local calls, as this will hide different sorts of failure mode. So make sure if you’re using client libraries that the abstraction of the remote call doesn’t go too far.
>
> If we hold the tenets of antifragility in mind, and expect failure will occur anywhere and everywhere, we are on the right track. Make sure your timeouts are set appropriately. Understand when and how to use bulkheads and circuit breakers to limit the fallout of a failing component. Understand what the customer-facing impact will be if only one part of the system is misbehaving. Know what the implications of a network partition might be, and whether sacrificing availability or consistency in a given situation is the right call.

#### Highly Observable

> We cannot rely on observing the behavior of a single service instance or the status of a single machine to see if the system is functioning correctly. Instead, we need a joined-up view of what is happening. Use semantic monitoring to see if your system is behaving correctly, by injecting synthetic transactions into your system to simulate real-user behavior. Aggregate your logs, and aggregate your stats, so that when you see a problem you can drill down to the source. And when it comes to reproducing nasty issues or just seeing how your system is interacting in production, use correlation IDs to allow you to trace calls through the system.

