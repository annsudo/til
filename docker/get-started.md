# Running Docker in both development and production environment 
Containers are running instances of docker images

### EX: <image-name> --> redis -app
  
docker search <name> -->to find an image 


docker run <options> <image-name> --> start a container based on a Docker Image  
 ** By default, Docker will run a command in the foreground. 
  
docker run -d <image-name> --> To run in the background, the option -d needs to be specified
  
docker run -d redis:3.2. --> specify version

docker ps --> lists all running containers

docker inspect <friendly-name|container-id> --> provides more details about a running container

docker logs <friendly-name|container-id> -->  display messages the container has written to standard error or standard out

### accessing
If a service needs to be accessible by a process not running in a container, then the port needs to be exposed via the Host. Once exposed, it is possible to access the process as if it were running on the host OS itself.

ports are bound when containers are started using -p <host-port>:<container-port> option
it's useful to define a name when starting the container, this means she doesn't have to use Bash piping or keep looking up the name when trying to access the logs.
 --> running Redis in the background, with a name of redisHostPort on port 6379 is using the following command
  docker run -d --name redisHostPort -p 6379:6379 redis:latest

!! The problem with running processes on a fixed port is that you can only run one instance.

#### tips
You can specify a particular IP address when you define the port mapping, for example, -p 127.0.0.1:6379:6379


#### Jane would prefer to run multiple Redis instances and configure the application depending on which port Redis is running on
---> jane discovers that just using the option -p 6379 enables her to expose Redis but on a randomly available port.
    docker run -d --name redisDynamic -p 6379 redis:latest

--> While this works, she now doesn't know which port has been assigned. Thankfully, this is discovered via
     docker port redisDynamic 6379
     
--> Jane also finds that listing the containers displays the port mapping information,
    docker ps
    
####  Persisting Data
Jane needs the data to be persisted and reused when she recreates a container.

--> Containers are designed to be stateless. Binding directories (also known as volumes) is done using the option -v <host-dir>:<container-dir>. 
  
Using the Docker Hub documentation for Redis, Jane has investigated that the official Redis image stores logs and data into a /data directory.
Any data which needs to be saved on the Docker Host, and not inside containers, should be stored in /opt/docker/data/redis.

--> docker run -d --name redisMapped -v /opt/docker/data/redis:/data redis

##### tips: Docker allows you to use $PWD as a placeholder for the current directory.

### Running A Container In The Foreground
-->  If Jane wanted to interact with the container (for example, to access a bash shell) she could include the options -it.
-->  certain images allow you to override the command used to launch the image.  Being able to replace the default command makes it possible to have a single image that can be re-purposed in multiple ways. For example, the Ubuntu image can either run OS commands or run an interactive bash prompt using /bin/bash

docker run ubuntu ps --> launches an Ubuntu container and executes the command ps to view all the processes running in a container.

docker run -it ubuntu bash --> allows Jane to get access to a bash shell inside of a container.


# Build own Docker Image

--> Docker Images start from a base image

--> This base image is defined as an instruction in the Dockerfile. Docker Images are built based on the contents of a Dockerfile. The Dockerfile is a list of instructions describing how to deploy your application

### In this example, our base image is the Alpine version of Nginx
in the editor !! in "Dockerfile"

`FROM nginx:alpine
COPY . /usr/share/nginx/html`

The first line defines our base image. 
The second line copies the content of the current directory into a particular location inside the container.

--> The Dockerfile is used by the Docker CLI build command. The build command executes each instruction within the Dockerfile. The result is a built Docker Image that can be launched and run your configured app. `docker build -t <build-directory> `. The -t parameter allows you to specify a friendly name for the image and a tag, commonly used as a version number. This allows you to track built images and be confident about which version is being started.

`docker build -t webserver-image:v1 . ` --> Build our static HTML image
docker images --> view a list of all the images on the host using

--> Launch our newly built image providing the friendly name and tag. As it's a web server, bind port 80 to our host using the -p parameter.

`docker run -d -p 80:80 webserver-image:v1`

--> Once started, you'll be able to access the results of port 80 via
` curl docker`

# Learn how to build and launch your own container images
For this scenario, the container will be running a static HTML application using Nginx, a high-performance web server
If you want to access any of the services, then use docker instead of localhost or 0.0.0.0.

The Dockerfile allows for images to be composable, enabling users to extend existing images instead of building from scratch. By building on an existing image, you only need to define the steps to setup your application.
--> 
` FROM nginx:1.11-alpine `

--> With the base image defined, we need to run various commands to configure our image. There are many commands to help with this, the main commands two are COPY and RUN.

RUN <command> allows you to execute any command as you would at a command prompt, for example installing different application packages or running a build command

COPY <src> <dest> allows you to copy files from the directory containing the Dockerfile to the container's image. This is extremely useful for source code and assets that you want to be deployed inside your container.  

`COPY index.html /usr/share/nginx/html/index.html `

Define which ports on Docker shoul be opened and can be boud to

` EXPOSE <port>`

define the command that launches the application. The CMD line in a Dockerfile defines the default command to run when a container is launched.
If the command requires arguments then it's recommended to use an array, for example ["cmd", "-a", "arga value", "-b", "argb-value"], which will be combined together and the command cmd -a "arga value" -b argb-value would be run.

` CMD ["nginx", "-g", "daemon off;"]`. --> will be auto translated on run to nginx -g daemon off

An alternative approach to CMD is ENTRYPOINT. While a CMD can be overridden when the container starts, a ENTRYPOINT defines a command which can have arguments passed to it when the container launches. In this example, NGINX would be the entrypoint with -g daemon off; the default command.


After writing your Dockerfile you need to use docker build to turn it into an image

`docker build -t my-nginx-image:latest . `. --> -t used for defining friendly name

You can check the container is running using 
`docker ps `

Launch an instance
` docker run -d -p 80:80 <image-id|friendly-tag-name> `

You can access the launched web server via the hostname docker

` curl -i http://docker `

# Docker commands
`docker image list ` --> lists all th images

### docker logs 
 `docker logs ContainerID/Name `
 
 ### terminal of a running container, it --> interactive terminal
 
  `docker exec -it conainerIB /bin/bash`
  
  __ ` env` --> enviromental variables
  __ ` exit` --> exit the terminal
  
  ## docker run vs docker start
   `docker run ` --> takes image and creates a container 
   
   `docker start ` --> starts the container
   
 ## docker network  
    `docker network ls `  --> shows networks 
    `docker network create my-new-network `
    `docker run -d-p 27017:27017 -e USER_NAME=myUserName --name myMongoBD --net my-new-network mongo : ` --> runs container in a special network + uses enviromental varibals -e
    same 
    ` docker run -d \
    -p 27017:27017 \
    -e USER_NAME=myUserName \
    --name myMongoBD \
    --net my-new-network \
    mongo  `
    
   [Tutorial](https://www.youtube.com/watch?v=6YisG2GcXaw&list=PLy7NrYWoggjwPggqtFsI_zMAwvG0SqYCb&index=8)
   `docker logs containerName ` --> all logs
   `docker logs containerName | tail ` --> last part of logs only
   `docker logs containerName -f ` --> string the logs (real time streaming). write --------- to mark the last logs, new would appear after it
   
   ## Docker compose file ([Picture](https://youtu.be/MVIcrmeV_6c?list=PLy7NrYWoggjwPggqtFsI_zMAwvG0SqYCb&t=368))
   
   `docker-compose -f myComposefile.yaml up ` --> starting container using Docker compose file. "Up" starts all containers in the yaml
   `docker-compose -f myComposefile.yaml down ` --> stopping containers, removing them, and removing the network
   
   ## Building image from you own application [tutorial](https://www.youtube.com/watch?v=WmcdMiyqfZs&list=PLy7NrYWoggjwPggqtFsI_zMAwvG0SqYCb&index=10)
   ### Dockerfile: 
   
   `FROM node:13-alpine` -->  same as #install node
    ` RUN mkdir -p /home/app` --> executed in the container only.-p stands for sub-directory
    ` COPY . /home/app` --> coppies from your laptop (.  which means ths directory) to conteiner/home/app
       `CMD ["node", "/home/app/server.js"] ` --> entery point command. Cam only be used once
   
   ### Building docker image from Dockerfile :
   
   ` docker build -t my-app:1.0 .` --> -t for tag, {.} for curent directory= where the Docker file is placed
   ` ` --> 
  ## Deleting
  
  `docker rm containerName` -->deleting container
  `docker rmi imageName` -->deleting image
 
 ## See inside the container
 ` docker exec -it containerName /bin/bash` --> or sometimes  ` docker exec -it containerName /bin/sh`
 ` ` -->
 
 
 
 
 
 
 
 
