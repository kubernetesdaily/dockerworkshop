---
title: " Docker Compose CLI - Down Command "
description: " Stop and remove containers, networks "
---



### create docker-compose.yml with following content 

```yml
version: "3.9"
services:
  web:
    build: .
    ports:
      - "8000:5000"
  redis:
    image: "redis:alpine"

```


### Create docker compose up 

```sh

 docker compose up 
WARN[0000] Found orphan containers ([7-dc-cli-client-1]) for this project. If you removed or renamed this service in your compose file, you can run this command with the --remove-orphans flag to clean it up. 
Attaching to 7-dc-cli-redis-1, 7-dc-cli-web-1
7-dc-cli-redis-1  | 1:C 04 Mar 2023 14:23:46.156 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
7-dc-cli-redis-1  | 1:C 04 Mar 2023 14:23:46.156 # Redis version=7.0.9, bits=64, commit=00000000, modified=0, pid=1, just started
7-dc-cli-redis-1  | 1:C 04 Mar 2023 14:23:46.156 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
7-dc-cli-redis-1  | 1:M 04 Mar 2023 14:23:46.157 * monotonic clock: POSIX clock_gettime
7-dc-cli-redis-1  | 1:M 04 Mar 2023 14:23:46.158 * Running mode=standalone, port=6379.
7-dc-cli-redis-1  | 1:M 04 Mar 2023 14:23:46.158 # Server initialized
7-dc-cli-redis-1  | 1:M 04 Mar 2023 14:23:46.160 * Ready to accept connections
7-dc-cli-web-1    |  * Serving Flask app 'app.py'
7-dc-cli-web-1    |  * Debug mode: off
7-dc-cli-web-1    | WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
7-dc-cli-web-1    |  * Running on all addresses (0.0.0.0)
7-dc-cli-web-1    |  * Running on http://127.0.0.1:5000
7-dc-cli-web-1    |  * Running on http://172.18.0.3:5000
7-dc-cli-web-1    | Press CTRL+C to quit


```

### Create docker compose Down  

```sh
 7-DC-CLI git:(main) ✗ docker compose down 
[+] Running 3/3
 ⠿ Container 7-dc-cli-redis-1  Removed                                                                                                                                               0.2s
 ⠿ Container 7-dc-cli-web-1    Removed                                                                                                                                              10.2s
 ⠿ Network 7-dc-cli_default    Removed                                                                                                                                               0.1s
➜  7-DC-CLI git:(main) ✗ 


```