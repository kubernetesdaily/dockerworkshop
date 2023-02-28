---
title: " Dockerfile Lab - Entrypoint instruction "
description: " Dockerfile Lab 4 "
---

#### Running a Docker Container with ENTRYPOINT

Let's learn the details in this case by actually executing ENTRYPOINT in exec form. The following is an example of a Dockerfile that uses the exec form of ENTRYPOINT, which outputs a character string on the command line.

```dockerfile 
FROM alpine
ENTRYPOINT ["echo", "Hello!"]

```

#### Build dockerfile 

```
➜  Dockerfile git:(main) ✗ docker build -t sangam14/entrypoint-dockerfile  -f dockerfile.entrypoint .
[+] Building 3.2s (6/6) FINISHED                                                                                            
 => [internal] load build definition from dockerfile.entrypoint                                                        0.0s
 => => transferring dockerfile: 94B                                                                                    0.0s
 => [internal] load .dockerignore                                                                                      0.0s
 => => transferring context: 2B                                                                                        0.0s
 => [internal] load metadata for docker.io/library/alpine:latest                                                       3.1s
 => [auth] library/alpine:pull token for registry-1.docker.io                                                          0.0s
 => [1/1] FROM docker.io/library/alpine@sha256:69665d02cb32192e52e07644d76bc6f25abeb5410edc1c7a81a10ba3f0efb90a        0.0s
 => => resolve docker.io/library/alpine@sha256:69665d02cb32192e52e07644d76bc6f25abeb5410edc1c7a81a10ba3f0efb90a        0.0s
 => => sha256:69665d02cb32192e52e07644d76bc6f25abeb5410edc1c7a81a10ba3f0efb90a 1.64kB / 1.64kB                         0.0s
 => => sha256:c41ab5c992deb4fe7e5da09f67a8804a46bd0592bfdf0b1847dde0e0889d2bff 528B / 528B                             0.0s
 => => sha256:d74e625d91152966d38fe8a62c60daadb96d4b94c1a366de01fab5f334806239 1.49kB / 1.49kB                         0.0s
 => exporting to image                                                                                                 0.0s
 => => exporting layers                                                                                                0.0s
 => => writing image sha256:0d5a798a648339f8ea8094b10568eb2dc44540480deff55f680dfd689f787013                           0.0s
 => => naming to docker.io/sangam14/entrypoint-dockerfile                                                              0.0s
 ```

#### run docker container 

```
 Dockerfile git:(main) ✗ docker run sangam14/entrypoint-dockerfile 
Hello!
➜  Dockerfile git:(main) ✗ docker run sangam14/entrypoint-dockerfile echo "sangam"
Hello! echo sangam
➜  Dockerfile git:(main) ✗ 

```

#### Overwrite with `--entrypoint` option

On the other hand, in ENTRYPOINT, you can change the instruction by using the option of `—entrypoint` as follows.

```
docker run --rm --entrypoint sh sangam14/entrypoint-dockerfile  -c 'echo "test"'
test

```

CMD and ENTRYPOINT have similar roles and are confusing, but they have different functions. CMD, ENTRYPOINT, and ENTRYPOINT also behave differently between shell form and exec form, so it's a good idea to use each function properly. The instructions in the Dockerfile are a bit complicated, but you can use them effectively if you understand them.


