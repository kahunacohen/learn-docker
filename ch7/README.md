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

## The null Network
If you need to run a container that cannot be attached to any network it's advised to attach to the `none` network:

```
docker container run -rm --it --network none alpine:latest /bin/sh
```

## Port Management
We've firewalled our containers, but we often need a way for the outside world to communicate with our containers. For
example a web app's web server needs a way to listen to incoming requests from outside the container. For this we have to
map a container's port to a port available from outside the container (the host). We need to wire up a port in the container's network to the network on the host.

There are several ways of doing this:

### Let Docker Choose an Available Port with `-P`

```
docker container run --name web -P -d nginx:alpine
```

This maps nginx port 80 in the container to a free port on the host. We can see which port is being used by doing:

```
docker container port web
```

### Map Port to Specific Port Number with `-p`
Use `-p` to map internal port to host with `{host_port}:{container_port}`:

```
docker container run --name web -p 8080:80 -d nginx:alpine
```

This allows us to access the webserver running on `80` in the container on the host at `8080`.




