# Docker - nginx

Many docker repositories (even the ones found at Dockerhub, even official ones) have some glaring 
faults that need to be addressed like the user of the container, the permissions problems when mapping volumes
to the host and the default configurations of the services. This repository is part of a series that
aims to take care of those problems and gather the information in a single source while also helping
docker newcomers to understand the environment, the troubles that one might find and how to overcome
them.

## Table of Contents
- [Docker - nginx](#nginx-docker)
  - [Pre-requisites](#pre-requisites)
  - [Project structure](#project-structure)
  - [Binaries](#binaries)
- [Environments](#environments)
  - [Developmtent](#development)
  - [Production](#production)
- [Authors](#authors)  

## Pre-requisites

We're going to use docker and docker-compose to run our development, staging and production environments since it allows 
to have an equal playground between all of them, as well as making the project agnostic to the developer environment/OS.
If you don't have the tools you can find a script to provision the latest vesion of docker community edition (currently 
17.09.1-ce) and the according version of docker-compose (1.17.0-rc1). This script is available for 64-bit ubuntu distributions,
you can find more info about the installation of both binaries in their respective officail pages:

* [Docker Engine](https://docs.docker.com/engine/installation/) - Guide to install docker on multiple environments.
* [Docker Compose](https://docs.docker.com/compose/install/) - Guide to install docker-compose on multiple environments.

Otherwise, if you're using a 64-bit ubuntu distribution (or any other 64-bit ppa distro), simply run:
```shell
./bin/provision-docker-1.17.0
```
as sudo to provision the project.

## Project structure

The project has the following structure (not taking into consideration README files):

```text
docker-nginx/
├─ bin/
│   └─ ...
├─ docker/
│   ├─ nginx/
│   │   ├─ config/
│   │   │   └─ default.conf
│   │   └─ Dockerfile
├─ .gitignore
├─ .travis.yml
├─ docker-compose.yml
└─ swarm-compose.yml
```

Regarding docker's infrastructure:
* `bin/` has the set of scripts that help the developer run, test, debug and deploy the application. more on them later.
* `docker/` contains the configuration files, Dockerfiles and entrypoints of the nginx container (staging/production) and node (provisioning and development).
* `.travis.yml` presents the automatic deployment setup for the host of the project.
* `docker-compose.yml` holds the configuration for a provisioning environment as well as the development server.
* `swarm-compose.yml` is the configuration for a swarm cluster setup, with the required connexion to the traefik proxy network and the limitations of usage in the deploy configuration.

## Binaries
You'll find a set of binaries in the /bin folder, they're there to help you out setting up the project, debugging it, running your tests and provsioning the various environments.
These binaries include:

* **provision-docker-1.17.0:** binary that installs docker-ce and docker-compose to the required version for the application to run (available for 64-bit ppa distributions).
* **push-images:** Allows to push the built docker images to the localhost registry of a cluster.
* **start:** Development script starts a development nginx server at port 80 by default.
* **swarm-start:** Script to deploy a stack in a swarm cluster with the required services to run the application.

# Environments

In this section we'll discuss the different environment setup options the project offers.

## Development

Running the project is as simple as running

```sh
./bin/start
```

This runs the container in a local docker environment through the docker-compose api, you'll be able to find the 
project at localhsot:80 by default, and you can change that port in `docker-compose.yml`


## Production

If you wish to run the application in a swarm cluster environment the local build won't be enough for it. Once you've tested
your staging build and passed all the test pipelines you can start a deploying process. To do so you can setup the project in the 
host that you wish, and simply run:

followed by:
```sh
./bin/push-images
```

This will push the production image of the application to the local registry repository of the cluster. Afterwards you just need to
run:
```sh
./bin/swarm-start
```
in any of your managers in order to start the application in a cluster. More info on:
* [Docker Swarm mode](https://docs.docker.com/engine/swarm/) - Docker native clusterization and container orchestration.
* [Docker Registry](https://docs.docker.com/registry/) - Docker image repository hub.

# Authors

* **Lambdaloopers** - *Project setup* - [github](https://github.com/lambdaloopers)

# License

This project is under the MIT license - do as you wish with it and enjoy!
