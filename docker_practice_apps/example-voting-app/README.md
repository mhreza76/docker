# Example Voting App

A simple distributed application running across multiple Docker containers.

## Run manually in docker

```shell
docker run -d --name=redis redis
docker run -d -e POSTGRES_PASSWORD=postgres -e POSTGRES_USER=postgres --name=db postgres:15-alpine

cd vote
docker build . -t voting-app
docker run -d -p 5000:80 --name=voting-app --link redis:redis voting-app

cd ../worker
docker build . -t worker-app
docker run -d --name=worker-app --link redis:redis --link db:db worker-app

cd ../result
docker build . -t result-app
docker run -d -p 5001:80 --name=result-app --link db:db result-app
```

## Run manually in docker compose
- Make sure docker-compose installed.
```shell
cd vote
docker build . -t voting-app
docker run -d -p 5000:80 --name=voting-app --link redis:redis voting-app

cd ../worker
docker build . -t worker-app
docker run -d --name=worker-app --link redis:redis --link db:db worker-app

cd ../result
docker build . -t result-app
docker run -d -p 5001:80 --name=result-app --link db:db result-app
```

- make a "docker-compose.yml" file and put the below code into this.
```shell
 - 5000:80
    links:
      - redis
  worker:
    image: worker-app
    links:
      - redis
      - db
  result:
    image: result-app
    ports:
      - 5001:80
    links:
      - db
```
run `docker-compose up`




## Getting started

Download [Docker Desktop](https://www.docker.com/products/docker-desktop) for Mac or Windows. [Docker Compose](https://docs.docker.com/compose) will be automatically installed. On Linux, make sure you have the latest version of [Compose](https://docs.docker.com/compose/install/).

This solution uses Python, Node.js, .NET, with Redis for messaging and Postgres for storage.

Run in this directory to build and run the app:

```shell
docker compose up
```

The `vote` app will be running at [http://localhost:5000](http://localhost:5000), and the `results` will be at [http://localhost:5001](http://localhost:5001).

Alternately, if you want to run it on a [Docker Swarm](https://docs.docker.com/engine/swarm/), first make sure you have a swarm. If you don't, run:

```shell
docker swarm init
```

Once you have your swarm, in this directory run:

```shell
docker stack deploy --compose-file docker-stack.yml vote
```

## Run the app in Kubernetes

The folder k8s-specifications contains the YAML specifications of the Voting App's services.

Run the following command to create the deployments and services. Note it will create these resources in your current namespace (`default` if you haven't changed it.)

```shell
kubectl create -f k8s-specifications/
```

The `vote` web app is then available on port 31000 on each host of the cluster, the `result` web app is available on port 31001.

To remove them, run:

```shell
kubectl delete -f k8s-specifications/
```

## Architecture

![Architecture diagram](architecture.excalidraw.png)

* A front-end web app in [Python](/vote) which lets you vote between two options
* A [Redis](https://hub.docker.com/_/redis/) which collects new votes
* A [.NET](/worker/) worker which consumes votes and stores them in…
* A [Postgres](https://hub.docker.com/_/postgres/) database backed by a Docker volume
* A [Node.js](/result) web app which shows the results of the voting in real time

## Notes

The voting application only accepts one vote per client browser. It does not register additional votes if a vote has already been submitted from a client.

This isn't an example of a properly architected perfectly designed distributed app... it's just a simple
example of the various types of pieces and languages you might see (queues, persistent data, etc), and how to
deal with them in Docker at a basic level.
