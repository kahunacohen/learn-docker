## Chapter 10: Introduction to Docker Swarm
### Architecture
*Raft consensus group* consists of an odd number of manager nodes that communicate with eachother synchronously Each manager holds a complete state of the swarm. There's a leader and followers.

The workers execute the application and communicate with eachother asynchronously over a gossip network due to scability reasons.

### Swarm Nodes
A swarm is a collection of nodes. A node can be a physical computer or a VM. A docker host is a node on which docker is installed. Any node that joins the swarm must also be a docker host.

### Swarm Managers
Each swarm must have at least one manager, though more than one is always recommended. They communicate with eachother using the *raft consensus protocol*. This is a general protocol that's often used when multiple entities need to agree with
eachother about what action to take next. Raft gets its name from *Reliable,Replicated,Redundant, And Fault-Tolerant*.

Managers become a leader by election. A majority of other managers must elect a new manager. For example if the leader goes
down for maintanance, or it malfunctions, a new leader must be elected and once the old manager is brought back online it
becomes a follower. When a new leader is elected its state is synced with the other managers.

A manager manages the swarm state in a high performance key-value store. The state includes things such as how many worker nodes are online, IP addresses, logs etc. Because the communication of managers is syncrounous, it's relatively slow and can 
negatively affect swarm performance. Therefore, we keep the number of managers to a minumim. For example, one manager is
recommended for dev, three for small to meduium sized production environments, and five only for very large apps.

### Swarm Workers
p. 196
