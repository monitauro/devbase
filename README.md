# devbase
devbase


## Build command

docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q)
docker images -a | grep -vE "ubuntu" | awk '{print $3}' | xargs docker rmi
docker build -t docker-base .
docker run -i -t docker-base bash

docker tag local-image:tagname new-repo:tagname
docker push new-repo:tagname


docker tag docker-base:latest monitauro/ubuntu-16.04-base:latest
docker push monitauro/ubuntu-16.04-base:latest

## Build command
docker build -t monitauro/ubuntu-16.04:php-7.x .

## UP command
docker run -d --name php docker-php 

## LOGIN command
docker exec -it php bash

## OTHER commands
docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q)

docker images -a | grep -vE "ubuntu" | awk '{print $3}' | xargs docker rmi

docker images -a | awk '{print $3}' | xargs docker rmi

docker rmi $(docker images -q) --force
