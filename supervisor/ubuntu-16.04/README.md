## Build command
docker build -t devbase-supervisor .

## UP command
docker run -d --name devbase-supervisor monitauro/devbase-supervisor:ubuntu-16.04

## LOGIN command
docker exec -it devbase-supervisor bash

## PUSH
docker push monitauro/devbase-bootstrap:ubuntu-16.04

## OTHER commands
docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q)

docker images -a | grep -vE "ubuntu" | awk '{print $3}' | xargs docker rmi

docker images -a | awk '{print $3}' | xargs docker rmi

docker rmi $(docker images -q) --force

