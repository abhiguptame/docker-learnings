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
```
