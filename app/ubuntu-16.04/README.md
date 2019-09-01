### SUBINDO CONTEINER COMO DAEMON
docker-compose up -d

### REMOVER TODOS OS CONTEINERS
docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q)


docker rmi $(docker images -q) --force