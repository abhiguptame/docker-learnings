# Docker Learnings

```
$ docker info
$ docker run hello-world
```

## Run an Ubuntu container with:  
```
 $ docker run -it ubuntu bash   
```

## Show the Images:
```
$ docker images
```

## Docker Flow:
### IMAGE => $ docker run => RUNNING CONTAINER => STOPPED CONTAINER => $ docker commit => NEW IMAGE

## Run the Image:
```
$ docker run -ti ubuntu:latest bash
```

### -ti stands for Terminal Interactive 

## Check the Image Distribution Details:
```
$ cat /etc/lsb-release 
ISTRIB_ID=Ubuntu                                                                                                                                             DISTRIB_CODENAME=focal 
DISTRIB_RELEASE=20.04 
DISTRIB_DESCRIPTION="Ubuntu 20.04 LTS"   

```

## To Come out of Ubuntu Terminal:
```
$ exit 
```
### OR
### CTRL + d

## To Take a look at running images:
```
$ docker ps --format $FORMAT
```
### => --format for printing the data vertically

## To Create File in Ubuntu:
```
$ touch My_File_Name
```

## Created files will not be persisted as part of Images: It didn't get deleted it is there in Stopped Container

## To Check the Most Recently running Container:
```
$ ps -l --format=$FORMAT
```

## To Create a New Image:
```
$ docker commit <CONTAINER_NAME> <MY_CUSTOM_DOCKER_NAME>
```

## To Add the Tag to Image:
```
$ docker tag <SHA256_DOCKER_IMAGE_ID> <MY_CUSTOM_DOCKER_NAME>
```

## Running Things in Docker:
```
$ docker run
=> Containers have a main process
=> The Container stops when that process stops
=> Containers have names
```

## Remove the container once exit
```
$ docker run -rm -ti ubuntu
```
### eg:
```
$ docker run -rm -ti ubuntu sleep 5
$ docker run -rm -ti ubuntu bash -c "sleep 3; echo all done"
```

## To Keep The Container In Background:
```
$ docker run -d -ti ubuntu bash 
=> -d for deattach 
```

## To Attach the Container:
```
$ docker attach <MY_CONTAINER_NAME>
```

## To De-attach the Running Container:
### => CTRL+p CTRL+q

## Running More Things in a Container:
```
$ docker exec
```

### => Starts another process in a existing container
### => Great for debugging and DB administration
### => Can't add ports,volumes, and so on 

## Open a new bash for existing contianer:
```
$ docker exec -ti <CONTAINER_NAME> bash
```

## Looking at the Container Output:
```
$ docker logs <CONTAINER_NAME>
```

## Stopping Container:
```
$ docker kill <CONTAINER_NAME>
```

## Removing Container:
```
$ docker rm <CONTAINER_NAME>
```

## Resource Constraints:
### Memory Limits:
```
$ docker run --memory <maximum-allowed-memory> <image-name> commands
```

### CPU Limits:
```
$ docker run --cpu-shares <Relative to other container>
$ docker run --cpu-quota <To limit it in general>

```

## Container Networking:
```
$ docker run -rm -ti -p 45678:45678

$ docker run -rm -ti -p 45678:45679 --name echo-server ubuntu:14.04 bash

$ nc -lp 45678 | nc -lp 45679
```
### => nc :  Netcap
### => -lp : Listen port

## Send and Recieve Data On Different Port:

### => Open two terminals

#### 1) $ nc localhost 45678
#### 2) $ nc localhost 45679

#### => Type Some Text in 1), Data will appear in 2)

#### To call docker on port: host.docker.internal (For Mac), For windows use machine IP address or DNS name 

## Exposing Ports Dynamically:
### => The port inside the container is fixed
### => The port on the host is chosen from the unused ports
### => This allows many containers running programs with fixed ports
### => This often is used with a service discovery program

## To Get the external port assign dynamically:
```
$ docker run -rm -ti -p 45678 -p 45678 --name echo-server ubuntu:14.04 bash
```

## To Get the List of the assigned ports:
```
$ docker port echo-server
```

## Exposing UDP Ports:
### => docker run -p outside-port: inside-port/protocol(tcp/ udp)
### => By default protocal is TCP
```
$ docker run -p 1234:1234/udp 
```
#### => Listen to UDP ports:
```
$ nc -ulp 45678
```
#### => -ulp : UPD listen port
```
$ nc -u localhost 32456
```

## Docker Virtual Network:

## To List the networks:
```
$ docker network ls
```
## Create a new network:
```
$ docker network create <Network_Name>
```
## Create a container in network:
```
$ docker run -rm ti --net <Network_Name> --name <Container_Name> ubuntu:14.04 bash
 ```
## To Check the Network Machine running:
```
$ ping <Network_Name>
```

## To Connect a container with existing Network:
```
$ docker network connect <Network_Name> <Container_Name>
```

## Legacy Linking for connecting to containers:
### => Links all ports, through only one way
### => Secret environment variables are shared only one way
### => Depends on startup order
### => Restarts only sometimes break the links

## To Create machine with environment variable name:
```
$ docker run -rm -ti -e SECRET=<Secret_Name> --name <Container_Name> ubuntu:14.04 bash
```
### => -e : Environment

### => Connect to Existing Container:
```
$ docker run -rm -ti --link <Container_Name> --name <Container_Name> ubuntu:14.04 bash
```
#### => Links only run one way

### To Get the Evironment Details:
```
$ env
```
## Listing Images:
```
$ docker images
```

## To Tag an Image: Tagging gives images names
```
$ docker commit <Container_ID> <Image_Name>:<TAG_Name>
```

## Getting Images: Run automaticallly by docker run
```
$ docker pull
```

## Cleaning Up: Remove Images from System
```
$ docker rmi <Image_Name>:<TAG_Name>
or
$ docker rmi <Image_ID>
```
## Docker Volumes:
### => Virtual "discs" to store and share data

### => Two main varieties:
#### 1) Persistent : Data persists post container is deleted
#### 2) Ephemeral : Data gets deleted with container, Volumes can be passed to other container

## Sharing Data with the host:
### => "Shared folders" with the host
```
$ docker run -ti -v <Host-Machine-Folder-Path>:<Container-Folder-Path> <Image> <Application-Name>
$ docker run -ti -v /Users/my_username/my_folder:/shared-folder ubuntu bash
```
#### => -v : Volume
### => Sharing a "single file" into a container
```
$ docker run -ti -v <Host-Machine-File-Path>:<Container-File-Path> <Image> <Application-Name>
$ docker run -ti -v /Users/my_username/my_file.txt:/shared-folder/my_file.txt ubuntu bash
```

## To Use Volume From Other Container:
```
$ docker run -ti -volumes-from <Container_Name> ubuntu bash
```
## Docker Registries:
### => Registries manage and distribute images
### => Docket Company offers these for free
### => We can create our own as well

## Finding Images:
```
$ docker search <Image_Name>
```
### => It can be searched on web: https://hub.docker.com

## To Login:
```
$ docker login
```

## Pull an Image:
```
$ docker pull <Image_Name>:<Tag-Name>
```

## Rename Pulled Image:
```
$ docker tag <Pulled_Image_Name>:<Tag-Name> <docker.io User Name>/<New Image Name>:<Tag_Name>
```

## Push the Docker Image:
```
$ docker push <docker.io User Name>/<New Image Name>:<Tag_Name>
```
#### => Don't push Images with Password

# Dockerfile

### => This is a small program to create an image
```
$ docker build -t <My_Result_Name> .
=> -t : Tag Name
=> . : The Path Of the docker file
=> When it finishes, the result will be in our local docker registry
```

## Producing the Next Image with Each Step:
### => Each line takes the image from the previous line and makes another image
### => The previous image is unchanged
### => It does not edit the state from the previous line
### => We don't want large files to span lines or our images will be huge

## Caching With Each Step:
### => This is important, watch the build output for "using cache"
### => Docker skips lines that have not changed since last build
### => If our first line is "download latest file", it may not always run
### => This caching saves huge amount of time 
### => The parts that changes the most belong at the end of the Dockerfile

## Dockerfiles are not shell scripts:
### => Processes we start on one line will not be running on the next line
### => Environment variables we set will be set on the next line: If we use the ENV command remember that each line is its own call to docker run.

## Make Dockerfile:
```
$ sudo vim Dockerfile
```
### => Add following data:
```
FROM <Image_Name>
RUN echo "building simple docker image"
CMD echo "hello container"
```
## Build Docker Image:
```
$ docker build -t <My_Container_Name> .
```

## Install a Program with Docker Build:
=> Put following in Dockerfile:
```
FROM debian:sid
RUN apt-get -y update
RUN apt-get install nano
CMD ["/bin/nano", "/tmp/notes"]
```

## Adding a File Through Docker Build and Opening it:
=> Put following in Dockerfile:
```
FROM <Existing_Container_Name>
ADD notes.txt /notes.txt
CMD ["/bin/nano", "/notes.txt"]
```

# Dockerfile Syntax:

## The FROM Statement:
=> Which image to dowload and start from 
=> Must be the first command in your Dockerfile

## The MAINTAINER Statement:
=> Defines the author of the Dockerfile
```
MAINTAINER FirstName LastName <email@domain.com>
```

## The RUN Statement:
### => Runs the command line, waits for it to finish, and saves the result

## The ADD Statement:
### => Adds local files:
```
ADD run.sh /run.sh
```

### => Adds the contents of tar archives:
```
ADD project.tar.gz /install/
```

### => Works with URLs as well:
```
ADD https://project.domain.com/download/project.rpm /project/
```

## The ENV Statement:
### => Sets environment variables:
### => Both during the build and when running the result:
```
ENV DB_HOST=db.production.domain.com
ENV DB_PORT=5432
```

## The ENTRYPOINT Statement:
### => ENTRYPOINT specifies the start of the command to run

## The CMD Statement:
### => CMD specifies the whole command to run

### => If we have both ENTRYPOINT and CMD, they are combined together
### => If our constainer acts like a command-line program, we can use ENTRYPOINT

## The EXPOSE Statement:
### => Maps a port into the container
```
EXPOSE 8080
```

## The VOLUME Statement:
### => Defines shared or ephemeral volumes:
```
VOLUME ["host/path/", "container/path/"]
VOLUME ["/shared-data"]
```
### => Avoid defining shared folders in Dockerfiles

## The WORKDIR Statement:
### => Sets the directory the container starts in 
```
WORKDIR /install/
```

## The USER Statement:
### => Sets which user the container will run as
```
USER abhiguptame
```

### REFERENCES: https://docs.docker.com/engine/reference/builder/


### Multi-project Docker files:

#### => Run a container which gives the size of a page:

### Put following in Dockerfile:

```
FROM ubuntu:16.04
RUN apt-get update
RUN apt-get -y install curl 
RUN curl https://google.com | wc -c > google-size
ENTRYPOINT echo google is this big: cat google-size
```
### => wc : word count
### Here size of the image: ~171 MB

## To reduces the size of the image:
### => Now split the Dockerfile content as following:

```
FROM ubuntu:16.04 as <Container_Name>
RUN apt-get update
RUN apt-get -y install curl 
RUN curl https://google.com | wc -c > google-size

FROM <New_Container_Name>
COPY --from=<Container_Name> /google-size /google-size
ENTRYPOINT echo google is this big: cat google-size
```

### => Here Size of the image: ~4.4 MB

## Preventing the Golden Image Problem:
### => Include installers in your project
### => Have a canonical build that builds everything completely from scratch 
### => Tag builds with the git hash of the code that built in
### => Use small base images
### => Build images you share publically from Dockerfiles, always
### => Don't ever leave passwords in layers; delete files in the same step!

## What Kernels Do:
### => Responds to messages from the hardware
### => Starts and schedule programs
### => Control and organize storage
### => Pass messages between programs
### => Allocate resources, memory, CPU, network, and so on 
### => Create containers by Docker configuring the kernel

## What Docker (Program written in Go) Does:
### => Manage kernel features: 
### 1) Uses "cgroups" to contain processes
### 2) Uses "namespaces" to contain networks
### 3) Uses "copy-on-write" filesystems to build images

### => Makes scripting distributed system "easy"

## Docker is two programs: a client and a server
### => The server receives commands over a socket (either over a network or through a "file", docker.sock)
### => The client can even run inside docker itself

## To See The Bridge Details:
```
$ apt-get install bridge-utils
```

## Show Bridge Details:
```
$ brctl show 
```

## Bridging:
### => Docker uses bridges to create virtual networks in our computer
### => These are software switches
### => They control the Ethernet layer
### => We can turn off this protection with 
```
$ docker run --net=host options <Image_Name> command
$ docker run --net=host --priviledged=true ubuntu bash
```

## Routing:
### => Creates "firewall" rules to move packets between networks
### => Change the source address on the way out
### => Change the destination address on the way back in
### => "Exposing" a port is really "port forwarding"

## To Check Routing:
```
$ sudo iptables -n -L -t nat
```

## To Install iptables:
```
$ sudo apt-get iptables
```

## Namespaces:
### => They allow processes to be attached to private network segments
### => These private networks are bridged into a shared network with the rest of the containers
### => Contaniers have virtual network "cards"
### => Containers get their own copy of the networking stack 


## Process and cgroups:
### => In Docker, our container starts with an init process and vanishes when process exits

## To Find the info of container process:
```
$ docker inspect --format '{{.state.pid}}' <Container_Name>
```

## Resource Limiting:
### => Scheduled CPU time
### => Memory allocation limits
### => Inherited limitation and qhotas
### => Can't escape your limits by starting more processes

## Storage:
### The Secret of Docker: COWs (copy on write)

## Moving COWs:
### => The Contents of layers are moved between containers in gzip files
### => The containers are independent of the storage engine
### => Any containers can be loaded (almost) anywhere
### => It is possible to run out of layers on some of the storage engines

## Volumes and Bind Mounting:
### => The Linux VFS (Virtual File System)
### => Mounting devices on the VFS
### => Mounting directories on the VFS

### To Mount:
```
$ mount -o bind <First-Folder-Path> <Second-Folder-Path>
```

### To Unmount:
```
$ umount <Second-Folder-Path>
```

### To Show all the directories with file info:
```
$ ls -R
```

#### => Mounting voulumes, always mounts the hosts's filesystem over the guest

### What is a Docker Registry?
### => Is a program
### => Stores layers and images
### => Listems on (usually) port 5000
### => Maintains an index and searched tags
### => Autorizes and authenticates connections (sometimes)
### => The registery is a Docker Service.

```
$ docker run -d -p 5000:5000 --restart:always --name <Give-New-Registry-Name> <Image-Name>:<Tag or Version Name>
$ docker run -d -p 5000:5000 --restart:always --name new-registry registry:2

```
### More Info: https://docs.docker.com/registry/

## Saving and Loading Containers:
```
$ docker save -o <File-Name(Mostly tar.gz)> <Image-Name>:<Tag-Name> <Image-Name>:<Tag-Name> <Image-Name>:<Tag-Name>
$ docker load -i <File-Name>

=> -o : Output
=> -i : Input
```
## Orchestrations:
### => Start Containers and restart them if they fail
### => Service discovery, allow them to find each other
### => Resource allocation, match containers to computes

## Docker Compose:
### => Single machine coordination
### => Designed for testing and development
### => Bring up all your containers, volumes, networks, etc., with one command

## Kubernetes:
### => Containers run programs
### => Pods group containers together
### => Services make pods available to others
### => Labels are used for very advanced service discovery
### => Built in service discovery 
### => More Info: https://kubernetes.io/

## EC2 Container Service (ECS):
### => Task Definitions: Define a set of containers that always runtogether
### => Tasks: Actually makes a container run right now
### => Services and exposes it to the Net: Ensures that a task is running all the time

## Advantages of ECS:
### => Connects load balancers (ELBs) to service
### => Can create our own host instances in AWS
### => Make your instances start the agent and join the cluster
### => Pass the docker control socket into the agent
### => Provides docker repos, and its easy to run our own repos
### => Note that containers (tasks) can be part of CloudFormation stacks!
### => More Info: https://aws.amazon.com/ecs/
### => Other Option: https://aws.amazon.com/fargate/
