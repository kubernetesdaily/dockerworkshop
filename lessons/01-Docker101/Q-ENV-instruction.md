---
title: " Dockerfile Lab - ENV instruction "
description: " Dockerfile Lab 11 "
---


cat dockerfile 

```dockerfile
FROM alpine
ENV appDescription This app is a sample of using ENV instructions
ENV appName=env-demo
ENV note1="The First Note First" note2=The\ Second\ Note\ Second \
	note3="The Third Note Third"
ENV changeMe="Old Value"
CMD ["sh"]


```

#### build dockerfile 

```sh

➜  Dockerfile git:(main) ✗ docker build -t sangam14/env-dockerfile  -f dockerfile.env .
[+] Building 0.1s (5/5) FINISHED                                                                                                                  
 => [internal] load build definition from dockerfile.env                                                                                     0.0s
 => => transferring dockerfile: 279B                                                                                                         0.0s
 => [internal] load .dockerignore                                                                                                            0.0s
 => => transferring context: 2B                                                                                                              0.0s
 => [internal] load metadata for docker.io/library/alpine:latest                                                                             0.0s
 => CACHED [1/1] FROM docker.io/library/alpine                                                                                               0.0s
 => exporting to image                                                                                                                       0.0s
 => => exporting layers                                                                                                                      0.0s
 => => writing image sha256:6f9741e9b952495e61c515945c4acd85d85aef1e631b7d37c348c26e9f5d216d                                                 0.0s
 => => naming to docker.io/sangam14/env-dockerfile                                                                                           0.0s

```

### inspect Env Config 

```sh
docker image inspect --format '{{json .Config}}' sangam14/env-dockerfile  | jq '.Env'
[
  "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
  "appDescription=This app is a sample of using ENV instructions",
  "appName=env-demo",
  "note1=The First Note First",
  "note2=The Second Note Second",
  "note3=The Third Note Third",
  "changeMe=Old Value"
]

```
#### change envirmonment variable 

```sh

docker  run --rm --env changeMe="New Value" --env adhoc="run time"   sangam14/env-dockerfile   env 
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
HOSTNAME=12804862fac4
changeMe=New Value
adhoc=run time
appDescription=This app is a sample of using ENV instructions
appName=env-demo
note1=The First Note First
note2=The Second Note Second
note3=The Third Note Third
HOME=/root

```