# Multi Container Applications

In the real world scenarios, most of the time we will be using multi containers applications. In other world, we have to deal with several containers, espacially in the microservices architecture.

One of the reasons Docker containers are so powerful is that we can connect them together and make them intercat with each other. 


## Networking

Let's say we want to build a web application that uses React library as frontent, django as a backend and postgress for database. In such case we can create three different containers and we can deploy them together.

In order to do that, we shoudl create a **Network** for the containers to make them communicate. In this case, the frontend can send requests to the backend, and the backend can read and write from the database.

There are different types network bridges used in Docker:
- [bridge](https://docs.docker.com/network/bridge/) (default)
- [host](https://docs.docker.com/network/host/)
- [overlay](https://docs.docker.com/network/overlay/)
- [ipvlan](https://docs.docker.com/network/ipvlan/)
- [macvlan](https://docs.docker.com/network/macvlan/)

more about docker networking at [Docker Networking overview](https://docs.docker.com/network/).

First, we must create a network in order to connect a container to it:

```docker
docker network create [NETWORK NAME]
```

Now when running a container, add the `--network` flag:
```
docker run -d --rm --name [CONTAINER NAME] --network [NETWORK NAME] [IMAGE NAME]
```

Now if we run two containers, one that has database running and the other is a backend. In the backend when we want to connect the db, we replace the url with the name of the database container.

For instance if the we replace `mongodb://127.0.0.1:27017/dbname` with `mongodb://mongodb:27017/dbname` to connect to a mongodb database called `dbname`.

To list all networks:
```
docker network ls
```

To list all networks:
```
docker network ls
```
To inspect a network:
```
docker network inspect [NETWORK NAME]
```
To remove a network:
```
docker network rm [NETWORK NAME]
```

