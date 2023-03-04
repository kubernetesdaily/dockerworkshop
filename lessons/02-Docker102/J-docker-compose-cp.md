---
title: " Docker Compose CLI - CP Command "
description: " Copy files/folders between a service container and the local filesystem "
---

### Copy files/folders between a service container and the local filesystem

```
# Syntax to Copy from Container to Docker Host  
docker cp {options} CONTAINER:SRC_PATH DEST_PATH 
# Syntax to Copy from Docker Host to Container  
docker cp {options} SRC_PATH CONTAINER:DEST_PATH 

```

#### lets run ngnix container 


```
version: '3'
services:
  web:
    image: nginx:latest

```

### run docker compose up 
```
docker compose -f docker-compose-ngnix.yml up 

```

### check running container  
```
➜  dockerworkshop git:(main) ✗ docker ps
CONTAINER ID   IMAGE                           COMMAND                  CREATED              STATUS              PORTS     NAMES
008940fdbed8   nginx:latest                    "/docker-entrypoint.…"   29 seconds ago       Up 28 seconds       80/tcp    7-dc-cli-web-1
```

### create index.htm
```

 Docker-Compose git:(main) ✗ cd 7-DC-CLI 
➜  7-DC-CLI git:(main) ✗ ls
Dockerfile               app.py                   docker-compose-ngnix.yml docker-compose.yml       index.html               requirements.txt         src
```
### copy index.html

```
$ docker-compose cp index.html web:/usr/share/nginx/html/

```