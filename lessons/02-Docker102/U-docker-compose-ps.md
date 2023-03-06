---
title: " Docker Compose CLI - ps Command "
description: " List containers "
---

#### List containers

```sh
 docker compose ps 
NAME                          IMAGE                       COMMAND                  SERVICE             CREATED             STATUS                       PORTS
example-voting-app-db-1       postgres:15-alpine          "docker-entrypoint.s…"   db                  4 hours ago         Up About an hour (healthy)   5432/tcp
example-voting-app-redis-1    redis:alpine                "docker-entrypoint.s…"   redis               4 hours ago         Up About an hour (healthy)   6379/tcp
example-voting-app-result-1   example-voting-app-result   "nodemon server.js"      result              About an hour ago   Up About an hour             0.0.0.0:5858->5858/tcp, 0.0.0.0:5001->80/tcp
example-voting-app-worker-1   example-voting-app-worker   "dotnet Worker.dll"      worker              About an hour ago   Up About an hour             
➜  example-voting-app git:(main) 

````
