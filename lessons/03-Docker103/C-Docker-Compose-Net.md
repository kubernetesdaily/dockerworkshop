---
title: " Newtorking in Docker Compose "
description: " Docker Compose Newtorking "
---

### creating and using networks in docker-compose.yml

```
version: "3.7"
services:
  app1:
    image: httpd:latest
    container_name: app1
    ports:
     - 8080:80
    networks:
      - app1_net
networks:
  app1_net:

```

### run docker compose up 

```
docker-compose up

```

### Open browser 

```
http://localhost:8080
```

Observe the output

### On a second terminal
```
docker ps
```
#### Observe the networking part!

```
docker inspect app1

```

### Stops the docker-compose
```
docker-compose rm
```


