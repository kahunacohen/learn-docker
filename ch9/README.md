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

### Zero Downtime Deployments
Now-a-days applications are expected to be updated without taking them offline. Also see: https://dev.to/mostlyjason/intro-to-deployment-strategies-blue-green-canary-and-more-3a3.

#### Rolling Deployments
The orchestrator replaces batches of instances, such that old and new instances co-exist. The orchestrator checks for misbehavior and replaces the next batch if things are OK. 
If, not the orchestrator will roll back the misbehaving instances.

#### Blue-Green Deployments
This is another fail-safe process. In this method, two identical production environments work in parallel and work off the
same backends. Blue is considered production, and green is the new version. Green is deployed but not routed to. Once
checked for health, the load balancer routes to green and can be re-routed back to blue if problems crop up.

Blue-green deployments rely on traffic routing. This can be done by updating DNS CNAMES for hosts. However, long TTL values can delay these changes. Alternatively, you can change the load balancer settings so the changes take effect immediately. Features like connection draining in ELB can be used to serve in-flight connections.

#### Canary Releases
Canary deployment is like blue-green, except it’s more risk-averse. Instead of switching from blue to green in one step, you use a phased approach.

With canary deployment, you deploy a new application code in a small part of the production infrastructure. Once the application is signed off for release, only a few users are routed to it. This minimizes any impact.

