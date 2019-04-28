# Chapter 3

## Running a container
* `docker container run {NAME}` either runs a cached container or fetches and then runs. Pass `-d` for dameon.
* Pass `--name` option to name the container.

## Exec into a container
`docker container exec -i -t {NAME}`

## Attaching to a running container

`docker container run -d --name random alpine /bin/sh -c "while :; do wget -qO- http://numbersapi.com/2012/year; printf '\n'; sleep 5; done"`

This creates a named "random" container based off of alpine and runs as a demeon. It hits an API end-point every 5 seconds with random events from 2012. We can then
attach to it and see output from terminal like this:

`docker container attach random`.
