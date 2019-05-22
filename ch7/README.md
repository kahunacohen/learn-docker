## Chapter 7: Single-Host Networking

### Container Network Model (CNM)
In distributed apps, services are isolated but often still need to communicate. These communication pathways are called
networks.

Docker defines a simple networking model called container network model, or CNM. Three elements:

1. **Sandbox**: perfectly isolates container from outside world. 
2. **Endpoint**: we need to be able to send and receive data packets. So the endpoint connects the isolated container with the network.
3. **Network**: pathway that transports data packets between containers.

CNM is a model not implementation. There are several implementations that are puggable.

### Network Firewalling
The only way for containers to talk to eachother is if they are attached to the same network. Networks are easy and cheap to
setup, so make sure you design your architecture such that only services that need to directly communicate with eachother are
attached to the same network.

### The Bridge Network
Default type of network. Based on Linux bridge. There's as host-wide network, but it's recommended to create custom networks
that isolate your services. The network allocates a range of IPS, which you can see by doing:

```
docker network inspect {network_name}
```

Under the `IPAM.subnet` key, you can see the range of IPs allocated. E.g. `172.17.0.0/16`.
This is a range of IPs between `172.17.0.0.2` and ...`255`. Every time a container connects it will be given an 
incrementally higher IP.


To create a custom network:

```
docker network create --driver bridge . {network_name}
```

p. 143...



