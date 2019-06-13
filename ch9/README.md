## Chapter 9: Orchestrators 

### Tasks of an Orchestrator

### Reconciling the Desired State
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

### Affinity & Location Awareness
Some clusters may require specific hardware requirements. E.g. I.O. intensive services may need a fast solid state drive. An
orchestrator has some way of expressing that a cluster of nodes's affinity for a particular hardware setup, or other requirement.

Geo/location awareness refers to an orchestrator's ability to distrubute load across geographically labeled clusters, such
as East, West, Central etc. This decreases the chance of outage due to a geographically specific event.

### Security

#### Secure Communication & Cryptographic Node Identity

Only trusted nodes can join a cluster. Each node that joins the cluster gets a cryptographic node identity and all
communication between nodes must be encrypted.

Communication in a cluster can be separated into three types:

1. **management**: used by cluster managers or masters to execute health checks, schedule instances etc.
1. **control**: exchange important state information between all nodes of a cluster. E.g. updating local IP tables etc.
1. **data**: application services communicate with each other and exchange data

Normally, the orchestrator is primarally concerned with securing management and control planes. The data plane is usaally
handled by the user, though orchestrators can faciliate that too.

#### Secure Networks & Network Policies
We want controlled access among nodes on the same network to communicate and no way for outside traffic to hit any nodes
in the app unless authorized. Two ways to do this:

1. **Software-defined-network (SDN)**: groups application services together
1. **Flat network**: One flat network and use network policies.

#### Role-based Access Control (RBAC)
To be enterprise-ready an orchestrator must have a way to grant group/role permissions to add nodes/clusters etc. For example,
a typical enterprise might have a dev, qa, and prod groups with different users assigned to them. Grants are used for this.

#### Secrets
An app often needs to access sensitive data, such as API keys, SSH keys, passwords etc. An orchestrator stores such
encrypted data in its cluster state database.

#### Content Trust
For added security we want to make sure only trusted images run in our clusters. The orchestrator achieves this by reading an
image signature that's signed at the source then confirmed before allowing an image to run in the cluster.

#### Reverse Uptime
As of this writing, most orchestrators don't implenent this yet...but the idea is to regularly and at relatively short
intervals (say once a day) tear down each node and replace it with a fresh node. That way if a hack gains access to a node
he can only do damage for a day. In real life a hacker could disguise his identity and spend days wreaking havock if he has
control of nodes.

### Introspection
Orchestrators allow human operators to easily monitor system information such as CPU, memory, disk usage etc. Should
available on a per-node basis or in an aggregated form.

Orchestrators should also give us easy access to logs and allow us to exec into any container if we have proper permissions.

The orchestrator should also allow us to easily trace requests because in a distributed system it's often challenging to
understand how each request is handled.

A graphical dashboard should also be made available to human operators showing the most critical sytem information.

Finally the orchestrator should provide an API for external services to access system data.

## Popular Orchestrators

### Kubernetes
The leader of the orchestrator space. Built by Google. It was designed with massive scability in mind. Cons include setup
complexity etc.

### Docker Swarm
New versions are simple to setup and powerful.

## Apache Mesos & Marathon
Relatively little reach.

## Amazon ECS
Simple to use, but tightly coupled to AWS. Can't use it if you are not invested in that ecosystem.

## Microsoft ACS
Simple and easy to setup but must be bought into the MS Azure ecosytem. 

