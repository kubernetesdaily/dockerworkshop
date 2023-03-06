---
title: " Dockerfile Lab - Volume instruction "
description: " Dockerfile Lab 8 "
---

cat dockerfile.vol 

```Dockerfile
FROM alpine
RUN mkdir /myvol
RUN echo "hello world" > /myvol/greeting
VOLUME /myvol
CMD ["sh"]

```

#### Build above dockerfile 



```sh
docker build -t sangam14/vol-dockerfile  -f dockerfile.vol .            
[+] Building 0.4s (7/7) FINISHED                                                                                                                           
 => [internal] load build definition from dockerfile.vol                                                                                              0.0s
 => => transferring dockerfile: 140B                                                                                                                  0.0s
 => [internal] load .dockerignore                                                                                                                     0.0s
 => => transferring context: 2B                                                                                                                       0.0s
 => [internal] load metadata for docker.io/library/alpine:latest                                                                                      0.0s
 => CACHED [1/3] FROM docker.io/library/alpine                                                                                                        0.0s
 => [2/3] RUN mkdir /myvol                                                                                                                            0.2s
 => [3/3] RUN echo "hello world" > /myvol/greeting                                                                                                    0.2s
 => exporting to image                                                                                                                                0.0s
 => => exporting layers                                                                                                                               0.0s
 => => writing image sha256:7d9ebad9eb42a7006dbb89c1a544e5fcbce3de83cb470210b89adf4ffc4670db                                                          0.0s
 => => naming to docker.io/sangam14/vol-dockerfile                                                                                                    0.0s
```

#### run dockerg

```sh
docker  run --rm -it --mount source=myvolsrc,target=/myvol sangam14/vol-dockerfile 
```

output 

```sh
docker  run --rm -it --mount source=myvolsrc,target=/myvol sangam14/vol-dockerfile   
/ # ls
bin    dev    etc    home   lib    media  mnt    myvol  opt    proc   root   run    sbin   srv    sys    tmp    usr    var
/ # cat myvol/greeting 
hello world

```
#### check it out all mounted volumes 

```sh
docker volume ls

```
output 

```sh
Dockerfile git:(main) ✗ docker volume ls
DRIVER    VOLUME NAME
local     myvolsrc

```

#### run container 

```sh
docker  run --rm -d --name vol-demo sangam14/vol-dockerfile tail -f /dev/null
76a5bfedc0a43bde7f2788fec5e5aafaa94854f16f960ed736ba598b80560f8d

```

```sh
docker ps
CONTAINER ID   IMAGE                       COMMAND                  CREATED              STATUS              PORTS     NAMES
76a5bfedc0a4   sangam14/vol-dockerfile     "tail -f /dev/null"      About a minute ago   Up About a minute             vol-demo
```
#### stop running container 

```sh
docker container stop vol-demo
vol-demo

```

#### lets run container 

```sh
docker  run -d --name vol-demo --mount source=myvolsrc,target=/myvol  sangam14/vol-dockerfile  tail -f /dev/null
```
#### exec into running container 

```sh
 Dockerfile git:(main) ✗ docker container exec vol-demo ls -l /myvol
total 4
-rw-r--r--    1 root     root            12 Mar  2 19:47 greeting

```
#### check it out mount point using docker inspect 

```sh

docker volume inspect myvolsrc -f "{{.Mountpoint}}"
```

#### stop running container 

```sh
docker container stop vol-demo
```
#### remove container 

```sh
docker container rm vol-demo

```
#### remove volume 

```sh
docker volume rm myvolsrc
```
#### verify once 

```sh
docker volume ls
docker container ls
```
