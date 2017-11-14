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

* 




























