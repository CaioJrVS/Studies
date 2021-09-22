###Docker

---

#### How docker works


Docker is a tool to virtualize images created from code.

Basically, we write code, like a backend API, then we create an image, that is
all the files that compose our API, the environment settings, 
the source code, the dependencies, libraries.

With that image we can tell docker to create a container from that image 
using the Dockerfile, where me set a layered commands to tell docker how
to run the API.

Docker is not limited to the imaes we build, there's a lot of images mantained
by it's creators on [Docker Hub](https://hub.docker.com/) we add these images
to our applications on the Dockerfile.

---

#### How to install docker

[How to install docker](https://docs.docker.com/get-docker/)

---

#### Dockerfile

The Dockerfile is the file where we set all the commands that a user 
would write on the commandline to setup the application

We have to write a Dockerfile for every unity of software we have to 
create a image for the containers to run

A simple Docker to run a express.js server would look like this:

```

FROM node

WORKDIR /app

COPY . .

RUN npm install

CMD ["npm","start"]

```

Here the FROM is the parent of the image we are building and **every**
Dockerfile must start with the From instruction


---

#### Docker commands

`docker build <path_to_Dockerfile>` creates the image from the Dockerfile especifactions 
for the container to run

`docker docker run [-d] <image_hash>` docker build a image and start a container in a atatched mode
this means that we are listening to the output of the program on the terminal

We can attatch to a detatched container again by runnint `docker container attatch <container_name>`

An alternative to this is `docker logs <container_name>` to print the output of the container 
if we add the flag `-f` is like attatching to the output of the container 

To be able to access the port exposed by a container we have to add `-p <local_port>:<container_port>`
i.e.: if we create a server to listen to the port 80 and then create a container to run the application
we have to run it like `docker run -p 3000:80 [image ID]`

`docker ps` lists the number of containers running

`docker ps -a` lists all containers you had in the path including the stopped containers

`docker images` list all created images

`docker stop <container_name>` stop a process

`docker start <container_name> [-a]` start a stopped container the flag `-a` is to attatch to the container

`docker rm <container_name>` remove a stopped container or delete a image

`docker container prune` removes all stopped containers

`docker rmi <image_id>` removes a image

`docker image prune -a` removes all images that are not being used by containers

To copy files into container or to copy files from a container we can use 

`docker cp <container_name/path_to_file> <path_to_file_outside_container>`

`docker cp <path_to_file_outside_container> <container_name/path_to_file>`

Containers can be named, so you dont have the automatically generated names,
for that you have to specify the `--name` flag on the `docker run` command  and then set the name of your container
like :

`docker run <image_id> --name <container_name>`

Images can be named and it has a lot of advantages, like setting a version i.e.: the node 14 version is node:14.
for that just specify the flag `--tag`

`docker build <path_to_dockerfile> --tag <image_name>:<image_tag>`
