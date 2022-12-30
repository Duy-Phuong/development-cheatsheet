# New syntax

[GitHub - PacktPublishing/Docker-Quick-Start-Guide: Docker Quick Start Guide, published by Packt](https://github.com/PacktPublishing/Docker-Quick-Start-Guide)

# Images & Containers Images

```bash
#Step 1: Building an image

#Syntax: docker build -t <image-name> <location of dockerfile>

-------------------------------------------------------------------

docker build -t my-test .



#Step 2: List all the images
#Syntax: docker images

-------------------------------------------------------------------

docker images



#Step 3: Inspect the Image
------------------------------------------------------------------

docker inspect <image-id>



docker run -itd --name my-http-container-1 -p 5555:80 my-httpd:latest

docker run -itd --name my-http-container-2 -p 5556:80 my-httpd:latest

docker run -itd --name my-http-container-with-random-port -P my-httpd:latest



#note if you run this it will NOT run in detached mode.

docker run -it --name my-http-container-not-detached-mode -p 5557:80 my-httpd:latest



Step 5: View all the containers

-------------------------------------------------------------------

# Shows all the containers which are running

docker ps 


#Shows all the containers stopped and running

docker ps -a


Step 6: Inspect the container and image

-------------------------------------------------------------------

docker inspect <container-id>


Step 7: View Container logs

-------------------------------------------------------------------

# View the logs of the container
docker logs <container-id>


# Tail the logs of the container
docker logs -ft <container-id>


Step 8: Stop the container

-------------------------------------------------------------------

docker stop <container-id>


Step 9: Stop the container

-------------------------------------------------------------------

docker start <container-id>


Step 10: Logging into the container

-------------------------------------------------------------------

#Note: The container must be started before we can do this.
docker exec -it <container-id> /bin/bash


Step 11: Remove all the containers and images

-------------------------------------------------------------------

#Note: To remove an image the corresponding container built from that image will need to be removed.


#Remove a specific container
docker rm <container-id>



#Remove all containers
docker rm $(docker ps -a -q)



# remove image (note: no containers for this image should be running)
docker rmi <image-id>


# remove all images
docker rmi $(docker images -q)

## ----------------------------------------------------- ##

# Build an image
docker build -t <image-name> <location-of-Dockerfile>

# run an image or run a container directly from a specific image
docker run -it -d -p <host-port>:<container-port> --name <container-name> <image-name>:tag

-i (interactive)
-t (TTY)
-d (Detached) : Run it in detached mode or else the life time of the container will be only as long as you run the terminal.
-P : Docker assigned default ports which will access the required port in the container (meaning the host machine can get assigned 32769 : pointing to default port of the image)
--name : represents a default name, if it is not specified the docker daemon will allocate a unique name
image-name : specifies the image to download from the repostory
tag : defaults to latest, a version for the given image can be specified.


# allows you to view the logs in the container
docker logs <container-id>

# Allow you to tail logs
docker logs -ft <container-id>

# List all containers running and stopped
docker ps -a

# List all containers running
docker ps

# List all images available
docker images

# stop a specified container
docker stop <container-id>

# stop all containers
docker stop $(docker ps -a -q)

# remove image (note: no containers for this image should be running)
docker rmi <image-id>

# remove all images
docker rmi $(docker images -q)

#Remove a specific container
docker rm <container-id>

#Remove all containers
docker rm $(docker ps -a -q)

# log in to the shell of the container. Typically entry-point refers to /bin/bash
docker exec -it <container-name> <entry-point>

docker login 
docker tag <currentimage>:<tag> <repository-name>/<image-name>:<tag>
docker commit <container-id> <repository-name>/<image-name>:<tag>
docker push <repository-name>/<image-name>:<tag>
docker inspect <container-name>
docker inspect <image-name>

#Container volume is the logs directory for example or war directory
docker run -itd -P -v <host-volume-absolute-path>:<container-volume-path>  <container-name>

#docker-machine commands
docker-machine version

#Version of docker-machine
docker-machine ls

#Create a docker-machine
docker-machine create --driver virtualbox --virtualbox-disk-size "20000" <machine-name> 
docker-machine create --driver hyperv  <machine-name>
docker-machine create -d hyperv --hyperv-virtual-switch "<NameOfVirtualSwitch>" <nameOfNode> 

#docker-machine ip default provides the ip of the name of machine "default"
docker-machine ip default

# Telling docker to talk to the new machine
docker-machine env default

#Stop docker-machine
docker-machine stop default

#Start docker-machine
docker-machine start default

# Removing a docker machine
docker-machine rm <machine-name>

#remove all docker machines
docker-machine rm -f $(docker-machine ls -q);

# Runs the container
docker-compose up

# Runs in detached mode
docker-compose up -d

# Builds the image
docker-compose build

# Stops the container
docker-compose stop <service-name>

# Starts the container
docker-compose start <service-name>

# Equivalent to docker exec
docker-compose run <service-name> /bin/bash 

# Build a specific image.
docker-compose up --build

# View logs
docker-compose logs <service-name>

# Tail logs
docker-compose logs -ft <service-name>

# Update specific container
docker-compose up -d --no-deps <service-name>

# Stop all containers
docker-compose down

#Remove errored out images
docker rmi $(docker images | grep "^<none>" | awk "{print $3}")

# Shut down containers, remove all images and rebuild images with docker-compose
docker-compose down
docker-compose rm -f
docker-compose pull
docker-compose up --build -d

# How to find the disk space used by docker
docker system df
du -sch /var/lib/docker/containers
du -c /var/lib/docker/ | head -15 | sort -rn

# cleaning up old containers and images from docker and release resources
docker rm $(docker ps -qa --no-trunc --filter "status=exited")
docker rmi $(docker images --filter "dangling=true" -q --no-trunc)
docker rmi $(docker images | grep "none" | awk '/ / { print $3 }')
docker volume rm $(docker volume ls -qf dangling=true)
docker volume ls -qf dangling=true | xargs -r docker volume rm
docker network rm $(docker network ls | grep "bridge" | awk '/ / { print $1 }')
docker network prune
docker system prune
docker system prune -af


# ---------- CLEAN UP -----------
# To delete all containers including its volumes use,

docker rm -vf $(docker ps -aq)

# To delete all the images,
docker rmi -f $(docker images -aq)

# Remember, you should remove all the containers before removing all the images from which those containers were created.
```

## Images

Images are one of the two core building blocks Docker is all about (the other one is
"Containers").

Images are blueprints / templates for containers. They are read-only and contain the
application as well as the necessary application environment (operating system, runtimes, tools,...).

Images do not run themselves, instead, they can be executed as containers.  
Images are either pre-built (e.g. official Images you find on DockerHub) or you build your own

Images by defining a Dockerfile.

Dockerfiles contain instructions which are executed when an image is built ( `docker build . `),
every instruction then creates a layer in the image. Layers are used to efficiently rebuild and share images.

The CMD instruction is special: It's not executed when the image is built but when a container
is created and started based on that image.

## Containers

Containers are the other key building block Docker is all about.

Containers are running instances of Images. When you create a container (via docker run ), a
thin read-write layer is added on top of the Image.

Multiple Containers can therefore be started based on one and the same Image. All
Containers run in isolation, i.e. they don't share any application state or written data.

You need to create and start a Container to start the application which is inside of a Container. So
it's Containers which are in the end executed - both in development and production.

Key Docker Commands

For a full list of all commands, add --help after a command - e.g. docker --help , docker run
--help etc.

Also view the official docs for a full, detailed documentation of ALL commands and features: https:
//docs.docker.com/engine/reference/run/

Important: This can be overwhelming! You'll only need a fraction of those features and
commands in reality!

```bash
docker build . : Build a Dockerfile and create your own Image based on the file

    -t NAME:TAG : Assign a NAME and a TAG to an image  

docker run IMAGE_NAME : Create and start a new container based on image IMAGENAME (or

use the image id)

    --name NAME : Assign a NAME to the container. The name can be used for stopping and
    removing etc.

    -d : Run the container in detached mode - i.e. output printed by the container is not
    visible, the command prompt / terminal does NOT wait for the container to stop

    -it : Run the container in "interactive" mode - the container / application is then
    prepared to receive input via the command prompt / terminal. You can stop the
    container with CTRL + C when using the -it flag

    --rm : Automatically remove the container when it is stopped 

docker ps : List all running containers

    -a : List all containers - including stopped ones docker images : List all locally stored images

docker rm CONTAINER : Remove a container with name CONTAINER (you can also use the
container id)

docker rmi IMAGE : Remove an image by name / id  

docker container prune : Remove all stopped containers  

docker image prune : Remove all dangling images (untagged images)
    -a : Remove all locally stored images  

docker push IMAGE : Push an image to DockerHub (or another registry) - 
the image name/ tag must include the repository name/ url

docker pull IMAGE : Pull (download) an image from DockerHub (or another registry) - this
is done automatically if you just docker run IMAGE and the image wasn't pulled before
```

### Attaching to an already-running Container

By default, if you run a Container without `-d`, you run in "attached mode".

If you started a container in detached mode (i.e. with `-d`), you can still attach to it afterwards without restarting the Container with the following command: `1. docker attach CONTAINER`

attaches you to a running Container with an ID or name of `CONTAINER`.

## Networking command

- **$ docker network ls**: This is the command we have been using previously, it simply lists networks available for your containers. It will output the network identifier, its name, the driver being used, and a scope of the network
- **$ docker network create**: Creates new network. The full syntax of the command is, docker network create [OPTIONS] NETWORK. We will use the command in a short while
- **$ docker network rm**: The dockercnetworkcrm command simply removes the network
- **$ docker network connect**: Connects the container to the specific network
- **$ docker network disconnect**: As the name suggests, it will disconnect the container from the network
- **$ docker network inspect**: The docker network inspect command displays detailed information about the network. It's very useful, if you have network issues. We are going to create and inspect our network now

```bash
$ docker run -p <hostPort>:<containerPort> <image ID or name> 
$ docker run -p 7000-8000:7000-8000 <container ID or name>  


$ docker network create myNetwork  
$ docker network inspect myNetwork 

docker run -it --net=myNetwork tomcat  

docker run -it --net=bridge myTomcat
docker run -it --net=container:myTomcat myPostgreSQL  

docker run -it --name myTomcat --net=myNetwork tomcat  
docker run -it --net container:myTomcat busybox  

$ docker run -it --name myTomcat2 --net=myNetwork -p 8080:8080 tomcat 

$ docker run --expose=7000-8000 <container ID or name>   
$ docker run -it --name myTomcat2 --net=myNetwork -p 8080:8080 tomcat   


$ docker run -it --name myTomcat3 --net=myNetwork -P tomcat
```

You may ask, what is the difference between exposing and mapping ports, that is, between `--expose` switch and `-p` switches? Well, the `--expose` will expose a port at runtime but will not create any mapping to the host. Exposed ports will be available only to another container running on the same network, on the same Docker host. The `-p` option, on the other hand, is the same as publish: it will create a port mapping rule, mapping a port on the container with the port on the host system. The mapped port will be available from outside Docker. Note that if you do -p, but there is no `EXPOSE` in the Dockerfile, Docker will do an implicit `EXPOSE`. This is because, if a port is open to the public, it is automatically also open to other Docker containers.

The -p option gives you more control than -P when mapping ports. Docker will not automatically pick any random port; it's up to you what ports on the host should be mapped to the container ports.

| Instruction               | Meaning                                                                                                                                         |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------- |
| EXPOSE                    | Signals that there is service available on the specified port. Used in the Dockerfile and makes exposed ports open for other containers.        |
| --expose                  | The same as EXPOSE but used in the runtime, during the container startup.                                                                       |
| -p hostPort:containerPort | Specify a port mapping rule, mapping the port on the container with the port on the host machine. Makes a port open from the outside of Docker. |
| -P                        | Map dynamically allocated random port (or ports) of the host to all ports exposed using EXPOSE or --expose.                                     |

### EXPOSE & A Little Utility Functionality

[Docker & Kubernetes: The Practical Guide [2023 Edition]](https://www.udemy.com/course/docker-kubernetes-the-practical-guide/learn/lecture/22181376#overview)

In the last lecture, we started a container which also **exposed** a port (port 80).

I just want to clarify again, that `EXPOSE 80` in the `Dockerfile` in the end is **optional**. It **documents** that a process in the container will expose this port. But you still need to then actually expose the port with `-p` when running docker run. So technically, `-p` is the **only required part** when it comes to listening on a port. Still, it is a **best practice** to also add `EXPOSE` in the `Dockerfile` to document this behavior.

## Volume command

The basis of volume-related commands is docker volume. The commands are as follows:

- **$docker volume create**: Creates a volume
- **$ docker volume inspect**: Displays detailed information on one or more volumes
- **$docker volume ls**: Lists volumes
- **$ docker volume rm**: removes one or more volumes
- **$ docker volume prune**: removes all unused volumes, which is all volumes that are no longer mapped into any container

The -v option can be used not only for directories but for a single file as well.

You can use the -v multiple times to mount multiple data volumes.

```bash
$ docker run -v d:/docker_volumes/volume1:/volume -it busybox  
$ docker run -it -v ~/.bash_history:/root/.bash_history ubuntu 

$ docker volume create 
$ docker volume create --name myVolume  
$ docker volume ls  
$ docker run -it -v myVolume:/volume --name myBusybox3 busybox  
$ docker run -it -volumes-from myBusybox3 --name myBusybox5 busybox  

docker volume ls -f dangling=true 
docker volume prune  

$ docker rm -v <containerName or ID>
$ docker volume rm <volumeName or ID>
```

The docker volume ls command can take some filter parameters, which can be quite useful. For example, you can list volumes that are not being used by any container:

`docker volume ls -f dangling=true`

```bash

```

[Online Courses - Learn Anything, On Your Schedule | Udemy](https://www.udemy.com/course/docker-kubernetes-the-practical-guide/learn/lecture/22181286#overview)

We saw, that anonymous volumes are **removed automatically**, when a container is removed.

This happens when you start / run a container with the `--rm` option.

If you start a container **without that option**, the anonymous volume would **NOT be removed,** even if you remove the container (with `docker rm ...`).

Still, if you then re-create and re-run the container (i.e. you run `docker run ...` again), a n**ew anonymous volume will be created**. So even though the anonymous volume wasn't removed automatically, it'll also **not be helpful** because a different anonymous volume is attached the next time the container starts (i.e. you removed the old container and run a new one).

Now you just start **piling up a bunch of unused anonymous volumes** - you can **clear them** via `docker volume rm VOL_NAME` or `docker volume prune`.

```bash
# feedback => container name
# feedback-node:volumes => volumes is tag
# mount folder app/feedback to host
docker build -t feedback-node:volumes .
docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedbackfeedback-node:volumes


# Run with bind mount
docker stop feedback-app
docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback -v "Users/maximilian/development/teching/udemy/docker-complete:/app" feedback-node:volumes

docker logs feedback-app

# don't mount /app/node_modules to host file system using anonymous volume
docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback -v "Users/maximilian/development/teching/udemy/docker-complete:/app" -v /app/node_modules feedback-node:volumes

# read-only volume
```

**Bind Mounts - Shortcuts**

Just a quick note: If you don't always want to copy and use the full path, you can use these **shortcuts**:

**macOS / Linux**: `-v $(pwd):/app`

**Windows**: `-v "%cd%":/app`

I don't use them in the lectures, since I want to show an approach that works for everyone (and I don't want to switch between both all the time), but you can use these shortcuts depending on which OS you are working on to save some typing.

[Online Courses - Learn Anything, On Your Schedule | Udemy](https://www.udemy.com/course/docker-kubernetes-the-practical-guide/learn/lecture/22166920#overview)

Volumes are folders (and files) managed on your host machine which are connected to folders `/` files inside of a container.
There are two types of Volumes:

- Anonymous Volumes: Created via -v /some/path/in/container and removed
  automatically when a container is removed because of --rm added on the docker run command

- Named Volumes: Created via -v some-name:/some/path/in/container and NOT
  removed automatically

# Networks / Requests

## Communicating with the Host Machine

Communicating with the Host Machine (e.g. because you have a database running on the Host
Machine) is also quite simple, though it doesn't work without any changes.

On your local machine, this would work - inside of a Container, it will fail. Because localhost
inside of the Container refers to the Container environment, not to your local host machine
which is running the Container / Docker!
But Docker has got you covered!
You just need to change this snippet like this:

```javascript
fetch('host.docker.internal:3000/demo').then(...)
```

`host.docker.internal` is a special address / identifier which is translated to the IP address of
the machine hosting the Container by Docker.

- Important: "Translated" does not mean that Docker goes ahead and changes the source code. Instead,
  it simply detects the outgoing request and is able to resolve the IP address for that request.

## Communicating with Other Containers

Communicating with other Containers is also quite straightforward. You have two main options:

- Manually find out the IP of the other Container (it may change though)

- Use Docker Networks and put the communicating Containers into the same Network

Option 1 is not great since you need to search for the IP on your own and it might change over
time.
Option 2 is perfect though. With Docker, you can create Networks via docker network create
SOME_NAME and you can then attach multiple Containers to one and the same Network.
Like this:

```bash
docker run -network my-network --name cont1 my-image
docker run -network my-network --name cont2 my-other-image
```

Both cont1 and cont2 will be in the same Network.
Now, you can simply use the Container names to let them communicate with each other - again,
Docker will resolve the IP for you (see above).

```javascript
fetch('cont1/my-data').then(...)
```

---

You need to replace local hosts with a special domain, which is understood by Docker and that's host.docker.internal. This special domain is recognized by Docker. It's understood by Docker. And it's translated to the IP address of your host machine as seen from inside the Docker container.

```bash
docker stop favorites
docker stop mongodb
docker container prune

docker network create favorites-net
docker run -d --name mongodb --network favorites-net mongo


docker build -t favorites-node .
docker run --name favorites --network favorites-net -d --rm -p 3000: 3000 favourites-node
```
