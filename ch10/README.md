## Chapter 10: Introduction to Docker Swarm
### Architecture
*Raft consensus group* consists of an odd number of manager nodes that communicate with eachother synchronously Each manager holds a complete state of the swarm. There's a leader and followers.

The workers execute the application and communicate with eachother asynchronously over a gossip network due to scability reasons.

### Swarm Nodes
A swarm is a collection of nodes. A node can be a physical computer or a VM. A docker host is a node on which docker is installed. Any node that joins the swarm must also be a docker host.

### Swarm Managers
Each swarm must have at least one manager, though more than one is always recommended. They communicate with eachother using the *raft consensus protocol*. This is a general protocol that's often used when multiple entities need to agree with
eachother about what action to take next. Raft gets its name from *Reliable,Replicated,Redundant, And Fault-Tolerant*.


