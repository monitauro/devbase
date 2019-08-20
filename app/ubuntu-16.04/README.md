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
docker build -t devphp .

## UP command
docker run -p 9001:9001 -p 80:80  -p 3306:3306 -d --name devphp_env devphp 
docker run -p <host_port1>:<container_port1> -p <host_port2>:<container_port2>

## LOGIN command
docker exec -it devphp_env bash

## OTHER commands
docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q)

docker images -a | grep -vE "ubuntu" | awk '{print $3}' | xargs docker rmi

docker images -a | awk '{print $3}' | xargs docker rmi

docker rmi $(docker images -q) --force


``` sh
# run application
docker run \
    -p 9001:9001 -p 80:80  -p 3306:3306 \
    -v /$(pwd):/app \
    -v /$(pwd)/docker/nginx.conf:/etc/nginx/sites-enabled/default:ro \
     -d --name devphp_env devphp 
```


### PHP application example

``` sh
# Host system
$ tree
.
├── docker
│   └── nginx.conf
└── public
    └── index.php

2 directories, 2 files
```

``` php
<?php
// index.php
phpinfo();
```

``` nginx
# nginx.conf
server {
    listen 80 default_server;

    server_name example.com;

    index index.php;
    root /app/public;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass             unix:/run/php/php7.2-fpm.sock;
        fastcgi_index            index.php;

        include                  fastcgi_params;

        fastcgi_param            SCRIPT_FILENAME $document_root$fastcgi_script_name;

        fastcgi_intercept_errors on;
        fastcgi_read_timeout     300;
        fastcgi_buffer_size      16k;
        fastcgi_buffers          4 16k;
    }
}
```