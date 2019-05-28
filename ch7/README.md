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

### More on Attaching Networks and Network Firewalls
You use networks to firewall containers and define what can come in and out of the container. Set up multiple networks and
selectively attach networks to containers to only communicate between containers that need to. You can also attach a container to multiple networks at once.

## The Host Network
Generally you don't want to attach containers to the docker host network as that's a security concern. But there are times
you do. For example for debugging purposes or for app development. To attach to the host network, simply pass the `host`
network name:

```
docker container run --rm -it --network host alpine:latest /bin/sh
```

If we then entered the container and ran `ip` we'd get the same information as if were were on the host and ran `ip`.




