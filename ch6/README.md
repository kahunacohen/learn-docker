## Chapter 6: Distributed Application Architecture

## Terms

* **Node**: An individual server used to run apps. Could be an actual on-site server, or a server in the cloud.
* **Cluster**: A group of servers used to run an app connected by a network running in a distributed fashion.
* **Network**: Physical/software communication path between nodes in a cluster.
* **Port**: Channel on which a server listens to incoming requests
* **Service**: A modular part of an application that stands alone and interconnects to other parts of the application, or outside applications.

## Monolithic vs. Distributed Architecture
Traditionally apps were built all in one piece. An example might be a website that had a data-base and ran on one server. The website might make direct queries to the database, format the results and display in HTML.

Distributed architecture breaks apart the app into several, loosely coupled services. For example, a database might run on several servers with a load balancer. A RESTful API app might also run on several distinct servers. The website would then
hit the API for data, as well as maybe a mobile app.

## Stateful Components
Managing state is hard because it's hard to test and hard to distribute. It is often the meat of the app...it represents the business logic. It's best to try to isolate the state and push it up, in one place, to the boundry of the app. For example, a RESTful API should often contain most of the business logic so that state doesn't get locked into one component of the app and
all apps can read/write to it.

## Service Discovery
Services need to know where to find other services. For example a webiste needs to know what IP:PORT to find the API.

In monolithic apps this information is often hard-coded/held in config files. One step better is sticking them in environment
variables. But in a distributed system, where ideally the location of a service could be at an arbitrary IP:PORT we need
an external authority whose sole job it is, is to track where services are at any given time. This is part of what something like orchestration tools such as Docker Swarm and Kubernetes do.

## Load Balancing
An external service that equally distributes requests to container instances. Usually a round robin algorithm is used.

## Defensive Programming
When working on distributed modular microsservice architectures we often rely on more external services. E.g. credit card validation, network calls out to APIs etc. We have to code to "expect the worst, hope for the best". There are several strategies.

### Retries
After a failed call, caller should try again. If that fails it should wait an increasingly longer interval until max tries is
reached, then either a) provide degraded service, or no service depending on context.

### Logging
Apps should log all important events. Log events need to be categorized to be useful. Usually one of:

* debug
* warning
* error
* fatal

Logs should be aggregated to a central location, not on one node.

### Error Handling
Fail fast. Detect errors as early as possible and have the operation fail immediately. E.g. check input range etc. If not
expected, fail immediately and log to `STDERR` for further processing.

### Redundancy
Critical parts of the system need to have multiple instances running so that the app as a whole has little chance of downtime.

### Health Checks
How does the router, orchestrator, load balancer know there's a problem to re-route to redunant instances? By implenting
health checks. Basically this a simple way for such services to query services to ask "are you still there"?

### Circuit Breaker Pattern

## Running in Production

## Logging

## Tracing

## Monitoring

## Application Updates

### Rolling Updates

### Blue-green Deployments

### Canary Releases

### Irreversible Data Changes

## Rollback
