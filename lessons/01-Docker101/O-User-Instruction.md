---
title: " Dockerfile Lab - USER instruction "
description: " Dockerfile Lab 9 "
---

cat dockerfile.user

```Dockerfile
FROM alpine
USER dockerworkshop:dockerworkshop
CMD ["sh"]

```

#### build dockerfile 

```sh
docker build -t sangam14/user-dockerfile  -f dockerfile.user .
[+] Building 0.1s (5/5) FINISHED                                                                                                                           
 => [internal] load build definition from dockerfile.user                                                                                             0.0s
 => => transferring dockerfile: 105B                                                                                                                  0.0s
 => [internal] load .dockerignore                                                                                                                     0.0s
 => => transferring context: 2B                                                                                                                       0.0s
 => [internal] load metadata for docker.io/library/alpine:latest                                                                                      0.0s
 => CACHED [1/1] FROM docker.io/library/alpine                                                                                                        0.0s
 => exporting to image                                                                                                                                0.0s
 => => exporting layers                                                                                                                               0.0s
 => => writing image sha256:48482459d09aecafaa0db190d6995b6d2339c0383c6cd97fafbea288124332c7                                                          0.0s
 => => naming to docker.io/sangam14/user-dockerfile  

```

#### run docker container in detach mode 

```sh
docker container run -d sangam14/user-dockerfile 
5880a92a14d3944a4be00a19d55a19dd941f0c1b9a7a7b9159febcc29a09ea98

```

If we check the owner of the sleep process on the host, we can see it belongs to the user with uid 1000, the one that is created in the image


```sh
ps aux | grep sleep
sangambiradar    22103   0.0  0.0 408111776   1168 s000  S+    2:03AM   0:00.00 grep --color=auto --exclude-dir=.bzr --exclude-dir=CVS --exclude-dir=.git --exclude-dir=.hg --exclude-dir=.svn --exclude-dir=.idea --exclude-dir=.tox sleep

```
