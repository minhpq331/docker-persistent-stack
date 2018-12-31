# Docker Persistent Stack
Docker resources to create persistent stack for development purpose with 1 command

## Available snippets

- [x] [MongoDB](./snippets/mongo.yml)
- [x] [MongoDB Local replicaset](./snippets/mongo-replicaset.yml)
- [x] [Redis](./snippets/redis.yml)
- [x] [MySQL](./snippets/mysql.yml)
- [x] [PostgreSQL](./snippets/postgresql.yml)
- [x] [Neo4J](./snippets/neo4j.yml)
- [x] [RabbitMQ](./snippets/rabbitmq.yml)

## How to use

It's easy, just follow these steps:

**Clone this repository:**
```bash
$ git clone https://github.com/minhpq331/docker-persistent-stack.git
$ cd docker-persistent-stack
```

**Make environment file, change your compose project name and put your `uid` and `gid` in**
```bash
$ cp .env.example .env
$ echo $(id -u):$(id -g)
1000:1000
# This is your uid and gid, copy all this line and paste it in .env file
```

**Choose your persistent stack from available snippets or write yours. Put it in `docker-compose.yml`**

```yml
version: '2'

services:
    mongo:
        container_name: mongo
        image: mongo:latest
        volumes:
            - ./data/mongo:/data/db
        user: "${UID_GID}"
        ports:
            - "27017:27017"
        networks:
            - common
        restart: always    

    redis:
        container_name: redis
        image: redis:alpine
        command: ["redis-server", "--appendonly", "yes"]
        volumes:
            - ./data/redis:/data
        user: "${UID_GID}"
        ports:
            - "6379:6379"
        networks:
            - common
        restart: always

networks:
    common:
```

**Start docker compose**

```bash
$ docker-compose up
```

Or start with interactive mode, you can close your terminal with your stack keep running
```bash
$ docker-compose up &
```

## Useful commands

If you want to keep your stack autorun from now on, make `docker` service autorun with your marchine and start `docker-compose` with interactive mode.

If you want to stop using this stack, simply run below commands:
```bash
$ cd docker-persistent-stack
$ docker-compose down
```

If you want to cleanup one of your service data file, run this command:
```bash
$ cd docker-persistent-stack
$ bash script/clean_data.sh ./data/mongo
# Change ./data/mongo to your service data folder
```