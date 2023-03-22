docker ps
docker ps -a
docker pull <image-name>
docker run <image-name>
docker rm <id / name>
docker rmi <image-name/ image-id>

docker ps -a -q   # will get list of id's of container which are running/stopped
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)

docker inspect <id>

-d  :  detach mode
-p  : publish   <host-ip>:<container-ip>
-e  : environment variable

8080 - host machine port
80 - container port
docker run -d -p 8080:80 nginx

docker run -name mysql \
	-network custom \
	-e MYSQL_USER=wordpress \
	-e MYSQL_PASSWORD=wordpress \
	-e MYSQL_ROOT_PASSWORD=my-secret-password \
	-e MYSQL_DATABASE=wordpress \
	-d mysql

1. smallest image - alpine
This command will create and exit the container immidiately 
docker run -d node:alpine

-it  :  
This command will create a container
docker run -d -it node:alpine

sh : sha shell
--------
docker exec -it <id> sh
node --version
node
process.env
ctrl+c or ctrl+D
-----------

docker run -d -it -e MY_TEST_ENV=sowmya node:alpine
docker exec -it <container-id> sh
node
process.env


======================================
Volumes and volumes mapping
=====================================
-v ${PWD}/html:/usr/share/nginx/html
-v <file-path-to-local-host>:<file-path-inside-container>

docker run -dit -p 8888:80 -v ${PWD}/html:/usr/share/nginx/html nginx

docker inspect <container-id>

--------------------------------------

docker run -it -v ${PWD}:/app python:alpine python /app/main.py

=======================================================================================
DOCKER FILE
-----------

Dockerfile  ->  docker build  ->  Image

Inorder to push to docker hub, should specify image name like
<dockerhub-username>/my-image:tag

docker build . -t my-image:tag

docker        - Reference to the Docker client
build         - Build an image from the Dockerfile
.             -  Location of the Dockerfile
-t            -  Add name with tag for the new image
my-image:tag  - Name of the new image




Dockerfile:

FROM ubuntu   (base image which will be utilise custom image)
RUN apt-get update  
CMD ["echo", "Welcome!"]   (This commad will executed, when new container will created based on this newly created                      image)

---------------------------------------

Sample Python application which writes/reads from the MongoDB

FROM python:alpine
WORKDIR /app
RUN pip install pymongo
COPY . /app
CMD ["python", "main.py"]


docker build . -t python-custom

# Incase if you wnat ot push the image to docker-hub
docker build . -t <dockerhub-username>/python-custom

docker run -it python-custom

docker ps -a

docker run -it python-custom sh


=> Create mongo container:
--name  :  custom name of the container

docker run -d --name=mongo  mongo
docker logs mongo
docker inspect mongo

=> connect python-custom container and mongo container

docker run -it python-custom  # getting error

docker run -it python-custom sh
ping mongo  # not connecting
ping 172.17.0.2  # ip address of mongo container, it is connecting

docker network list

=> Here we need to create new custom network and attach to it
Attaching containers to the custom bridge network
Connecting two containers by their names using custom network

docker network create python-mongo
docker network list

=> Docker runs internal DNS server which resolves names and IDs of the containers to their IP addresses

docker network connect python-mongo mongo  # attach mongo container to python-mongo network (container which        is already running)
docker inspect mongo
docker inspect mongo | grep python-mongo


# python-mongo container is stopped, so need to start with attaching network

docker run -it --network=python-mongo python-custom

docker run -it --network=python-mongo python-custom sh
ping mongo # connection is successfull

docker logs mongo

===============================================================
Docker Compose and YAML
===============================================================

version: '3'

services:
	app:
		build: ./app
	mongo:
		image: mongo
		ports:
			- 27017:27017

---------------------------------------------
REMOVE/DELETE:

docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
docker rmi $(docker images -q)

docker network list
docker network remove python-mongo
----------------------------------------------

docker compose up

docker compose down


docker compose up --build  # --build it will not use any pre build images

==========================================

docker volume list


















