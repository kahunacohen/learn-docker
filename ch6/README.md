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

## Defensive Programming

### Retries

### Logging

### Error Handling

## Redundancy

## Health Checks

## Circuit Breaker Pattern

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
