## Chapter 9: Orchestrators 

### Tasks of an Orchestrator

### Rconcolling the Desired State
Point of an orchestrator is to declaratively initialize and maintain a system's desired state. E.g. how many instances
of a container to run, network attributes, ports etc. Once a system is up and running the orchestrator should continue to
monitor the state and attempt to maintain it. So, for example, if an instance crashes and goes down the orchestrator should
work to maintain the desired amount of containers based on the configuration.

### Replicated and Global Services
A replicated service is a service that's required to run in a specified number of instances, while a global
service is a service that is supposed to run on each of the replicated services. Orchestrator's job, for example, is to work
to ensure that there is at least one global service running on each replicated service.

### Service Discovery
It is the orchestrators job to be able to find sub-services. We don't explicitely tell app instances, for example, what url
and port to find a specific instance of a RESTful API.

### Routing
The orchestrator handles routing of data packets between containers in the same cluster, between containers in different clusters and from outside the application entirely.

### Load Balancing
In a distributed system all services should be reduntant, meaning each services needs to have > 1 instances in case one goes
down. This means the orchestrator must distribute load equally to all instances of a service. This is usually done in a
round-robin manner. We don't want instances sitting around doing nothing.

### Scaling
Orchestrators manage how many instances of a service are up at any time depending on circumstances and load.

### Self-healing
Orchestrators need to be able to take down and replace unhealthy containers. Beyond crashes, how the orchestrator
determines health is up to the human managers of the system. To manage this, we can declaritvely define how to check
health--usually via HTTP `GET` requests etc, but we can do more if needed.
