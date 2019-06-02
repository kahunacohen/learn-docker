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

Some commands:
 * `docker compose up`: starts the app.
 * `docker-compose ps`: shows the services that make up an app and their current state.
 * `docker-compose down`: Shuts down all the services.

### Scaling a Service

E.g. `docker-compose up --scale web=3`. This increases web service instances to 3, but we'll get an error because we are
hard-coding the port to `3000`. Instead we can let Docker decide what port to use.

```
docker-compose down
```

Then, modify `docker-compose.yml`:

```
version: "3.5"
services:
  web:
    image: fundementalsofdocker/ch08-web:1.0
    ports:
      - 3000
  db:
    image: fundementalsofdocker/ch08-db:1.0
  volumes:
    - [ets=data:/var/lib/postgresql/data
 volumes:
   pets-data
 ```
 
 ### Building and Pushing an Application
 
 
 We can also just build the images of our application by adding build information to `docker-compose.yml`:
 
 ```
version: "3.5"
services:
  web:
    build: web
    image: fundementalsofdocker/ch08-web:1.0
    ports:
      - 3000
  db:
    build: database
    image: fundementalsofdocker/ch08-db:1.0
  volumes:
    - [ets=data:/var/lib/postgresql/data
 volumes:
   pets-data
 ```
 
 You might want to do this to push all images associated with an app to a remote hub.
 
 To run:
 
 ```
 docker-compose -f docker-compose.yml build
 docker login -u foo -p bar
 docker-compose -f docker-compose.yml push
 ```
 
 This will push the two images to the docker hub account.
 
