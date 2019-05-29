## Chapter 8: Docker Compose

Docker compose lets us build, run and scale an application consisting of multiple containers using a declarative approach.

To run multiple containers in an app you could use `docker container run` for each service. For example, you may have a
webserver with web application code in one container and a database server in another container. Instead of managing these
containers manually you can use docker compose, which uses a `docker-compose.yml` configuration file. E.g.:

```
version: "3.5"|
services:
    web: 
      image: fundementalsofdocker/ch08-web:1.9
      ports:
        - 3000:3000
    db:
      image: fundementalsofdocker/ch08-db:1.0
      volumes: 
        - pets-data:/var/lib/postgresql/data
    volumes:
      pets=data
```
