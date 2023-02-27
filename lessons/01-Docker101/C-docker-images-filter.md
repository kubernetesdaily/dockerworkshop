---
title: "Docker Image Filtering"
description: "Filter Docker Images"
---

running `docker run alpine ` command would download the Alpine Linux image from Docker Hub and start a new container based on that image. You can then use the container to run commands or applications. When you exit the container, it will stop running.

Alpine Linux is a lightweight Linux distribution that is commonly used in Docker containers due to its small size and security features.

```
dockerworkshop git:(main) ✗ docker pull alpine:3.6 
docker pull alpine:3.7
docker pull alpine:3.8
docker pull alpine:3.9
3.6: Pulling from library/alpine
e8f81692e76c: Pull complete 
Digest: sha256:66790a2b79e1ea3e1dabac43990c54aca5d1ddf268d9a5a0285e4167c8b24475
Status: Downloaded newer image for alpine:3.6
docker.io/library/alpine:3.6
3.7: Pulling from library/alpine
40223db5366f: Pull complete 
Digest: sha256:8421d9a84432575381bfabd248f1eb56f3aa21d9d7cd2511583c68c9b7511d10
Status: Downloaded newer image for alpine:3.7
docker.io/library/alpine:3.7
3.8: Pulling from library/alpine
788aef77d06b: Pull complete 
Digest: sha256:2bb501e6173d9d006e56de5bce2720eb06396803300fe1687b58a7ff32bf4c14
Status: Downloaded newer image for alpine:3.8
docker.io/library/alpine:3.8
3.9: Pulling from library/alpine
941f399634ec: Pull complete 
Digest: sha256:414e0518bb9228d35e4cd5165567fb91d26c6a214e9c95899e1e056fcd349011
Status: Downloaded newer image for alpine:3.9
docker.io/library/alpine:3.9

```

### docker images filtering 

The docker images command allows you to filter Docker images based on various criteria using the --filter option. Here are some common filters that you can use with the docker images command:


```
docker images --filter=reference='alpine'
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
alpine       3.9       9afdd4a290bf   2 years ago   5.3MB
alpine       3.8       b22edbe95d11   3 years ago   4.2MB
alpine       3.7       bd812700d284   3 years ago   4.01MB
alpine       3.6       25e814211fdd   3 years ago   3.84MB
```
Filters images that are or are not "dangling," meaning they are not tagged and not referenced by any container.
```
docker images --filter dangling=false 
```
Or to list images created before a specific image, you can run:
```
docker images --filter before=alpine:3.8 

```

