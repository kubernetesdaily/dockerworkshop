---
title: " Docker Compose CLI - Exec Command "
description: " Execute a command in a running container. "
---

### Docker Compose Exec into result service 


```


docker compose exec result sh 
# ls
Dockerfile  docker-compose.test.yml  package-lock.json  package.json  server.js  tests  views
# cat docker-compose.test.yml
version: '2'

services:

  sut:
    build: ./tests/
    depends_on:
      - vote
      - result
      - worker
    networks:
      - front-tier

  vote:
    build: ../vote/
    ports: ["80"]
    depends_on:
      - redis
      - db
    networks:
      - front-tier
      - back-tier

  result:
    build: .
    ports: ["80"]
    depends_on:
      - redis
      - db
    networks:
      - front-tier
      - back-tier

  worker:
    build: ../worker/
    depends_on:
      - redis
      - db
    networks:
      - back-tier

  redis:
    image: redis:alpine
    networks:
      - back-tier

  db:
    image: postgres:9.4
    environment:
      POSTGRES_USER: "postgres"
      POSTGRES_PASSWORD: "postgres"
    volumes:
      - "db-data:/var/lib/postgresql/data"
    networks:
      - back-tier

volumes:
  db-data:

networks:
  front-tier:
  back-tier:
# exit
➜  example-voting-app git:(main) 


```

