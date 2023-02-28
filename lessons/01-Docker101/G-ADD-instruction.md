---
title: " Dockerfile Lab - ADD instruction "
description: " Dockerfile Lab 1 "
---

Here's an example of a Dockerfile that uses the ADD instruction to copy a local file into a Docker image:


```Dockerfile
FROM ubuntu:latest

WORKDIR /app

ADD example.txt /app/

CMD ["cat", "/app/example.txt"]

```

### Build Dockerfile 


```bash

 Dockerfile git:(main) ✗ docker build -t sangam14/add-dockerfile  -f dockerfile.add .  
[+] Building 5.2s (9/9) FINISHED                                                                                                                                                                  
 => [internal] load build definition from dockerfile.add                                                                                                                                     0.0s
 => => transferring dockerfile: 131B                                                                                                                                                         0.0s
 => [internal] load .dockerignore                                                                                                                                                            0.0s
 => => transferring context: 2B                                                                                                                                                              0.0s
 => [internal] load metadata for docker.io/library/ubuntu:latest                                                                                                                             3.1s
 => [auth] library/ubuntu:pull token for registry-1.docker.io                                                                                                                                0.0s
 => [1/3] FROM docker.io/library/ubuntu:latest@sha256:9a0bdde4188b896a372804be2384015e90e3f84906b750c1a53539b585fbbe7f                                                                       2.0s
 => => resolve docker.io/library/ubuntu:latest@sha256:9a0bdde4188b896a372804be2384015e90e3f84906b750c1a53539b585fbbe7f                                                                       0.0s
 => => sha256:9a0bdde4188b896a372804be2384015e90e3f84906b750c1a53539b585fbbe7f 1.13kB / 1.13kB                                                                                               0.0s
 => => sha256:61bd0b97000996232eb07b8d0e9375d14197f78aa850c2506417ef995a7199a7 424B / 424B                                                                                                   0.0s
 => => sha256:a6be1f66f70f66ef43503292e38ccbfc14f2d5464e7736344783a8fc7bb339a8 2.31kB / 2.31kB                                                                                               0.0s
 => => sha256:8b150fd943bcd54ef788cece17523d19031f745b099a798de65247900d102e18 27.34MB / 27.34MB                                                                                             1.4s
 => => extracting sha256:8b150fd943bcd54ef788cece17523d19031f745b099a798de65247900d102e18                                                                                                    0.4s
 => [internal] load build context                                                                                                                                                            0.0s
 => => transferring context: 78B                                                                                                                                                             0.0s
 => [2/3] WORKDIR /app                                                                                                                                                                       0.1s
 => [3/3] ADD example.txt /app/                                                                                                                                                              0.0s
 => exporting to image                                                                                                                                                                       0.0s
 => => exporting layers                                                                                                                                                                      0.0s
 => => writing image sha256:c3438bfac421fa098b47f37ae00427eadcfb7ed36653a678738c63b0ab33a8d1                                                                                                 0.0s
 => => naming to docker.io/sangam14/add-dockerfile               


 ```

 ### run docker images 

```
Dockerfile git:(main) ✗ docker run sangam14/add-dockerfile                                                                                                                                     
Sangam Biradar 
Docker Community Leader 

```