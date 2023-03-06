---
title: " Build Own Dockerfile and Docker Compose with Custom configuration "
description: " Build own image "
---

#### Build Own Dockerfile and Docker Compose with Custom configuration


crate docker compose file with following content 

```Dockerfile 
version: '3'
services:
  phpapp:
    build:
      context: ./
      dockerfile: Dockerfile
    image: phpapp:123
    ports:
      - "8080:80"
    volumes:
      - "./:/var/www/html"
    container_name: my-php-app

```

- here using dockerfile to generate an image 
- mount folder inside your directory automatically 

#### create index.php with following content 
```php 
<?php

phpinfo();

```

#### build docker compose file 

```bash
 3-DC-Custom git:(main) ✗ docker compose up 
[+] Running 0/1
 ⠿ phpapp Warning                                                    3.5s
[+] Building 6.4s (7/7) FINISHED                                          
 => [internal] load .dockerignore                                    0.0s
 => => transferring context: 2B                                      0.0s
 => [internal] load build definition from Dockerfile.1               0.0s
 => => transferring dockerfile: 58B                                  0.0s
 => [internal] load metadata for docker.io/library/php:7.2-apache    4.7s
 => [auth] library/php:pull token for registry-1.docker.io           0.0s
 => CACHED [1/1] FROM docker.io/library/php:7.2-apache@sha256:4dc0f  0.0s
 => => resolve docker.io/library/php:7.2-apache@sha256:4dc0f0115acf  0.0s
 => exporting to docker image format                                 1.7s
 => => exporting layers                                              0.0s
 => => exporting manifest sha256:0ba34bb4f00dd5ff9830453180af98fe10  0.0s
 => => exporting config sha256:a1c8ee68f9b1abcb72605f5bf0eee69dbf23  0.0s
 => => sending tarball                                               1.6s
 => importing to docker                                              0.0s
[+] Running 2/2
 ⠿ Network 3-dc-custom_default  Cre...                               0.1s
 ⠿ Container my-php-app         Created                              0.1s
Attaching to my-php-app


```

#### check it out localhost 80 


http://localhost:8080


update your existing Dockerfile with following content 

```dockerfile 
FROM php:7.2-apache

RUN apt-get -y update \
&& apt-get install -y libicu-dev \ 
&& docker-php-ext-configure intl \
&& docker-php-ext-install intl

RUN docker-php-ext-install mysqli && docker-php-ext-enable mysqli


```

update existing docker-compose.yml 

```bash
version: '3'
services:
  phpapp:
    build:
      context: ./
      dockerfile: Dockerfile.2
    image: phpapp:123
    ports:
      - "8080:80"
    volumes:
      - "./:/var/www/html"
    container_name: my-php-app

```

#### rebuild docker compose 

```sh
3-DC-Custom git:(main) ✗ docker compose up --build
[+] Building 43.1s (9/9) FINISHED                                         
 => [internal] load .dockerignore                                    0.0s
 => => transferring context: 2B                                      0.0s
 => [internal] load build definition from Dockerfile.2               0.0s
 => => transferring dockerfile: 255B                                 0.0s
 => [internal] load metadata for docker.io/library/php:7.2-apache    2.3s
 => [auth] library/php:pull token for registry-1.docker.io           0.0s
 => CACHED [1/3] FROM docker.io/library/php:7.2-apache@sha256:4dc0f  0.0s
 => => resolve docker.io/library/php:7.2-apache@sha256:4dc0f0115acf  0.0s
 => [2/3] RUN apt-get -y update && apt-get install -y libicu-dev &  28.9s
 => [3/3] RUN docker-php-ext-install mysqli && docker-php-ext-enabl  6.9s
 => exporting to docker image format                                 4.9s 
 => => exporting layers                                              2.3s 
 => => exporting manifest sha256:ee6a98e1847d8149754239ab5be0cad310  0.0s 
 => => exporting config sha256:2800277bcab2cb605936b4f874bff2c8251d  0.0s 
 => => sending tarball                                               2.6s 
 => importing to docker                                              0.5s 
[+] Running 1/1
 ⠿ Container my-php-app  Recreated                                   0.1s
Attaching to my-php-app
my-php-app  | AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 172.22.0.2. Set the 'ServerName' directive globally to suppress this message
my-php-app  | AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 172.22.0.2. Set the 'ServerName' directive globally to suppress this message
my-php-app  | [Fri Mar 03 11:55:09.580207 2023] [mpm_prefork:notice] [pid 1] AH00163: Apache/2.4.38 (Debian) PHP/7.2.34 configured -- resuming normal operations
my-php-app  | [Fri Mar 03 11:55:09.580274 2023] [core:notice] [pid 1] AH00094: Command line: 'apache2 -D FOREGROUND'
my-php-app  | 172.22.0.1 - - [03/Mar/2023:11:55:14 +0000] "GET /mysql HTTP/1.1" 404 490 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/16.3 Safari/605.1.15"
my-php-app  | 172.22.0.1 - - [03/Mar/2023:11:55:19 +0000] "GET / HTTP/1.1" 200 23831 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/16.3 Safari/605.1.15"
¸^CGracefully stopping... (press Ctrl+C again to force)
Aborting on container exit...
[+] Running 1/1
 ⠿ Container my-php-app  Stopped                                     1.2s
canceled


```