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
