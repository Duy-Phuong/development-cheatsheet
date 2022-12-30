[docker-tutorial/scripts.txt at master · pictolearn/docker-tutorial · GitHub](https://github.com/pictolearn/docker-tutorial/blob/master/Usecase-8/scripts.txt)

```bash
*****************************************************************
Docker - machine: Windows and MAC
*****************************************************************

#Create 2 Docker machines (Remember both are virtual PCs running with different IPs)
------------------------------------------------------------------------

Windows: docker-machine create -d hyperv --hyperv-virtual-switch "vs-1" hyperv-vm-1
MAC: docker-machine create --driver virtualbox --virtualbox-disk-size "20000" hyperv-vm-1

Windows: docker-machine create -d hyperv --hyperv-virtual-switch "vs-1" hyperv-vm-2
MAC: docker-machine create --driver virtualbox --virtualbox-disk-size "20000" hyperv-vm-2

#Verify if docker-machine is running on an IP
------------------------------------------------------------------------
docker-machine ip hyperv-vm-1
docker-machine ip hyperv-vm-2

#Run the following command and whatever output you get run it again
------------------------------------------------------------------------
docker-machine env hyperv-vm-1
docker-machine env hyperv-vm-2

#Run the output of the last line of the command which is there before.
------------------------------------------------------------------------
 & "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env hyperv-vm-1 | Invoke-Expression
 & "C:\Program Files\Docker\Docker\Resources\bin\docker-machine.exe" env hyperv-vm-2 | Invoke-Expression

#Verify the docker-machine is active.
----------------------------------------
docker-machine ls

Turning OFF and ON docker-machine
-------------------------------------
docker-machine stop hyperv-vm-1
docker-machine stop hyperv-vm-2
docker-machine start hyperv-vm-1
docker-machine start hyperv-vm-2

Remove docker-machine (Turn off docker-machine on hyper-v manager for windows before executing this command, for MAC no extra steps are required)
docker-machine rm -f <name-of-machine>

*****************************************************************
Docker - compose commands to follow (Common for MAC and Windows)
Manual Reference: https://docs.docker.com/compose/reference/
*****************************************************************
Step-1 # Builds the image
-------------------------------------------------------------------
docker-compose build

#Build a specific image
docker-compose build <service-name>

# Builds and run the containers
docker-compose up --build -d

Step-2 # View the image (All started and stopped containers)
-------------------------------------------------------------------
docker-compose ps 


Step-3 # Run the image in detached/non-detached mode (use -d) 
-------------------------------------------------------------------
docker-compose up -d
docker-compose up
docker-compose up -d <service-name>


Step-4 # View logs and Tail logs
-------------------------------------------------------------------
# View logs
docker-compose logs <service-name>

# Tail logs
docker-compose  logs -ft <service-name>

Step-5 # Login to the container
-------------------------------------------------------------------
docker-compose run <service-name> /bin/bash 

Step-6 # Stops all containers related to the compose file
--------------------------------------------------
docker-compose stop

#Stop and start specific container
docker-compose stop <service-name>

#Stop and remove containers
docker-compose down
```

```bash
# View logs
docker-compose logs <service-name>

# Rebuild image
$ docker-compose up -d --build && docker ps && docker-compose logs -f nginx

docker-compose pull
docker-compose up -d --force-recreate
docker-compose up -d --force-recreate --no-deps ubiid-api


# docker-compose up -d --build = docker build . + docker run myimage 
docker-compose up -d --build

# Similar to `docker exec -it container-id bash`
$ docker-compose  exec nginx bash



# Delete all volumes and images
$ docker-compose down -v && docker rmi -f $(docker images -aq) && rm -rf nginx/logs/*
```

# Docker & Kubernetes: The Practical Guide [2023 Edition]

## Docker Compose

Docker Compose is an additional tool, offered by the Docker ecosystem, which helps with
orchestration / management of multiple Containers. It can also be used for single Containers to
simplify building and launching.

## Why?

Consider this example:

```bash
docker network create shop
docker build -t shop-node .
docker run -v logs:/app/logs --network shop --name shope-web shop-node
docker build -t shop-database
docker run -v data:/data/db --network shop --name shop-db shop-database
```

This is a very simple (made-up) example - yet you got quite a lot of commands to execute and memorize to bring up all Containers required by this application.
And you have to run (most of) these commands whenever you change something in your code or you need to bring up your Containers again for some other reason.
With Docker Compose, this gets much easier.
You can put your Container configuration into a docker-compose.yaml file and then use just one command to bring up the entire environment: docker-compose up

## Docker Compose Files

A docker-compose.yaml file looks like this:

```yml
version: "3.8" # version of the Docker Compose spec which is being used
services: # "Services" are in the end the Containers that your app needs
  web:
    build: # Define the path to your Dockerfile for the image of this container
    context: .
    dockerfile: Dockerfile-web
    volumes: # Define any required volumes / bind mounts
      - logs:/app/logs
  db:
    build:
    context: ./db
    dockerfile: Dockerfile-web
    volumes:
      - data:/data/db
```

You can conveniently edit this file at any time and you just have a short, simple command which you can use to bring up your Containers:

```bash
docker-compose up
```

You can find the full (possibly intimidating - you'll only need a small set of the available options though) list of configurations here: https://docs.docker.com/compose/compose-file/
Important to keep in mind: When using Docker Compose, you automatically get a Network
for all your Containers - so you don't need to add your own Network unless you need multiple
Networks!

## Docker Compose Key Commands

There are two key commands:

```bash
docker-compose up : Start all containers / services mentioned in the Docker Compose file
   -d : Start in detached mode
   --build : Force Docker Compose to re-evaluate / rebuild all images (otherwise, it only
   does that if an image is missing)
docker-compose down : Stop and remove all containers / services
    -v : Remove all Volumes used for the Containers - otherwise they stay around, even if
    the Containers are removed
```

Of course, there are more commands. You'll see more commands in other course sections (e.g. the "Utility Containers" and "Laravel Demo" sections) but you can of course also already dive
into the official command reference: https://docs.docker.com/compose/reference/
