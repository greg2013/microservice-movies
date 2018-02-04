# Developing Microservices - Node, React, and Docker

[![Build Status](https://travis-ci.org/mjhea0/microservice-movies.svg?branch=master)](https://travis-ci.org/mjhea0/microservice-movies)

http://mherman.org/microservice-movies/

## Architecture

| Name             | Service | Container | Tech                 |
|------------------|---------|-----------|----------------------|
| Web              | Web     | web       | React, React-Router  |
| Movies API       | Movies  | movies    | Node, Express        |
| Movies DB        | Movies  | movies-db | Postgres             |
| Swagger          | Movies  | swagger   | Swagger UI           |
| Users API        | Users   | users     | Node, Express        |
| Users DB         | Users   | users-db  | Postgres             |
| Functional Tests | Test    | n/a       | TestCafe             |


## Want to learn how to build this project?

Check out the [blog post](http://mherman.org/blog/2017/05/11/developing-microservices-node-react-docker/).

## Want to use this project?

### Setup

1. Fork/Clone this repo

1. Download [Docker](https://docs.docker.com/docker-for-mac/install/) (if necessary)

1. Make sure you are using a Docker version >= 17:

    ```sh
    $ docker -v
    Docker version 17.03.0-ce, build 60ccb22
    ```

### Build and Run the App

#### Set the Environment variables

```sh
$ export NODE_ENV=development
```

#### Fire up the Containers

Build the images:

```sh
$ docker-compose build
```

Run the containers:

```sh
$ docker-compose up -d
```

#### Migrate and Seed

With the apps up, run:

```sh
$ sh init_db.sh
```

#### Sanity Check

Test out the following services...

##### (1) Users - http://localhost:3000

| Endpoint        | HTTP Method | CRUD Method | Result        |
|-----------------|-------------|-------------|---------------|
| /users/ping     | GET         | READ        | `pong`        |
| /users/register | POST        | CREATE      | add a user    |
| /users/login    | POST        | CREATE      | log in a user |
| /users/user     | GET         | READ        | get user info |

##### (2) Movies - http://localhost:3001

| Endpoint      | HTTP Method | CRUD Method | Result                    |
|---------------|-------------|-------------|---------------------------|
| /movies/ping  | GET         | READ        | `pong`                    |
| /movies/user  | GET         | READ        | get all movies by user    |
| /movies       | POST        | CREATE      | add a single movie        |

##### (3) Web - http://localhost:3007

| Endpoint   | HTTP Method | CRUD Method | Result                  |
|-------------|-------------|-------------|------------------------|
| /           | GET         | READ        | render main page       |
| /login      | GET         | READ        | render login page      |
| /register   | GET         | READ        | render register page   |
| /logout     | GET         | READ        | log a user out         |
| /collection | GET         | READ        | render collection page |

##### (4) Movies Database and (5) Users Database

To access, get the container id from `docker ps` and then open `psql`:

```sh
$ docker exec -ti <container-id> psql -U postgres
```

##### (6) Functional Tests

With the containers up running and TestCafe globally installed, run:

```sh
$ sh test.sh
```

##### (7) Swagger - http://localhost:3003/docs

Access Swagger docs at the above URL

#### Commands

To stop the containers:

```sh
$ docker-compose stop
```

To bring down the containers:

```sh
$ docker-compose down
```

Want to force a build?

```sh
$ docker-compose build --no-cache
```

Remove images:

```sh
$ docker rmi $(docker images -q)
```
``

---
### Docker optimization
`Reduced size from 3,434 mb to 502 mb`
```
gyzheng@greg sandbox  $ docker images | grep microservicemovies | head -20
microservicemovies_web-service        latest              0441bfb32037        2 minutes ago       268MB
microservicemovies_swagger            latest              5c531d1e1d7d        6 minutes ago       88.6MB
microservicemovies_movies-service     latest              8143de7e555d        7 minutes ago       117MB
microservicemovies_movies-db          latest              c031234078e8        8 minutes ago       38.2MB
microservicemovies_users-service      latest              929be921d7a7        8 minutes ago       121MB
microservicemovies_users-db           latest              26e9484cdadd        15 minutes ago      38.2MB
microservicemoviesv2_web-service      latest              389a427fa5fa        26 hours ago        877MB
microservicemoviesv2_swagger          latest              12ed39a7c714        27 hours ago        697MB
microservicemoviesv2_movies-service   latest              3f902847f99e        27 hours ago        726MB
microservicemoviesv2_movies-db        latest              bea610ce28ef        27 hours ago        287MB
microservicemoviesv2_users-service    latest              aa4f83d353d5        27 hours ago        730MB
microservicemoviesv2_users-db         latest              68d0054e10b3        27 hours ago        287MB
```
`Clean up previous images`
```
docker rmi $(docker images | grep microservicemoviesv | awk '{print $3}')
```
