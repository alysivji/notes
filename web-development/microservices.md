# Microservices

## Microservices in Python talk

### Lifecycle of a Microservice

* On startup, the microservice registers with the service register, or is "discovered" by it
* The load balancer watches the registery and updates itself to include the new microservice
* The new service starts receiving traffic from the load balancer
* If more than one instance of the service exist, the traffic is split among them
* The service sends "keep-alive" signals, or responds to periodic health checks
* When the service is stopped, or stops sending keep-alives, or fails a health check, it is removed from the registry, and in turn from the load balancer

### 12 factor app
* read it. learn it
