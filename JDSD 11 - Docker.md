---
title: JDSD 11 - Docker
created: '2020-05-02T14:40:28.349Z'
modified: '2020-05-03T07:27:40.067Z'
---

# JDSD 11 - Docker

* You need to make sure that the project works on your laptop, but also on a different machine.
  * We need to be able to have a consistent environment.
* Containers: Small boxes that can be done everywhere.
  * Multiple layers doing their own thing, all working together.

## Docker Containers

* Before Docker, there were virtualised machines.
  * Virtual machines are sandboxed environments that contain full-fledged computer.
  * Each virtual machine was it's own computer.
  * Because they were such big things, it could take ages to even boot up the application.
* With docker, they wrap up the software in a complete filesystem that is needed to run.
  * Anything you can install on a server, you can use with docker.
  * Containers are a lightweight alternative to virtual machines.
    * They are designed for running single applications on each container.
    * Containters used the hosts operating system and so need just a few seconds to initialise.
  * Easy to grow and grow as you scale.
  * Containters bundle the libraries and settings for the application, not the guest operating system etc.
* A container has:
  * A Host - A computer or machine in which we can host the container on.
  * Container - On top of the host, we have the container.
  * Image - inside of the container we have the image.
    * An image is what docker uses to bundle yuour application to a standalone application in the image.
    * The image generates the contianer that runs 'Node' for us, for example.
      * Within this container, we can have all the libraries and dependencies we want.
    * Standalone, executable package.
  * Within the container, we can also use a filesystem.
* Docker Hub
  * hub.docker.com
  * Kind of like NPM in javascript.
  * You can search and download images that are pre-written.
    * For example, there's already a node-image that is built, with different versions.
    * Postgres/Mongo/Ubuntu etc.
* In our development set-up
  * We'll create a backend docker container set up that has the database, the node server api and redis.
  * We can run one single command and boot it all up together.

## Dockerfile

* Build an image file at the root of the project: `touch Dockerfile`
* `FROM node:carbon` & `CMD ["/bin/bash"]`
  * This grabs node:carbon from docker hub.
  * The CMD instruction tells us what to run in the container.
* We can then use `docker build -t tagnamecontainer .`
  * This will build and tag a docker image.
  * It becomes a lot faster once it has been done.
* We can now use `docker run -it tagnamecontainer`
  * This will then put the terminal inside the docker container.
  * The command bin/bash tells us to go into our shell.
  * Always check what version you are running in your containers.

## Docker Commands

* `-it` puts us inside the container.
* To run a container in the background use `run -it -d tagnamecontainer`
  * You can then use `docker ps` to see all the containers currently running.
* We can then use `docker exec -it containeridhash bash` to go into it.
* `docker stop containeridhash` to stop a container.

## Docker file 2

* `WORKDIR /usr/src/smart-brain-api`
  * This will set the working directory of the app.
* `COPY ./ ./` this will copy all the files in the current folder to the docker container.
* `RUN npm install` - When we open a container, we want to install and run the npm.
  * Run is an image build step.
  * The docker file can run many run steps but only one command when you launch the build image.
* When we have a container, the container doesn't really know of the machine.
  * The computer can't connect to the container by default.
  * We need to forward a port: `docker run -it -p 3000:3000 tagnamecontainer`

## Docker Compose

* Docker compose lets you compose different pieces in one container.
* Docker compose for Linux has to be installed separately.
* Create a file in the root directory: `docker-compose.yml`

```yml
version: '3.6'

services:

  # Backend Api
  smart-brain-api:
    container_name: backend
    # image: node:8.11.1
    build: ./
    command: npm start
    working_dir: /usr/src/smart-brain-api
    environment:
      POSTGRES_URI: postgres://sally:secret@postgres:5432/smart-brain-docker
    links:
      - postgres
    ports:
      - "3000:3000"
    volumes:
      - ./:/usr/src/smart-brain-api
    
  # Postgres
  postgres:
    environment:
        POSTGRES_USER: sally
        POSTGRES_PASSWORD: secret
        POSTGRES_DATABASE: smart-brain-docker
        POSTGRES_HOST: postgres
    image: postgres
    ports:
      - "5432:5432"
```
* Now run `docker-compose build`
* `docker-compose run smart-brain-api`
* `docker-compose down` will remove any old versions of containers
* `docker-compose up --build` and then `docker-compose up`
  * Builds, (re)creates, starts, and attaches to containers for a service.
  * If there are existing containers for a service, and the service’s configuration or image was changed after the container’s creation, docker-compose up picks up the changes by stopping and recreating the containers (preserving mounted volumes).
  * It will also define services according to their container_name.
* With volumes, the host filesystem will be updated and listen for changes from the local machine.
* `docker-compose exec smart-brain-api bash` will run the bash for the container.
* the environment variables can now be accessed at: `process.env.POSTGRES_HOST` etc.
  * After more changes we can now just use:

```node

const db = knex({
  client: 'pg',
  connection: process.env.POSTGRES_URI
});

```

### Making a Postgres File Setup

* Create a folder: `postgres` > Dockerfile
  * Two files in a `tables` folder: login.sql + users.sql

```dockerfile

FROM postgres:10.3

ADD /tables/ /docker-entrypoint-initdb.d/tables
ADD /seed/ /docker-entrypoint-initdb.d/seed
ADD deploy_schemas.sql /docker-entrypoint-initdb.d/tables/

```
* Users table in the .sql file:

```SQL

BEGIN TRANSACTION;
CREATE TABLE users (
  id serial PRIMARY KEY,
  name VARCHAR(100),
  email text UNIQUE NOT NULL,
  entries BIGINT DEFAULT 0,
  joined TIMESTAMP NOT NULL
);

COMMIT;
```
* The begin transaction/commit will make sure that these are atomic commands that will not partially create something if it fails any step.
  * Creates all or nothing.
* In the root of the postgres folder: deploy_schemas.sql:

```SQL

-- Deploy fresh database tables
\i '/docker-entrypoint-initdb.d/tables/users.sql'
\i '/docker-entrypoint-initdb.d/tables/login.sql'

\i '/docker-entrypoint-initdb.d/seed/seed.sql'

```
* Order matters if your tables depend on each other.
* Anytime we have a dockerfile, we want to run build rather than image in the docker-compose.yml file.
* Seed SQL:

```SQL
BEGIN TRANSACTION;

INSERT into users (name, email, entries, joined) values ('Jessie', 'jessie@gmail.com', 5, '2018-01-01');
INSERT into login (hash, email) values ('hashstring', 'jessie@gmail.com')

COMMIT;

```

## Update: Docker Networks

Docker is constantly evolving. One of the features that has changed is the links property you have seen in previous videos while writing our docker compose file. Links have been replaced by networks. Docker describes them as a legacy feature that you should avoid using. You can safely remove the link and the two containers will be able to refer to each other by their service name (or container_name). So in our case all you would need to do is remove the below lines from your Docker Compose file and everything should still work fine:
```
links:
  - postgres
  - redis
```
By default Compose sets up a single network for your app. Each container for a service joins the default network and is both reachableby other containers on that network, and discoverable by them at a hostname identical to the container name. If you want a custom network, you can use the networks property.

