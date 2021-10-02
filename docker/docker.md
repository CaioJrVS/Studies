### Docker

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

**.dockerignore**

This file is similar to .gitignore and it is used to set the name of the files
that docker will ignore when compying the files specified inside Dockerfile

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

---

#### Publishing Docker images

There's a lot of images repositories, but docker has [dockerhub](https://hub.docker.com/)
and create a account

Then repositories -> create repository -> set a name to the repository

Now we can push ou images to the repository we just create

Type `docker login` on the terminal to login with your docker account 

Create an image that is the same name as the repository and you can set a tag for the name also

Then to push the image created to the repository: `docker push <image_name>` remember that the image name must be the same name as the repository, to change a image tag and name: `docker tag <image_name_to_change> <new_image_name>` 

To pull an image: `docker pull <image_name>`

---

#### Volumes

The container runs an image and all the data stored to the container is gonna be missed when 
the container is removed, just because the container has its own operating system and so it's
own file system and when we save something into it and thenn the container is removed, so is 
the content insed of it.

The solution to this is the Volume, we map a folder inside the container to one of our own 
Operating System so when the container is removed we keep the data

**Annonymous Volumes**

Annonymous Volumes are are created by docker and even though it's mapped to a file inside our
system, it's removed when a container is removed (It's only removed if you add the --rm flag
when creating a container from an image, otherwise it will persist but without any utility because
when we create another container from the same image it will crerate another annonymous volume for 
the new container, to remove an ano).

To create an Annonymous volume just add to the Dockerfile:

`Volume ["<container_volume_path>"]`

**Volumes**

To map a folder inside our application just add `-v <folder_name>:<path_to_folder_in_container>` to the docker run command like:

`docker run <image_id> --rm -p 3000:80 -v feedback:app/feedback`

with this when the container is stopped and removed and run again the data will persist

**Bound Map**

If we set an absolute path on the `<folder_name>` of a file that is inside our container 
the container will use that file and every change we make to the file is going to be 
reflected inside the container 

`docker volume ls` lists all the volumes

`docker volume --help` list the commands for volume management, there's not much 
and it's very easy to understand

--- 

#### Environment variables

To set an environment variable with docker just add the name of the environment
variable inside Docker file i.e.:

```
FROM node:14 

# Other commands

ENV PORT 80

EXPOSE $PORT

```

And then we pass it using the command line with the `-e` command on the `docker run` command command

`docker run <image_id> -e PORT=60 -p 3000:60`

If we have an .env file we can specify the file using `--env-file ./.env`

**Avoid adding security information like connection strings to the image you are building. 
The information is going to persist on the image and people can access using docker image <history>**

---

#### Arguments

We can set arguments to be passed to the image on the command line using Dockerfile
and passing it with the flag `--build-arg`


```
ARG DEFAULT_PORT=80

```

```
docker run <image_id> --build-arg DEFAULT_PORT=60

```


