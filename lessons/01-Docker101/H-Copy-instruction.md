---
title: " Dockerfile Lab - COPY instruction "
description: " Dockerfile Lab 2 "
---

COPY is a dockerfile command that copies files from a local source location to a destination in the Docker container. A Dockerfile is a text file with instructions to set up a Docker container.

create myfile1.txt and myfile2.txt with following content :

```sh
# myfile1.txt
Hello This is my first file !
This is file will be copied in /usr/share directory from Docker host to Docker Container.
```

```sh
# myfile2.txt
Hello This is my second file !
This is file will be copied in /tmp directory from Docker host to Docker Container.

```
The general syntax of the COPY command is:

```sh
COPY <src> <dest>

```
Here, `<src>` and `<dest>`are file paths.` <src>` is the path to the source folder containing files to be copied. This option can be left empty to copy the contents of the current directory. The source of the files has to be a directory on the local computer.

`<dest>` is the destination of the COPY command inside the docker container. This is the path where files are to be copied.

```Dockerfile
# Instruction for Dockerfile to create a new image on top of the base image (ubuntu)
# Using the base image ubuntu: latest
FROM ubuntu:latest
# Copying myfile1.txt to the containers /usr/share directory
COPY myfile1.txt /usr/share
# Copying myfile2.txt to the containers /tmp directory
COPY myfile2.txt /tmp

```
### Build Dockerfile using following Command 

```sh
  Dockerfile git:(main) ✗ docker build -t sangam14/copy-dockerfile  -f dockerfile.copy .
[+] Building 2.2s (9/9) FINISHED                                                                                                                                                                  
 => [internal] load build definition from dockerfile.copy                                                                                                                                    0.0s
 => => transferring dockerfile: 356B                                                                                                                                                         0.0s
 => [internal] load .dockerignore                                                                                                                                                            0.0s
 => => transferring context: 2B                                                                                                                                                              0.0s
 => [internal] load metadata for docker.io/library/ubuntu:latest                                                                                                                             2.1s
 => [auth] library/ubuntu:pull token for registry-1.docker.io                                                                                                                                0.0s
 => CACHED [1/3] FROM docker.io/library/ubuntu:latest@sha256:9a0bdde4188b896a372804be2384015e90e3f84906b750c1a53539b585fbbe7f                                                                0.0s
 => [internal] load build context                                                                                                                                                            0.0s
 => => transferring context: 344B                                                                                                                                                            0.0s
 => [2/3] COPY myfile1.txt /usr/share                                                                                                                                                        0.0s
 => [3/3] COPY myfile2.txt /tmp                                                                                                                                                              0.0s
 => exporting to image                                                                                                                                                                       0.0s
 => => exporting layers                                                                                                                                                                      0.0s
 => => writing image sha256:4c660d66bd5f94311a22be23394032e2f2dd45f40fb4831f8e083efe90488763                                                                                                 0.0s
 => => naming to docker.io/sangam14/copy-dockerfile    

 ```

 #### check inside container and search for text file 

```sh
➜  Dockerfile git:(main) ✗ docker run -it sangam14/copy-dockerfile bash                  
root@27a3fbe098c3:/# ls
bin  boot  dev  etc  home  lib  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@27a3fbe098c3:/# cat /usr/share/myfile1.txt 
# myfile1.txt
Hello This is my first file !
This is file will be copied in /usr/share directory from Docker host to Docker Container.root@27a3fbe098c3:/# ls
bin  boot  dev  etc  home  lib  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@27a3fbe098c3:/# cat /tmp/myfile2.txt 

# myfile2.txt
Hello This is my second file !
This is file will be copied in /tmp directory from Docker host to Docker Container.root@27a3fbe098c3:/# 

```
both file successfully copied inside container 





