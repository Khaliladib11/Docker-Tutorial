# Docker Compose

<p align="center">
  <img align="center" src="../images/docker-compose-button.jpg">
</p>

While working with Docker can cool, working with several containers and run each commmand seperatly can bring headache. **Docker Compose** is a Docker tool that can be used to define and run multi-container applications.
With this tool, we use `yaml` files to configure the app with all the container and necessary configurations. In other words, docker compose can automate the multi-container workflow.

Docker Compose can be used in developement, test and production environments. It has several features:
- Multiple isolated environments on a single host.
- Preserve volume data when containers are created.
- Only recreate containers that have changed.
- Variables and moving a composition between environments.
- Orchestrate multiple containers that work together.

To install Docker Compose go through the [docker documentation](https://docs.docker.com/compose/install/).

In the docker compose yaml file we must define the services we want to use. For instance, for each container we have to provide the name of the image, the ports, the volumes we want to use, etc...

Now let us take a look at Docker Compose yaml file and break it down to understand it better.
```yaml
version: "3.8"
services:
  mongodb:
    image: "mongo"
    volumes:
      - data:/data/db
    env_file:
      - ./env/mongo.env
    container_name: mongodb
    
  backend:
    build: ./backend
    ports:
      - 80:80
    volumes:
      - logs:/app/logs
      - ./backend:/app
      - /app/node_modules
    env_file:
      - ./env/backend.env
    depends_on:
     - mongodb
    container_name: backend-container

  frontend:
    build: ./frontend
    ports:
      - 3000:3000
    volumes:
      - ./frontend/src:/app/src
    stdin_open: true
    tty: true
    depends_on:
      - backend
    container_name: frontend-container
 
 volumes:
  data:
  logs:
```

- `version`: this to define the version for docker compose we are using.
- `services`: this create and define the containers we want, in our case we have three different containers, under service we provide the name of the service (not name of the container).
- `image`: if we want to run a container with a pre-build image we use this flag.
- `build`: if we want to build a new image using a `Dockerfile` we use this flag, and we specify the location of the file. In our example, we have a Dockerfiles in `backend` and `frontend` folders.
- `ports`: to define and map the container's port to the host machine.
- `volumes`: this is for define the volumes and the bind mounts.
- `depends_on`: this option is used if we have container depends on other. In this case, the frontend depends on the backend which depends on the database.
- `container_name`: to give a name to the container.
- `env_file`: to set up an environment variables.


*Note here that we don't define any network, that's because Docker Compose handles that on our behalf.*

To start up the application:
```
docker compose up
```
*we can add `-d` option for detach mode*

To stop the application:
```
docker compose stop
```

To just build the images from the yaml file:
```
docker compose build
```

To list all the images you've build using the docker compose:
```
docker compose images
```

To list all the containers in the current docker compose file:
```
docker compose ps
```

To stop and remove containers, networks, images, and volumes:
```
docker compose down
```
