# Chapter 4: Creating & Managing Container Images

## Ways of Creating images

### Interactive

Run a base container, exec into it, add files etc. and commit. To see history of how it was
built run `docker image history`.

### Dockerfile
Use a manifest. This is how it's usually done.

### Importing & Loading From File
Import or load from a tarbar of an existing image using `docker image load` and `docker image save`.

## Building an Image
1. Create an image with `docker image build -t {NAME} .` when a Dockerfile is in the current directory.
E.g.: `docker image build -t my-centos .`, uses centos as a base and installs packages as per Dockerfile.
`-t` is for tag.
2. Run it: `docker container run -d --name my-centos my-centos`
3. Exec into it: 

## Multi-stage Builds
Sometimes we need tools in order to produce artifacts that we use in the container, but we don't need those
tools for the final container. The extra tools just take up space and make the image too large. In this case we can
use multistep builds by using aliases, and copying artifacts in to final stage. E.g. Dockerfile:

```
FROM alpine:3.7 AS build
# Install tools for build etc. maybe apk for building c++ apps etc.
# Compile source code C++ hello world program into app/bin/hello etc.

FROM alpine:3.7 # This is the "Real" build.
COPY  --from=build app/bin/hello app/hello # Copy binary from aliased build
CMD app/hello # Run the binary.
```

This results in an image with just the final binary.

## Dockerfile Best Practices

### .dockerignore

Use `.dockerignore` file to exclude unnecessary files/folders etc. from the image.

### Order Lines to Take Advantage of Caching
Order individual commands smartly. Every cmd in docker is a new layer. When a higher layer changes, 
subsequent layers have to be rebuilt. So make sure anything expensive (e.g. npm install) only happens after something that doesn't change much. For example consider this Dockerfile:

```
FROM node:9.4
RUN mkdir -p /app
WORKDIR /app
COPY . /app
RUN npm install
CMD ["npm", "start"]
```

Here, the app code changes all the time...everytime the developer changes source code. That means that 
everytime app code changes, npm install has to be run and that's expensive. So this is smarter:

```
FROM node:9.4
RUN mkdir -p /app
WORKDIR /app
COPY ./package.json /app
RUN npm install
COPY . /app
CMD ["npm", "start"]
```

Now when rebulding an image npm install will only change when package.json changes, which isn't so often. 

### Keep Layers to a Minimum
Each command in a Dockerfile is another image layer, so the easiest way to keep layers down is to combine
commands. E.g.: 

```
RUN apt-get update
RUN apt-get install -y ca=certificate
```

is better as:

```
RUN apt-get update && apt-get install -y ca=certificate
```

### Use Multi-stage Builds

See section above.

## Sharing & Shipping Images

### Tagging an Image
To share we must give it a globally unique name. Form of name:

```
{registry-url}/{user-or-org}/{name}:{tag}
```


## Official Images
Some important images are sponsered by Docker and officially maintained. These tend to be base images like Linux distros etc.

## Pushing Images to a Registry

First we need to tag (name) our image:

```
docker image tag {source-image} {fully-qualified-tag}
# E.g. dokcker image tag alpine:latest kahunacohen/my-alpine:1.0
```

Then we need to login:

```
docker login -u ... -p ...
```

Then push:

```
docker image push kahunacohen/my-alpine:1.0
```
