[Docker-Quick-Start-Guide/chapter02-source.txt at master · PacktPublishing/Docker-Quick-Start-Guide · GitHub](https://github.com/PacktPublishing/Docker-Quick-Start-Guide/blob/master/Chapter02/chapter02-source.txt)

https://subscription.packtpub.com/book/networking-and-servers/9781789347326/2/ch02lvl1sec11/summary

# Learning Docker command

```bash
Chapter 2 Source Code Examples

# the new command syntax...
docker container run hello-world

# the old command syntax...
docker run hello-world

# new syntax
# Usage: docker container run [OPTIONS] IMAGE [COMMAND] [ARG...]
docker container run hello-world

# old syntax
docker run hello-world

# You can pre-seed the local docker cache with container images you plan to run by using the docker pull command.  For example:
# new syntax
# Usage: docker image pull [OPTIONS] NAME[:TAG|@DIGEST]
docker image pull hello-world

# old syntax
docker pull hello-world

# Usage: docker container ls [OPTIONS]
docker container ls

# short form of the parameter is -a
docker container ls -a
# long form is --all
docker container ls --all

# old syntax
docker ps -a

# there is no short form of the --rm parameter
docker container run --rm hello-world

# The Remove Container command
# the new syntax
# Usage: docker container rm [OPTIONS] CONTAINER [CONTAINER...]
docker container rm cd828234194a

# the old syntax
docker rm cd828234194a

# Here is an example using the "--name" parameter:
docker container run --name hi-earl hello-world

# You can delete multiple containers at the same time by providing the unique identifier for each on the command line, like this:
docker container rm hi-earl hi-earl2

# An example using the force command to remove it:
docker container rm --force web-server

# Some times, you may want to remove all of the containers on your system, running or not. Here is the command:
# first list all container command
docker container ls --all --quiet
# Now we can feed the values returned by the container list command as input parameters to the container remove command. It will look like this:
# using full parameter names
docker container rm --force $(docker container ls --all --quiet)
# using short parameter names
docker container rm -f $(docker container ls -aq)

# using the old syntax
docker rm -f $(docker ps -aq)

# You can add something like the following to your ~/.bash_profile or ~/zshrc file...
alias RMAC='docker container rm --force $(docker container ls --all --quiet)'

# It is the "--detach" parameter.  Here is what using that parameter looks like:
# using the full form of the parameter
docker container run --detach --name web-server --rm nginx
# using the short form of the parameter
docker container run -d --name web-server --rm nginx
Using this parameter detaches the process from the foreground session and returns control to you as soon as the container has started. You next question is probably How do I stop a detached container.  Well I am glad you asked. You use the container stop command.

The Stop Container command
# Usage: docker container stop [OPTIONS] CONTAINER [CONTAINER...]
docker container stop web-server
In our case, we used the "--rm" parameter when running the container, so as soon as the container is stopped, the "read/write" layer will be automatically deleted.  Like many of the Docker commands, you can provide more than one unique container identifier as parameters to stop more than one container with a single command.

Now you might be wondering, "If I use the "--detach" parameter, how do I see what is happening with the container?". There are several ways you can get information from and about the container. Let's take a look at some of them before we continue with our run parameter exploration.

The Container Logs command
When you run a container in the foreground, all of the output the container sends to standard output and standard error are displayed in the console for the session that ran the container.  However, when you use the "--detach" parameter, control of the session is returned as soon as the container starts so yo don't see the data sent to stdout and stderr.  If you want to see that data you use the container logs command.  That command looks like this:

# the long form of the command
# Usage: docker container logs [OPTIONS] CONTAINER
docker container logs --follow --timestamps web-server
# the short form of the command
docker container logs -f -t web-server

# get just the last 5 lines (there is no short form for the "--tail" parameter)
docker container logs --tail 5 web-server

# the old syntax
docker logs web-server
#The "--details", "--follow", "--timestamps", and "--tail" parameters are all optional,

# The Container Top command
# using the new syntax
# Usage: docker container top CONTAINER [ps OPTIONS]
docker container top web-server

# using the old syntax
docker top web-server
As you might expect, the container top command is only useful for viewing the processes of a single container at a time.

# The Container Inspect command
# using the new syntax
# Usage: docker container inspect [OPTIONS] CONTAINER [CONTAINER...]
docker container inspect web-server

# using the old syntax
docker inspect web-server
As just mentioned, this command returns a lot of data.  You may only be interested in a subset of the metadata.  You can use the "--format" parameter to narrow the data returned.  Check out these examples...

Getting some State data:

# if you want to see the state of a container you can use this command
docker container inspect --format '{{json .State}}' web-server1 | jq

# if you want to narrow the state data to just when the container started, use this command
docker container inspect --format '{{json .State}}' web-server1 | jq '.StartedAt'
Getting some NetworkSettings data:

# if you are interested in the container's network settings, use this command
docker container inspect --format '{{json .NetworkSettings}}' web-server1 | jq

# or maybe you just want to see the ports used by the container, here is a command for that
docker container inspect --format '{{json .NetworkSettings}}' web-server1 | jq '.Ports'

# maybe you just want the IP address used by the container, this is the command you could use.
docker container inspect -f '{{json .NetworkSettings}}' web-server1 | jq '.IPAddress'
Getting data for more than one container with a single command:

# maybe you want the IP Addresses for a couple containers
docker container inspect -f '{{json .NetworkSettings}}' web-server1 web-server2 | jq '.IPAddress'

# since the output for each container is a single line, this one can be done without using jq
docker container inspect -f '{{ .NetworkSettings.IPAddress }}' web-server1 web-server2 web-server3
Most of the above examples use the json processor jq.  If you haven't already installed it on your system, now might be a good time to do so.  Here are the commands to install jq on each of the OSes we've used in this book:

# install jq on Mac OS
brew install jq

# install jq on ubuntu
sudo apt-get install jq

# install jq on RHEL/CentOS
yum install -y epel-release
yum install -y jq

# install jq on Windows using Chocolatey NuGet package manager
chocolatey install jq
The "--format" parameter of the inspect command uses go tempates.  You can find more information about them on Docker document pages for formatting output at this URL: https://docs.docker.com/config/formatting

The Container Stats command
Another very useful Docker command is the stats command.  It provides live, continually updated usages statistics for one or more running containers.  It is a bit like using the Linux top command.  You can run the command with no parameters to view the stats for all running containers, or you can provide one or more unique container identifiers to view the stats for one or more containers specific containers.  Here are some examples of using the command:

# using the new syntax, view the stats for all running containers
# Usage: docker container stats [OPTIONS] [CONTAINER...]
docker container stats

# view the stats for just two web server containers
docker container stats web-server1 web-server2

# using the old syntax, view stats for all running containers
docker stats

# running the container detached
docker container run --detach --name web-server1 nginx

# running the container interactively
# using the long form of the parameters
docker container run --interactive --tty --name web-server2 nginx bash

# using the short form of the parameters (joined as one), which is much more common usage
docker container run -it --name web-server2 nginx bash

# running interactively using the default CMD
docker container run -it --name earls-dev ubuntu

# run a container detached
docker container run --detach -it --name web-server1 -p 80:80 nginx

# show that the container is running
docker container ps

# attach to the container
# Usage: docker container attach [OPTIONS] CONTAINER
docker container attach web-server1

# issue a <ctrl-pq> keystroke to detach (except for Docker on Mac, see below for special Mac instructions)

# again, show that the container is running detached.
docker container ps

# when you are using Docker for Mac, remember to always add the "--sig-proxy=false" parameter
docker attach --sig-proxy=false web-server1

# The Container Exec command
# start an nginx container detached
docker container run --detach --name web-server1 -p 80:80 nginx

# see that the container is currently running
docker container ls

# execute other commands in the running container
# Usage: docker container exec [OPTIONS] CONTAINER COMMAND [ARG...]
docker container exec -it web-server1 bash
docker container exec web-server1 cat /etc/debian_version

# confirm that the container is still running
docker container ls

# The Container Commit command
# Usage: docker container commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]
docker container commit ubuntu new-ubuntu

# The "--publish" parameter includes pairs of port numbers separated by a colon.  For example:
# create an nginx web-server that redirects host traffic from port 8080 to port 80 in the container
docker container run --detach --name web-server1 --publish 8080:80 nginx

# all of these can be running at the same time
docker container run --detach --name web-server1 --publish 80:80 nginx
docker container run --detach --name web-server2 --publish 8000:80 nginx
docker container run --detach --name web-server3 --publish 8080:80 nginx
docker container run --detach --name web-server4 --publish 8888:80 nginx

# however if you tried to run this one too, it would fail to run
#    because the host already has port 80 assigned to web-server1
docker container run --detach --name web-server5 --publish 80:80 nginx
```

# Create docker image

understanding dockerfile in details and explain the difference

[Docker-Quick-Start-Guide/Chapter03 at master · PacktPublishing/Docker-Quick-Start-Guide · GitHub](https://github.com/PacktPublishing/Docker-Quick-Start-Guide/tree/master/Chapter03)

# Docker volumes

[Docker-Quick-Start-Guide/chapter04-source.txt at master · PacktPublishing/Docker-Quick-Start-Guide · GitHub](https://github.com/PacktPublishing/Docker-Quick-Start-Guide/blob/master/Chapter04/chapter04-source.txt)

```bash
Chapter 4 Source Code Examples

# Docker volume managment command
docker volume

# Docker volume management subcommands
docker volume create         # Create a volume
docker volume inspect        # Display detailed information on one or more volumes
docker volume ls             # List volumes
docker volume rm             # Remove one or more volumes
docker volume prune          # Remove all unused local volumes
# Syntax for the volume create command
Usage: docker volume create [OPTIONS] [VOLUME]

# The options available to the volume create command:
-d, --driver string         # Specify volume driver name (default "local")
--label list                # Set metadata for a volume
-o, --opt map               # Set driver specific options (default map[])

# Using the volume create command with no optional parameters
docker volume create

# Create a volume with a fancy name
docker volume create my-vol-02

# The Magic Screen command
screen ~/Library/Containers/com.docker.docker/Data/com.docker.driver.amd64-linux/tty
# or if you are using Mac OS High Sierra
screen ~/Library/Containers/com.docker.docker/Data/vms/0/tty
# Use <CTRL-a>k to kill the screen session

# Start by creating a new volume
docker volume create my-osx-volume
# Now find the Mountpoint
docker volume inspect my-osx-volume -f "{{json .Mountpoint}}"
# Try to view the contents of the Mountpoint's folder
sudo ls -l /var/lib/docker/volumes/my-osx-volume
# "No such file or directory" because the directory does not exist on the OS X host

# Now issue the Magic Screen command and hit <enter> to get a prompt
screen ~/Library/Containers/com.docker.docker/Data/vms/0/tty
# You are now root in the VM, and can issue the following command
ls -l /var/lib/docker/volumes/my-osx-volume
# The directory exists and you will see the actual Mountpoint sub folder "_data"
# Now hit control-a followed by lower case k to kill the screen session
<CTRL-a>k

# mount a pre-created volume with --mount parameter
docker container run --rm -d \
--mount source=my-vol-02,target=/myvol \
--name vol-demo2 \
volume-demo2:1.0 tail -f /dev/null

# VOLUME instruction Dockerfile for Docker Quick Start
FROM alpine
RUN mkdir /myvol
RUN echo "Data from image" > /myvol/both-places.txt
CMD ["sh"]

# Map a single file from the host to a container
echo "important data" > /tmp/data-file.txt
docker container run --rm -d \
   -v /tmp/data-file.txt:/myvol/data-file.txt \
   --name vol-demo \
   volume-demo2:1.0 tail -f /dev/null
# Prove it
docker exec vol-demo cat /myvol/data-file.txt

# Using --mount with source and target
docker container run --rm -d \
   --mount source=my-volume,target=/myvol,readonly \
   --name vol-demo1 \
   volume-demo:latest tail -f /dev/null

# Using --mount with source and destination
docker container run --rm -d \
   --mount source=my-volume,destination=/myvol,readonly \
   --name vol-demo2 \
   volume-demo:latest tail -f /dev/null

# Using -v
docker container run --rm -d \
   -v my-volume:/myvol:ro \
   --name vol-demo3 \
   volume-demo:latest tail -f /dev/null

# Check which container have mounted a volume by name
docker ps -a --filter volume=in-use-volume

# Remove volumes command syntax
Usage: docker volume rm [OPTIONS] VOLUME [VOLUME...]
# Prune volumes command syntax
Usage: docker volume prune [OPTIONS]

# Using a filter on the volume ls command
docker volume ls --filter dangling=true

# Step 1
docker container run \
   --rm -d \
   -v data-vol-01:/data/vol1 -v data-vol-02:/data/vol2 \
   --name data-container \
   vol-demo2:1.0 tail -f /dev/null
# Step 2
docker container run \
   --rm -d \
   --volumes-from data-container \
   --name app-container \
   vol-demo2:1.0 tail -f /dev/null
# Prove it
docker container exec app-container ls -l /data
# Prove it more
docker container inspect -f '{{ range .Mounts }}{{ .Name }} {{ end }}' app-container
```

# Docker network

[Docker-Quick-Start-Guide/chapter06-source.txt at master · PacktPublishing/Docker-Quick-Start-Guide · GitHub](https://github.com/PacktPublishing/Docker-Quick-Start-Guide/blob/master/Chapter06/chapter06-source.txt)

```bash
# Docker network managment command
docker network

# Docker network management subcommands
docker network connect           # Connect a container to a network
docker network create            # Create a network
docker network disconnect        # Disconnect a container from a network
docker network inspect           # Display detailed information on one or more networks
docker network ls                # List networks
docker network rm                # Remove one or more networks
docker network prune             # Remove all unused networks

# Install the weave network driver plug-in
sudo curl -L git.io/weave -o /usr/local/bin/weave
sudo chmod a+x /usr/local/bin/weave
# Disable checking for new versions
export CHECKPOINT_DISABLE=1
# Start up the weave network
weave launch [for 2nd, 3rd, etc. optional hostname or IP of 1st Docker host running weave]
# Set up the environment to use weave
eval $(weave env)

# Start up weave on the 2nd node
weave launch node01

# Peer host node02 with the weave network by connecting from node01
weave connect node02

# On ubuntu-node01:
# Install and setup the weave driver
sudo curl -L git.io/weave -o /usr/local/bin/weave
sudo chmod a+x /usr/local/bin/weave
export CHECKPOINT_DISABLE=1
weave launch
eval $(weave env)

# On ubuntu-node02:
# Install and setup the weave driver
sudo curl -L git.io/weave -o /usr/local/bin/weave
sudo chmod a+x /usr/local/bin/weave
export CHECKPOINT_DISABLE=1
weave launch
eval $(weave env)

# Now, back on ubuntu-node01:
# Bring node02 in as a peer on node01's weave network
weave connect ubuntu-node02

# Starting with ubuntu-node01:
# Run a container detached on node01
docker container run -d --name app01 alpine tail -f /dev/null

# Now, launch a container on ubuntu-node02:
# Run a container detached on node02
docker container run -d --name app02 alpine tail -f /dev/null

# Since we are on node02, we will check there first...
# From inside the app02 container running on node02,
#    let's ping the app01 container running on node01
docker container exec -it app02 ping -c 4 app01

# Similarly, from inside the app01 container running on node01,
#   let's ping the app02 container running on node02
docker container exec -it app01 ping -c 4 app02

# Docker network create command syntax
# Usage: docker network create [OPTIONS] NETWORK

# Create a new overlay network, with all default options
docker network create -d overlay defaults-over

# Create a new overlay network with specific IP settings
docker network create -d overlay \
--subnet=172.30.0.0/24 \
--ip-range=172.30.0.0/28 \
--gateway=172.30.0.254 \
specifics-over
# Initial validation
docker network inspect specifics-over --format '{{json .IPAM.Config}}' | jq

# Create service tester1
docker service create --detach --replicas 3 --name tester1 \
  --network specifics-over alpine tail -f /dev/null
# Create service tester2
docker service create --detach --replicas 3 --name tester2 \
  --network specifics-over alpine tail -f /dev/null
# From a container in the tester1 service ping the tester2 service by name
docker container exec -it tester1.3.5hj309poppj8jo272ks9n4k6a ping -c 3 tester2
```
