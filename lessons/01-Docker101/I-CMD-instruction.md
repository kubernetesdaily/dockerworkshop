---
title: " Dockerfile Lab - CMD instruction "
description: " Dockerfile Lab 3 "
---


The CMD command we saw earlier followed the Shell syntax:

```bash
CMD executable parameter1 parameter2
```

However, it is better practice to use the JSON array format:
```json
CMD ["executable", "parameter1", "parameter2"]
```

A CMD command can be overridden by providing the executable and its parameters in the docker ​run command. For example:


```dockerfile 
FROM ubuntu
RUN apt-get update
CMD ["echo" , "Join CloudNativeFolks Community"]
```
#### build dockerfile 

```
 docker build -t sangam14/cmd-dockerfile  -f dockerfile.cmd . 
[+] Building 8.3s (7/7) FINISHED                                                                                                                                                                  
 => [internal] load build definition from dockerfile.cmd                                                                                                                                     0.0s
 => => transferring dockerfile: 125B                                                                                                                                                         0.0s
 => [internal] load .dockerignore                                                                                                                                                            0.0s
 => => transferring context: 2B                                                                                                                                                              0.0s
 => [internal] load metadata for docker.io/library/ubuntu:latest                                                                                                                             2.1s
 => [auth] library/ubuntu:pull token for registry-1.docker.io                                                                                                                                0.0s
 => CACHED [1/2] FROM docker.io/library/ubuntu@sha256:9a0bdde4188b896a372804be2384015e90e3f84906b750c1a53539b585fbbe7f                                                                       0.0s
 => [2/2] RUN apt-get update                                                                                                                                                                 6.0s
 => exporting to image                                                                                                                                                                       0.1s
 => => exporting layers                                                                                                                                                                      0.1s
 => => writing image sha256:c59a693968aab28243f6852d49be7299e0035e71e39b42f22c07be49cca74fb2                                                                                                 0.0s
 => => naming to docker.io/sangam14/cmd-dockerfile 

```

### run docker container 

```
Dockerfile git:(main) ✗ docker run sangam14/cmd-dockerfile 
Join CloudNativeFolks Community

```

