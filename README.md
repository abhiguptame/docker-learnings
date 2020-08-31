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
