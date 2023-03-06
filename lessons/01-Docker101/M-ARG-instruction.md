---
title: " Dockerfile Lab - ARG instruction "
description: " Dockerfile Lab 7 "
---

#### Build Dockerfile 

```sh
 Dockerfile git:(main) ✗ docker build -t sangam14/arg-dockerfile  -f dockerfile.arg .
[+] Building 3.5s (11/11) FINISHED                                                                                                                         
 => [internal] load build definition from dockerfile.arg                                                                                              0.0s
 => => transferring dockerfile: 336B                                                                                                                  0.0s
 => [internal] load .dockerignore                                                                                                                     0.0s
 => => transferring context: 2B                                                                                                                       0.0s
 => [internal] load metadata for docker.io/library/alpine:latest                                                                                      2.2s
 => [auth] library/alpine:pull token for registry-1.docker.io                                                                                         0.0s
 => CACHED [1/6] FROM docker.io/library/alpine@sha256:69665d02cb32192e52e07644d76bc6f25abeb5410edc1c7a81a10ba3f0efb90a                                0.0s
 => [2/6] RUN echo ENV is stronger than an ARG                                                                                                        0.2s
 => [3/6] RUN echo not going to matter                                                                                                                0.1s
 => [4/6] RUN echo ${key2}                                                                                                                            0.3s
 => [5/6] RUN echo defaultValue                                                                                                                       0.2s
 => [6/6] RUN echo ENV value takes over                                                                                                               0.3s
 => exporting to image                                                                                                                                0.0s
 => => exporting layers                                                                                                                               0.0s
 => => writing image sha256:acf55f3ef13e44ff24acf18f9c6320e5af33aa3eb9789274a46f47a9dff6d474                                                          0.0s
 => => naming to docker.io/sangam14/arg-dockerfile             

 ```

#### Inspect Env variable 

```sh
docker image inspect --format '{{json .Config}}' sangam14/arg-demo:1.0 | jq '.Env'

```

output 

```sh
docker image inspect --format '{{json .Config}}' sangam14/arg-dockerfile | jq '.Env'
[
  "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
  "key1=ENV is stronger than an ARG",
  "key2=ENV value takes over"
]
```
docker container run sangam14/arg-dockerfile env  

```sh
docker container run sangam14/arg-dockerfile env                                    
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=7b09d8fffd50
key1=ENV is stronger than an ARG
key2=ENV value takes over
HOME=/root

```
#### Pass env values while building dockerfile 

```sh
docker  build --rm --build-arg key1="buildTimeValue" --build-arg key2="good till env instruction" --tag sangam14/arg-dockerfile1 -f dockerfile.arg . 
Sending build context to Docker daemon  50.18kB
Step 1/11 : FROM alpine
latest: Pulling from library/alpine
af6eaf76a39c: Already exists 
Digest: sha256:69665d02cb32192e52e07644d76bc6f25abeb5410edc1c7a81a10ba3f0efb90a
Status: Downloaded newer image for alpine:latest
 ---> d74e625d9115
Step 2/11 : ENV key1="ENV is stronger than an ARG"
 ---> Running in 93620e2ca3e2
Removing intermediate container 93620e2ca3e2
 ---> e5eaad1dfbfa
Step 3/11 : RUN echo ${key1}
 ---> Running in bf14143efff7
ENV is stronger than an ARG
Removing intermediate container bf14143efff7
 ---> 2936757fac41
Step 4/11 : ARG key1="not going to matter"
 ---> Running in 161bf9d911e7
Removing intermediate container 161bf9d911e7
 ---> 87ff1f1e4c1b
Step 5/11 : RUN echo ${key1}
 ---> Running in 2e69ff7a2d91
ENV is stronger than an ARG
Removing intermediate container 2e69ff7a2d91
 ---> af66f1b039df
Step 6/11 : RUN echo ${key2}
 ---> Running in 5dd29054ba99

Removing intermediate container 5dd29054ba99
 ---> f575cd74046a
Step 7/11 : ARG key2="defaultValue"
 ---> Running in 757002f257f0
Removing intermediate container 757002f257f0
 ---> d60933ac5ef5
Step 8/11 : RUN echo ${key2}
 ---> Running in 742bdd38d1e0
good till env instruction
Removing intermediate container 742bdd38d1e0
 ---> b32166e66170
Step 9/11 : ENV key2="ENV value takes over"
 ---> Running in 1bedb017ed72
Removing intermediate container 1bedb017ed72
 ---> c8a4d9a2fd20
Step 10/11 : RUN echo ${key2}
 ---> Running in 96dbe6d53412
ENV value takes over
Removing intermediate container 96dbe6d53412
 ---> 6270d392443c
Step 11/11 : CMD ["sh"]
 ---> Running in b13a9ae5798f
Removing intermediate container b13a9ae5798f
 ---> 9f878f6fe503
Successfully built 9f878f6fe503
Successfully tagged sangam14/arg-dockerfile1:latest
```

#### Inspect Env of new docker image 

```sh
docker image inspect --format '{{json .Config}}' sangam14/arg-dockerfile1:latest | jq '.Env'
[
  "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
  "key1=ENV is stronger than an ARG",
  "key2=ENV value takes over"
]
```

```sh
docker container run sangam14/arg-dockerfile1 env
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=dff7e104aadb
key1=ENV is stronger than an ARG
key2=ENV value takes over
HOME=/root

```


cat dockerfile.arg1

```Dockerfile

FROM alpine

ENV lifecycle="production"
RUN echo ${lifecycle}
ARG username="35"
RUN echo ${username}
ARG appdir
RUN echo ${appdir}

ADD hello /${appdir}/
RUN chown -R ${username}:${username} ${appdir}
WORKDIR ${appdir}
USER ${username}

CMD ["./hello"]

```

#### build docker image and pass build args 

```sh
docker build --build-arg username=35 --build-arg appdir="/opt/hello" -t sangam14/arg1-dockerfile  -f dockerfile.arg1 .
```

