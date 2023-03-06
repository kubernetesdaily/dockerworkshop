---
title: "Images and Container As Tar file"
description: " share as tar "
---

#### Images and Container as Tarfile 

Docker provides the ability to save images and containers as tar files, which can be useful for sharing with others or transferring between systems. Here's how to do it:


```sh
docker pull nginx:latest
latest: Pulling from library/nginx
5731adb3a4ab: Pull complete 
8785c8f663d3: Pull complete 
023b6bd393e4: Pull complete 
fd8f86b165b0: Pull complete 
8f41e7c12976: Pull complete 
3b5338ea7d08: Pull complete 
Digest: sha256:6650513efd1d27c1f8a5351cbd33edf85cc7e0d9d0fcb4ffb23d8fa89b601ba8
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest

```

#### Saving an Image as a Tar File

To save a Docker image as a tar file, use the docker save command with the image name and output file name:

```sh
dockerworkshop git:(main) ✗ docker container run -it ubuntu:14.04 bash
Unable to find image 'ubuntu:14.04' locally
14.04: Pulling from library/ubuntu
d1a5a1e51f25: Pull complete 
75f8eea31a63: Pull complete 
a72d031efbfb: Pull complete 
Digest: sha256:64483f3496c1373bfd55348e88694d1c4d0c9b660dee6bfef5e12f43b9933b30
Status: Downloaded newer image for ubuntu:14.04
root@906d9f72e9fe:/# exit
exit

```

```sh
➜  dockerworkshop git:(main) ✗ docker ps
CONTAINER ID   IMAGE                       COMMAND                  CREATED             STATUS             PORTS     NAMES
1bf183201392   ubuntu:14.04                "bash"                   15 seconds ago      Up 14 seconds                loving_ride
```

```sh
docker export 1b  > os.tar
docker export loving_ride  > os1.tar
dockerworkshop git:(main) ✗ ls
os.tar   os1.tar
```


docker load is a command used to load images or container archives that were previously saved using the docker save command.

When you use the docker save command, it creates a tar archive of one or more Docker images and/or containers. You can then use the docker load command to load this tar archive back into Docker.

The syntax for using the docker load command is as follows:

```sh
➜  dockerworkshop git:(main) ✗ docker save -o os.tar ubuntu  
➜  dockerworkshop git:(main) ✗ docker load < os.tar            
Loaded image: ubuntu:14.04
```
