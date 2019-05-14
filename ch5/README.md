# Chapter 5: Data Volumes & System Management
The whole point of containers is that they are stateless and ephemeral. They are meant to be created and destroyed
easily and quickly. They must be reproducible in any environemnt. Yet real-world projects require state that must be
persisted. Apps need databases, filesystem access etc.

In Docker we achieve this by creating and mounting data volumes.

## Creating Volumes

To create a volume that can be mounted in a container:

```
docker volume create {name}
docker volume inspect {name}
```

## Mounting a Volume

To mount a volume to a container pass the `-v` option to run. E.g.:

```
docker container run --name test -it -v my-data:/data alpine:latest
```

This command runs a container called `test` based on `alpine:latest`, mounting the volume we just
created called `my-data` to the `/data` directory in the container.

## Removing Volumes
To remove a volume:

```
docker volume rm {volume_name}
```

This is a dangerous command because it deletes any data associated with the volume.

## Sharing Data Between Containers
To share data between containers we can mount a volume to multiple containers. To avoid race conditions
we want to make sure only one container has write access--the rest should be readonly. To mount a container as readonly.

Here's the writer:

```
docker container run -it --name writer -v shared-data:/data alpine /bin/sh
```

If we create a file in this container, we can read it thus:

```
docker container run -it --name reader -v shared-data:/app/data:ro ubantu:17.04 /bin/bash
```

We will get an error if we try to exec into reader and create a file in /app/data.

## Using Host Volumes
When developing apps we might want to mount a volume that maps to an actual directory on our host machine. For example if on our host machine we are developing an app on `/Users/acohen/projects/my-app`, we might run a container like this:

```
docker container run -rm -it -v /Users/acohen/projects/my-app:/my-app alpine:latest /bin/bash
```

## Defining Volumes in Images
You can set volumes in your Dockerfile with a `VOLUME` line. E.g.:

```
VOLUME {PATH}
VOLUME {COMMA SEPARATED PATHS}
VOLUME {ARRAY OF PATHS}
```

So, for example, the standard MongoDB image defines in its Dockerfile volumes where it will store data. We can see this by
doing:

```
docker image pull mongo:3.7
docker image inspect \
--format='{{json .ContainerConfig.Volumes}}' \
mongo:3.7 | jq

{
  "/data/configdb": {},
  "/data/db": {}
}
```

## Docker Housekeeping
Over time the docker hosts accumulates images, containers, volumes etc. which consumes system resources. We can use `docker system` to get more info about this kind of stuff.

### Listing Resource Consumption
`docker system df`. Adding the `-v` flag will give us more info.

### Pruning Containers
Pruning containers will remove containers that are not in running status. `docker container prune`. The `-f` flag will
force the action without confirmation.

### Pruning Images
Why would we need to prune images? Because layers of images are immutable. When something changes, like source code, new layers are created, but the layers they replace become orphaned. `docker image prune`.

### Pruning Volumes
Docker won't let you prune volumes associated with a container. E.g.

```
docker volume prune
```

You can also label volumes, then prune according to label:

```
docker volume prune --filter 'label=demo'
```
