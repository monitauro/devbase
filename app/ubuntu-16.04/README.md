### SUBINDO CONTEINER COMO DAEMON
docker-compose up -d

### LOGIN NO CONTEINER 
docker-compose exec app bash
docker exec -it db bash

### REMOVER TODOS OS CONTEINERS
docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q)


docker rmi $(docker images -q) --force