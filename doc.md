# Docker

## Docker runtime
docker runtime allows us to start and stop containers
- runc
- containerd

## Docker engin
docker daemon

## Orchestration

## Open conainer initiative
- https://opencontainers.org/
- blog docker-and-oci-runtimes-a9c23a5646d6


# Docker in windows

Install docker desktop

# Docker Images
- images are readonly
- Like blueprints for container
1. Runtime environment
2. application code
3. any dependencies
4. extra configuration (env variables)
5. commands

Images are made up from several "layers"

1. parent image
   - includes the OS and sometimes the runtime environment

docker-hub

2. source code
3. dependencies


# Containers
- Running instance of an image
- Runs our application
- Isolated Process

find images
https://hub.docker.com/

to install a image
docker pull node

- It'll create a image using node with a random name
- now you can see created image from the docker desktop
- we can use this image as a parent layer

# The Dockerfile
- In your projects root directory create a file name Dockerfile
- It tells docker to create a specific image with all the layers

- this is our initial layer a parent image
FROM node:20-alpine

and add our addition layers
- to copy our source code
first dot is to tell the source code path
second dot is to tell in image where to put code
path is relative here
COPY . /app

- to install all the dependencies
RUN npm install // this gonna run in the root director of the image
// but in our image, the source code is inside the /app directory

so run our command in our source code, at top we add WORKDIR to tell our work directory

WORKDIR /app

and after adding WORKDIR
we change our COPY . /app to COPY . .
because it'll copy code inside /app/app

- exposing port to the our container
EXPOSE 3000


- To run our node application we add
CMD ['node', 'app.js']

we don't use RUN node app.js, because it executed while image is being created, but we want to start node after image is done


# To build our image
- Go to your project directory
- run docker build -t image_name relative_path_to_Dockerfile

# This command takes you straight inside the container.
docker run -it ubuntu

# Create image with tag
docker build -t image_name:tag .

# .dockerignore file
to ignore unnecessary file like:
node_module
*.md

# See all docker images
docker images

# give all images id
docker images -q

# delete all images
docker rmi $(docker images -q)

# See all running containers
docker ps
or
docker container ls

# See all the containers
docker ps -a

# Stop the container
docker stop container_id or container_name

# See all the details
docker inspect container_id

# See logs of the container
docker logs container_id

shows logs only 5s before
docker logs --since 5s container_id

# commit changes in container
docker commit -m "message" container_id image_name:tag

# If you want to attach bash shell in your running container
docker container exec -it container_id bash

# run container
docker run --name any_given_name_container -p local_port:container_port -d image_name or image_id
note: If you add -d it will detach terminal to listning to terminal, Essentially, you run container in the background.

while using docker run, it runs the image with a brand new container


# run a container without creating new
docker start


# Layer caching

COPY package.json .

RUN npm install

COPY . .


# Managing Images & Containers
- delete not in use image
docker image rm image_name

- delete in use image
docker image rm image_name -f

- delete container
- docker container rm container_name1 container_name2


# Delete all Images, Container and volumes
docker system prune -a

# Delete all the stoped container
docker container prune -f


# Volumes

install a watcher globally : for nodejs nodemon
at top install nodemon globally
RUN npm install -g nodemon
inside package.json  'dev': 'nodemon -L app.js'; -L because we're runing docker inside windose
CMD ['npm', 'run', 'dev']

While running a container
docker run --name container_name -p 4000:4000 --rm -v absolute_path_of_the_local_project_dir:container_work_dir -v /app/node_modules image_name:tag_name

# Docker compose
docker-compose.yaml
version: "3.8"
services:
   project_name: 
      build: relative_path_to_the_folder_where_Dockerfile_is
      container_name: container_name
      ports:
         - '4000:4000'
      volumens:
         - ./api:/app (relative path to the project: container path)
         - ./app/node_modules


inside you root folder
- run the container
docker-compose up

- stop the container
docker-compose down

- delete all images
docker-compose down --rmi all


# Docker new update 2024

1. Docker Scout
2. docker init
3. docker compose watch
4. Docker Build Cloud
5. Docker Debug

# What next to learn?
docker compose
docker volumes
docker networking
docker file best practices
docker swarm
Architecture of docker engin
docker client --> daemon --> containerd  --> shim, runc
                                         --> shim, runc

TLS

