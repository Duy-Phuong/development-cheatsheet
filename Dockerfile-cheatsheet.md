https://subscription.packtpub.com/book/virtualization-and-cloud/9781786468390/5/ch05lvl1sec38/dockerfile-instructions

## Dockerfile instructions

### FROM

This is the first instruction in the Dockerfile. It sets the base image for every subsequent instruction coming next in the file. The syntax for the FROM instruction is straightforward. It's just:

**FROM <image>, or FROM <image>:<tag>, or FROM <image>@<digest>**

For example:

- openjdk: An official repository containing an open-source implementation of the Java platform, Standard Edition. The tag latest, which will be used if you do not specify any tag, points to the 8u121-alpine OpenJDK release, as of the time of writing this book
- fabric8/java-alpine-openjdk8-jdk: This base image is actually also being used by the fabric8 Maven plugin
- frolvlad/alpine-oraclejdk8: There are three tags you can choose from: full (only src tarballs get removed), cleaned (desktop parts get cleaned), slim, everything but the compiler and JVM is removed. The tag latest points to the cleaned one
- jeanblanchard/java: A repository containing images based on Alpine Linux to keep the size minimal (about 25% of an Ubuntu-based image). The tag latest points to Oracle Java 8 (Server JRE)

### MAINTAINER (deprecated)

### WORKDIR

The WORKDIR instruction adds a working directory for any CMD, RUN, ENTRYPOINT, COPY, and ADD instructions that comes after it in the Dockerfile. The syntax for the instruction is WORKDIR /PATH. You can have multiple WORKDIR instructions in one Dockerfile, if the relative path is provided; it will be relative to the path of the previous WORKDIR instruction.

### ADD

What ADD basically does is copy the files from the source into the container's own filesystem at the desired destination. It takes two arguments: the source () and a destination ():

```
ADD <source path or URL> <destination path >
```

The source can have two forms: it can be a path to a file, a directory, or the URL. The path is relative to the directory in which the build process is going to be started (the build context we have mentioned earlier). This means you cannot have, for example "../../config.json" placed as a source path parameter of the ADD instruction.

The source and destination paths can contain wildcards. Those are the same as in a conventional file system: * for any text string, or ? for any single character.

For example, ADD target/*.jar / will add all files ending with .jar into the root directory in the image's file system.

**When it comes to the ownership of the files created in the image, they will always be created with the user ID (UID) 0 and group ID (GID) 0**

### COPY

The COPY instruction will copy new files or directories from  and add them to the file system of the container at the path .

It's very similar to the ADD instruction, even the syntax is no different:

```
COPY <source path or URL> <destination path >
```

The same rules from ADD apply to COPY: all source paths must be relative to the context of the build. Again the presence of the trailing slash at the end of the source and destination path is important: if it's present, the path will be considered a file; otherwise, it will be treated as a directory.

Of course, as in ADD, you can have multiple source paths. If source or destination paths contain spaces, you will need to wrap them in square brackets:

```
COPY ["<source path or URL>" "<destination path>"]
```

The  is an absolute path (if begins with a slash), or a path relative to the path specified by the WORKDIR instruction.

### ADD and COPY

As you can see, the functionality of COPY is almost the same as the ADD instruction, with one difference. COPY supports only the basic copying of local files into the container. On the other hand, `ADD gives some more features, such as archive extraction, downloading files through URL, and so on`. Docker's best practices say that you should prefer COPY if you do not need those additional features of ADD. The Dockerfile will be cleaner and easier to understand thanks to the transparency of the COPY command.

### RUN

The RUN instruction is the central executing instruction for the Dockerfile. In essence, the RUN instruction will execute a command (or commands) in a new layer on top of the current image and then commit the results. The resulting committed image will be used as a base for the next instruction in the Dockerfile. As you will remember from [Chapter 1](https://subscription.packtpub.com/book/virtualization-and-cloud/9781786468390/1), *Introduction to Docker*, layering is the core concept in Docker. RUN, takes a command as its argument and runs it to create the new layer.

This also means that COPY and ENTRYPOINT set parameters can be overridden at runtime, so if you don't change anything after starting your container, the result will always be the same. RUN however, will be executed at build time and no matter what you do at runtime, its effects will be here.

### CMD

The CMD instruction is used to define the default action taken when containers are run from images built with their Dockerfile. While it is possible to include more than one CMD instruction in a Dockerfile, only the last one will be significant

The purpose of a CMD instruction is to provide `defaults for an executing container`. You can think of the CMD instruction as a starting point of your image, when the container is being run later on. This can be an executable, or, if you specify the ENTRYPOINT instruction (we are going to explain it next), you can omit the executable and provide the default parameters only. The CMD instruction syntax can have two forms:

- `CMD ["executable","parameter1","parameter2"]`: This is a so called exec form. It's also the preferred and recommended form. The parameters are JSON array, and they need to be enclosed in square brackets. The important note is that the exec form does not invoke a command shell when the container is run. It just runs the executable provided as the first parameter. If the ENTRYPOINT instruction is present in the Dockerfile, CMD provides a default set of parameters for the ENTRYPOINT instruction.
- `CMD command parameter1 parameter2`: This a shell form of the instruction. This time, the shell (if present in the image) will be processing the provided command. The specified binary will be executed with an invocation of the shell using `/bin/sh -c`. It means that if you display the container's hostname, for example, using `CMD echo $HOSTNAME`, you should use the shell form of the instruction.

We have said before that the recommended form of `CMD` instruction is the exec form. Here's why: everything started through the shell will be started as a subcommand of `/bin/sh -c`, which does not pass signals. This means that the executable will not be the container's PID 1, and will not receive Unix signals, so your executable will not receive a `SIGTERM` from `docker stop <container>`. There is another drawback: you will need a shell in your container. If you're building a minimal image, it doesn't need to contain a shell binary. The `CMD` instruction using the shell form will simply fail.

When Docker is executing the command, it doesn't check if the shell is available inside the container. If there is no `/bin/sh` in the image, the container will fail to start.

On the other hand, if we change the `CMD` to the exec form, Docker will be looking for an executable named echo, which, of course, will fail, because echo is a shell command.

Because `CMD` is the same as a starting point for the Docker engine when running a container, there can only be one single `CMD` instruction in a Dockerfile.

If there are more than one `CMD` instruction in a `Dockerfile`, only the last one will take effect.

### RUN and CMD

RUN is a build-time instruction, the CMD is a runtime instruction.

Believe it or not, we can now have our REST example microservice containerized. Let's check if it builds by executing the mvn clean install on the pom.xml file created in [Chapter 4](https://subscription.packtpub.com/book/virtualization-and-cloud/9781786468390/4), *Creating Java Microservices*. After the successful build, we should have a target directory with the rest-example-0.1.0.jar file created. The Spring Boot application JAR in the target directory is an executable, fat JAR. We are going to run it from within the Docker container. Let's write the basic Dockerfile using the command we already know and place it in the root of our project (this will be the context for our docker build command):

```docker
FROM jeanblanchard/java:8
COPY target/rest-example-0.1.0.jar rest-example-0.1.0.jar
CMD java -jar rest-example-0.1.0.jar
```

We can now run the docker build command, using rest-example as the image name, omitting the tag (as you will remember, omitting a tag when building an image will result in creating the latest tag):

```java
$ docker build . -t rest-example
$ docker run -it rest-example
```

### The ENTRYPOINT

The reason for that is simple: CMD was developed first, then ENTRYPOINT was developed for more customization, and some functionality overlaps between those two instructions. Let's explain it a bit. The ENTRYPOINT specifies a command that will always be executed when the container starts. The CMD, on the other hand, specifies the arguments that will be fed to the ENTRYPOINT. Docker has a default ENTRYPOINT which is /bin/sh -c but does not have a default CMD. For example, consider this Docker command:

```java
docker run ubuntu "echo" "hello world"
```

The syntax for the ENTRYPOINT instruction can have two forms, similar to CMD.

`ENTRYPOINT ["executable", "parameter1", "parameter2"]` is the exec form, preferred and recommended. Exactly the same as the exec form of the `CMD` instruction, this will not invoke a command shell. This means that the normal shell processing will not happen. For example, ENTRYPOINT `[ "echo", "HOSTNAME" ]` will not do variable  on the `$HOSTNAME` variable. If you want shell processing then you need either to use the shell form or execute a shell directly. For example:

```docker
ENTRYPOINT [ "sh", "-c", "echo $HOSTNAME" ]
```

Variables that are defined in the Dockerfile using ENV (we are going to cover this in a while), will be substituted by the Dockerfile parser.

`ENTRYPOINT command parameter1 parameter2` is a a shell form. Normal shell processing will occur. This form will also ignore any CMD or `docker run command` line arguments. Also, your command will not be PID 1, because it will be executed by the shell. As a result, if you then run `docker stop <container>`, the container will not exit cleanly, and the stop command will be forced to send a `SIGKILL` after the timeout.

Exactly the same as with the CMD instruction, only the last `ENTRYPOINT` instruction in the Dockerfile will have an effect. Overriding the `ENTRYPOINT` in the Dockerfile allows you to have a different command processing your arguments when the container is run. If you need to change the default shell in your image, you can do this by changing an `ENTRYPOINT`:

```docker
FROM ubuntu
ENTRYPOINT ["/bin/bash"]
```

From now on, all parameters from CMD, or provided when starting the container using docker run, will be processed by the Bash shell instead of the default `/bin/sh -c`.

Consider this simple Dockerfile based on BusyBox. BusyBox is software that provides several stripped-down Unix tools in a single executable file. To demonstrate ENTRYPOINT, we are going to use a ping command from BusyBox:

```docker
FROM busybox
ENTRYPOINT ["/bin/ping"]
CMD ["localhost"]
```

Let's build the image using the previous Dockerfile, by executing the command:

```bash
$ docker build -t ping-example .
```

If you now run the container using the ping image, the ENTRYPOINT instruction will be processing arguments from the supplied CMD argument: it will be localhost by default in our case. Let's run it, using the following command:

```markup
$ docker run ping-example
```

Because the command-line parameter will be appended to the ENTRYPOINT parameters, we can run our ping image with different parameters passed to the ENTRYPOINT. Let's try it, by running our ping example with different input:

```bash
$ docker run ping-example www.google.com
```

- You can use the exec form of `ENTRYPOINT` to set fairly stable default commands and arguments and then use either form of `CMD` to set additional defaults that are more likely to be changed.

Having the `ENTRYPOINT` instruction gives us a lot of flexibility. And, last but not least, an ENTRYPOINT can be also overridden when starting the container using the `--entrypoint` parameter for the `docker run` command. Note that you can override the `ENTRYPOINT` setting using `--entrypoint`, but this can only set the binary to execute (no `sh -c` will be used). As you can see, both `CMD` and `ENTRYPOINT` instructions define what command gets executed when running a container. Let's summarize what we have learned about the differences and their cooperation:

- A Dockerfile should specify at least one `CMD` or `ENTRYPOINT` instruction
- Only the last `CMD` and `ENTRYPOINT` in a Dockerfile will be used
- `ENTRYPOINT` should be defined when using the container as an executable
- You should use the `CMD` instruction as a way of defining default arguments for the command defined as `ENTRYPOINT` or for executing an ad-hoc command in a container
- `CMD` will be overridden when running the container with alternative arguments
- `ENTRYPOINT` sets the concrete default application that is used every time a container is created using the image
- If you couple `ENTRYPOINT` with `CMD`, you can remove an executable from CMD and just leave its arguments which will be passed to ENTRYPOINT
- The best use for `ENTRYPOINT` is to set the image's main command, allowing that image to be run as though it was that command (and then use CMD as the default flags)

### CMD and ENTRYPOINT

Unlike the `CMD` parameters, the `ENTRYPOINT` command and parameters are not ignored when a Docker container runs with command-line parameters.

The `CMD` instruction, as you will remember from its description, sets the default command and/or parameters, which can be overwritten from the command line when you run the container. The `ENTRYPOINT` is different, its command and parameters cannot be overwritten using the command line. Instead, all command line arguments will be appended after the `ENTRYPOINT` parameters. This way you can, kind of, lock the command that will be executed always during the container start.

### EXPOSE

The EXPOSE instruction informs Docker that the container listens on the specified network ports at runtime. We have already mentioned the EXPOSE instruction in [Chapter 2](https://subscription.packtpub.com/book/virtualization-and-cloud/9781786468390/2), *Networking and Persistent Storage*. As you will remember, EXPOSE in a Dockerfile is the equivalent to the --expose command-line option. Docker uses the EXPOSE command followed by a port number to allow incoming traffic to the container. We already know that EXPOSE does not make the ports of the container automatically accessible on the host. To do that, you must use either the -p flag to publish a range of ports or the -P flag to publish all of the exposed ports at once.

Let's get back to our Dockerfile and expose a port:

```docker
FROM jeanblanchard/java:8
COPY target/rest-example-0.1.0.jar rest-example-0.1.0.jar
CMD java -jar rest-example-0.1.0.jar
EXPOSE 8080
```

If you now re-build the image using the same command, `docker build . -t rest-example`, you'll notice that Docker outputs the fourth layer, saying that port 8080 has been exposed. Exposed ports will be available for the other containers on this Docker host, and, if you map them during runtime, also for the external world. Well, let's try it, using the following docker run command:

```markup
$ docker run -p 8080:8080 -it rest-example
```

### VOLUME

The syntax couldn't be simpler: it's just VOLUME ["/volumeName"].

The parameter for VOLUME can be a JSON array, a plain string with one or more arguments. For example:

```docker
VOLUME ["/var/lib/tomcat8/webapps/"]
VOLUME /var/log/mongodb /var/log/tomcat
```

### LABEL

The syntax of the LABEL instruction is straightforward:

```docker
LABEL "key"="value"
```

To have a multiline value, separate the lines with backslashes; for example:

```docker
LABEL description="This is my \
multiline description of the software."
```

### ENV

ENV is a Dockerfile instruction that sets the environment variable <key> to the value <value>. You have two options for using ENV:

- The first one, `ENV <key> <value>`, will set a single variable to a value. The entire string after the first space will be treated as the <value>. This will include any character, and also spaces and quotes. For example:

```docker
ENV JAVA_HOME /var/lib/java8
```

- The second one, with an equal sign, is ENV <key>=<value>. This form allows setting multiple environment variables at once. If you need to provide spaces in the values, you will need to use quotes. If you need quotes in the values, use backslashes:

```docker
ENV CONFIG_TYPE=file CONFIG_LOCATION="home/Jarek/my \app/config.json"
```

The environment variables set using ENV will persist when a container is run from the resulting image. The same as with labels created with LABEL, you can view the ENV values using the docker inspect command. The ENV values can also be overridden just before the start of the container, using `docker run --env <key>=<value>`.

### USER

The USER instruction sets the username or UID to use when running the image. It will affect the user for any RUN, CMD, and ENTRYPOINT instructions that will come next in the Dockerfile.

The syntax of the instruction is just USER <user name or UID>; for example:

```markup
USER tomcat
```

You can use the USER command if an executable can be run without privileges. The Dockerfile can contain the user and group creation instruction the same as this one:

```bash
RUN groupadd -r tomcat && useradd -r -g tomcat tomcat
```

### ARG

The ARG instruction is being used to pass an argument to the Docker daemon during the docker build command. An ARG variable definition comes into effect from the line on which it is defined in the Dockerfile. By using the --build-arg switch, you can assign a value to the defined variable:

```bash
$ docker build --build-arg <variable name>=<value> .
```

The value from the --build-arg will be passed to the daemon building the image. You can specify multiple arguments using multiple ARG instructions. If you specify a build time argument that is not defined using ARG, the build will fail with an error, but the default value can be specified in the Dockerfile . You specify the default argument value this way:

```docker
FROM ubuntuARG user=jarek
```
