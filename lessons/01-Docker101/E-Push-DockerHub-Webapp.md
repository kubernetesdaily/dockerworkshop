---
title: " Push first Nginx Web app To DockerHub "
description: "nginx"
---


#### Pull nginx image from dockerhub using 

```sh
dockerworkshop git:(main) ✗ docker pull nginx
Using default tag: latest
latest: Pulling from library/nginx
Digest: sha256:6650513efd1d27c1f8a5351cbd33edf85cc7e0d9d0fcb4ffb23d8fa89b601ba8
Status: Image is up to date for nginx:latest
docker.io/library/nginx:latest
```
#### Run Docker with ngnix 

```sh
 dockerworkshop git:(main) ✗ docker run --name docker-nginx -p 80:80 -d nginx
63258aebdc2d8ea40a0099efb3e51f8b15db2fe2dc048da3901843b4782d19fb
```
–name docker-nginx : Name given to the container that is run is docker-nginx-p 80:80 : the port we are exposing and mapping from local machine port number to that of container, in the format local_machine_port:container_port-d : Detached mode – Runs the container in background

#### check all running docker containers 

```sh
➜  dockerworkshop git:(main) ✗ docker ps
CONTAINER ID   IMAGE                       COMMAND                  CREATED          STATUS          PORTS                NAMES
63258aebdc2d   nginx                       "/docker-entrypoint.…"   55 seconds ago   Up 55 seconds   0.0.0.0:80->80/tcp   docker-nginx
```

#### open localhost with specific port 

![](./images/ngnix.png)

#### Include a static Web Application in the Docker with NGINX

To include our static Web Application into the Docker Image with NGINX, we shall create a Dockerfile (including commands to build image) and an html file with name index.html (acting as our web application) in a directory named nginx-app.

create dockerfile with following content :

```dockerfile

FROM nginx
COPY . /usr/share/nginx/html

```


```html

<html>
  <head>
    <title>Docker NGINX Tutorial</title>
  </head>
  <body>
    <h1>Join CloudNativeFolks Community</h1>
    <p>Learn to Dockerize with NGINX and your web application.</p>
    <a href=" https://discord.com/invite/9ERSnT7 ">Join Discord </a>
    <a href=" ">NGINX Tutorial</a>
  </body>
</html>

```

#### Build Dockerfile 

```sh
Dockerfile git:(main) ✗ docker build -t nginx-application -f dockerfile.ngnix .
[+] Building 0.1s (7/7) FINISHED                                                                                                                                                                  
 => [internal] load build definition from dockerfile.ngnix                                                                                                                                   0.0s
 => => transferring dockerfile: 87B                                                                                                                                                          0.0s
 => [internal] load .dockerignore                                                                                                                                                            0.0s
 => => transferring context: 2B                                                                                                                                                              0.0s
 => [internal] load metadata for docker.io/library/nginx:latest                                                                                                                              0.0s
 => [internal] load build context                                                                                                                                                            0.0s
 => => transferring context: 82B                                                                                                                                                             0.0s
 => [1/2] FROM docker.io/library/nginx                                                                                                                                                       0.0s
 => [2/2] COPY . /usr/share/nginx/html                                                                                                                                                       0.0s
 => exporting to image                                                                                                                                                                       0.0s
 => => exporting layers                                                                                                                                                                      0.0s
 => => writing image sha256:54027a144afd33ddd1449b757581c7b554d5411c4b2bac291f5dfbccb85fda41                                                                                                 0.0s
 => => naming to docker.io/library/nginx-application                                                                                                                                         0.0s
➜  Dockerfile git:(main) ✗ 
```


#### run updated ngnix webapp 

```sh
docker run --name docker-nginx-app  -p 80:80 -d nginx-application

```

#### List docker images 

```sh
Dockerfile git:(main) ✗ docker images
REPOSITORY                                                TAG                                                                          IMAGE ID       CREATED         SIZE
nginx-application                                         latest                                                                       e8742ef897ea   2 minutes ago   135MB
```

#### Tag Docker Images 

```sh
docker tag nginx-application  sangam14/nginx-application 

```

#### Login into Your DockerHub Account 

```sh
docker login 
docker push sangam14/nginx-application  
Using default tag: latest
The push refers to repository [docker.io/sangam14/nginx-application]
4e9e8987d0ed: Pushed 
7a99131e1da4: Mounted from library/nginx 
c61a83b92ad9: Mounted from library/nginx 
0d96feb871c8: Mounted from library/nginx 
902b28ccafe7: Mounted from library/nginx 
3063fc92629d: Mounted from library/nginx 
a49c6ceb5b3a: Mounted from library/nginx 
latest: digest: sha256:09f29db6e4179bd1019a48d2d50944989347fdf145193f4165353d5148a902c8 size: 1777

```