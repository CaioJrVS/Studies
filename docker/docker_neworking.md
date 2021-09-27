### Networking 

---

How to make containers interact with each other, or fetch some API data on the internet 
or access a Database in the machine outside docker

- Docker containers can communicate with API's of the internet without any configurations!

- To access a Database or others API's on the localhost:
	- Instead of access it with "localhost" docker has a special parameter: `host.docker.internal`
	just change the localhost to this parameter and you are good to go

**Http requests to another container**

To access a container from another container we have to find the IP addres of the container
we want to access

For that we can find the IP addres of te container 

But that is a very hardcoded way and we don't want to set a new IP address every time we run 
the image because every container has it's IP address.

**Better way to stablish connection between containers**

A better way to stablish the connection between containers is to create a network and let 
docker resolve the IP address for us

For that: 

First we have to create a network with `docker network create <network_name>`.

With the network created, we then have to put the containers we want to communicate with each other
inside this network i.e: 

`docker network create my-network`

`docker run mongo --name mongodb --network my-network`

`docker run my-API --name my-api --network my-network`

and then, to make the communication between the containers inside the network, instead of setting 
the IP address on the URL, something like "http://172.17.0.1/movies", we put the name of the container
we want to comunicate, "http://mongodb/movies"

**As a side note, note that we didn't specify the flag `-p` when creating the containers
because the `-p` is used to access the container from our localhost machine**
