# Microservices

## Microservices in Python talk

### Lifecycle of a Microservice

* On startup, the microservice registers with the service register, or is "discovered" by it
* The load balancer watches the registery and updates itself to include the new microservice
* The new service starts receiving traffic from the load balancer
* If more than one instance of the service exist, the traffic is split among them
* The service sends "keep-alive" signals, or responds to periodic health checks
* When the service is stopped, or stops sending keep-alives, or fails a health check, it is removed from the registry, and in turn from the load balancer

## [Microservices](https://martinfowler.com/articles/microservices.html) by Martin Fowler

> There is a bare minimum of centralized management of these services, which may be written in different programming languages and use different data storage technologies.

> Using services like this does have downsides. Remote calls are more expensive than in-process calls, and thus remote APIs need to be coarser-grained, which is often more awkward to use. If you need to change the allocation of responsibilities between components, such movements of behavior are harder to do when you're crossing process boundaries.

> Any organization that designs a system (defined broadly) will produce a design whose structure is a copy of the organization's communication structure. -- Melvyn Conway, 1967

> a team should own a product over its full lifetime

## [Testing Strategies in a Microservices Architecture](https://martinfowler.com/articles/microservice-testing/)

*
