## Chapter 10: Introduction to Docker Swarm
### Architecture
*Raft consensus group* consists of an odd number of manager nodes that communicate with eachother synchronously Each manager holds a complete state of the swarm. There's a leader and followers.

The workers execute the application and communicate with eachother asynchronously over a gossip network due to scability reasons.

### Swarm Nodes
A swarm is a collection of nodes. A node can be a physical computer or a VM. A docker host is a node on which docker is installed. Any node that joins the swarm must also be a docker host.

### Swarm Managers
Each swarm must have at least one manager, though more than one is always recommended for critical applications. They communicate with eachother using the *raft consensus protocol*. This is a general protocol that's often used when multiple entities need to agree with
eachother about what action to take next. Raft gets its name from *Reliable,Replicated,Redundant, And Fault-Tolerant*.

Managers become a leader by election. A majority of other managers must elect a new manager. For example if the leader goes
down for maintanance, or it malfunctions, a new leader must be elected and once the old manager is brought back online it
becomes a follower. When a new leader is elected its state is synced with the other managers.

A manager manages the swarm state in a high performance key-value store. The state includes things such as how many worker nodes are online, IP addresses, logs etc. Because the communication of managers is synchronous, it's relatively slow and can 
negatively affect swarm performance. Therefore, we keep the number of managers to a minumim. For example, one manager is
recommended for dev, three for small to meduium sized production environments, and five only for very large apps.

### Swarm Workers
Simply run tasks that managers assign them. They exchange mostly information about service discovery and routing, which
containers are running etc. To ensure gossiping scales, they only communicate with three random neighbors.
her 
### Stacks, Services, and Tasks
When dealing with swarms and orchestrators we are no longer thinking in terms of containers with well known names
and IP addresses. This gets abstracted away. We are thinking in terms of clusters of hosts and we only care about having
a desired state and if the swarm is reflecting that desired state. E.g. we don't care about which node a service runs but
rather whether the desired number of replicas is always running on whateber nodes the swarm scheduler decides to put
them on.

### Services
A *swarm service* is a desired state. It's a manifest describing:

* name of the service
* image from which to create the service
* num of replicas to run
* network the container(s) are attached to
* ports that should be mapped

### Task
A container is an instance that runs on a worker node, while a task is a description of the service as part of a swarm
service.

### Stack
A stack is a collection of swarm services that are related, most probably because they are part of the same application.
We declare a stack declaratively using yaml.

## Multi Host Networking
We discussed how nodes communicate with each other over a single host, now containers on different nodes  must communicate
with each other. The Linux bridge networking model cannot do this, so it uses VXLAN. When a container tries to send
a packet to another container in a different node the bridge network realizes that it can't find the target. Then
each node participating in the overlay network receives a VXLAN Tunnel Endpoint object (VTEP). This intercepts
the packet and wraps it in a header that allows the target to receive it.

## Creating a Docker Swarm
