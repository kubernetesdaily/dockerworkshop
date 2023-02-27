---
title: "Interacting with container ecosystem "
description: "  "
---

# Interacting with container ecosystem
    
- Docker Images and Image Layers     

   - In Docker, an image is a lightweight, stand-alone, executable package that includes everything needed to run a piece of software, including the application code, libraries, dependencies, and runtime. It's used to build and run containerized applications.

   - A Docker image is made up of a series of layers, where each layer represents an   instruction in the image's Dockerfile. When you build an image, each instruction in the Dockerfile creates a new layer in the image. For example, if the Dockerfile has a COPY instruction to copy in some new files, that creates a new layer in the image with those files. If you then change a file and rebuild the image, only the layers that have changed need to be rebuilt, which makes building images faster and more efficient.
  
   - Each layer is a delta of the changes made in that instruction compared to the previous layer. When you run a container from an image, Docker creates a new top writable layer on top of the underlying layers, allowing you to make changes to the running container. These changes are not persisted when the container is stopped and removed, unless you commit the changes to a new image.

   - In summary, a Docker image is a lightweight, stand-alone, executable package that contains everything needed to run a piece of software, and it is made up of a series of layers representing instructions in the image's Dockerfile.

# Docker images commands 

```bash
docker images [OPTIONS] [REPOSITORY[:TAG]]

```

- The default docker images will show all top level images, their repository and tags, and their size.
<br>
- Docker images have intermediate layers that increase reusability, decrease disk usage, and speed up docker build by allowing each step to be cached. These intermediate layers are not shown by default.
<br>
- The SIZE is the cumulative space taken up by the image and all its parent images. This is also the disk space used by the contents of the Tar file created when you docker save an image.

-  An image will be listed more than once if it has multiple repository names or tags. This single image (identifiable by its matching IMAGE ID) uses up the SIZE listed only once.

- docker images - list all local Docker images

Example: `docker images`
<br>

-  ```docker image ls``` - list all local Docker images with additional information such as image ID, repository, and tag


Example: `docker image ls`


- ```docker image inspect``` - display detailed information about a Docker image

Example: `docker image inspect ubuntu:latest`
<br>

- ```docker image history``` - show the history of a Docker image
Example: ```docker image history ubuntu:latest```
<br>

- ```docker image pull``` - download a Docker image from a registry
Example: ```docker image pull ubuntu:latest```
<br>

- ```docker image push``` - push a Docker image to a registry
Example: ```docker image push my-image:latest```
<br>

- ```docker image tag``` - add a tag to a Docker image
Example: ```docker image tag ubuntu:latest my-image:latest```
<br>

- ```docker image build``` - build a Docker image from a Dockerfile
Example: ```docker image build -t my-image:latest .```
<br>

- ```docker image save``` - save a Docker image to a tar archive
Example: ```docker image save ubuntu:latest > ubuntu.tar```
<br>

-  ```docker image load` - load a Docker image from a tar archive
Example: ```docker image load < ubuntu.tar```


| Name, shorthand	 | Default | Description |
|-|-|-|
| --all , -a| 	| Show all images (default hides intermediate images) |
|--digests	|	| Show digests |
|--filter , -f | |		Filter output based on conditions provided |
|--format	| |	Pretty-print images using a Go template|
|--no-trunc	| |	Don't truncate output |
|--quiet , -q	| |	Only show image IDs|

<br>

# List recently created images 

```bash
$ docker images 
```
alternative 

```bash
$ docker image ls 
```

# Filtering Docker Image List 

```bash
$ docker images --filter "<key> = <value>"

```
With the `–filter` option, you can specify the following keys :

<br>

- ```reference``` : that can be used in order to isolate images having a certain name or tag;

```bash
$ docker images --filter "reference=deb*"
```
`before` : to filter images created “before” a specific point in time;

```bash
$ docker images --filter "before=<image_name>"
```

```since``` : to filter images since a specific point in time (usually another image creation);

```bash
  docker images --filter "since=<image_name>"
```

```label``` : if you used the LABEL instruction to create metadata for your image you can filter them later with this key

```bash
 $ docker images --filter "label=maintainer=<email>"
```

```dangling``` : in order to isolate images that are not used anymore.
   
```bash
docker images --filter "dangling=true"
```

### Here are some examples of using the 'docker images' command with the '--format' option:      

To list all Docker images and their IDs in a table format, use the following command:

```bash
docker images --format "table {{.ID}}\t{{.Repository}}"

```
output 

```bash
IMAGE ID REPOSITORY
4f4b6b93e0a4 alpine
```

To list all Docker images and their sizes in a custom format, use the following command:

```bash
docker images --format "{{.Size}}\t{{.Repository}}"
```
output 

```bash
SIZE REPOSITORY
3.99 MB alpine

```

To list all Docker images and their created dates in a custom format, use the following command:

```bash
docker images --format "{{.CreatedAt}}\t{{.Repository}}"

```

output 

```bash
CREATED AT REPOSITORY
2022-03-01T00:00:00Z alpine
```

To list all Docker images with their full repository and tag names, use the `--no-trunc` flag:

```bash
docker images --all --no-trunc

```

```docker images --quiet``` - This command lists all of the Docker images on the system, but only displays the image IDs.

```bash
docker images --quiet

```

output 

```bash
cdd72a7d23c8
```

```docker pull --quiet [image name]``` - This command pulls a Docker image from the registry, but only displays the image ID of the image being pulled.

```bash
docker pull --quiet alpine

```

output 

```bash
d9e555c53f53
```

```docker build --quiet -t [image name] [Dockerfile location]``` - This command builds a Docker image from a Dockerfile, but only displays the image ID of the image being built.

```bash
docker build --quiet -t my-image .
```
output

```bash
24c1f2a1c63d
```

# Understand Image Layers 
<br>
why layers ? 

to save on computational efforts when building images , and bandwidth (aka pulling and pushing ) them 

Build Container images 

```bash
FROM node:alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY ./ ./
CMD ["npm", "start"]

```

This Dockerfile contains instructions needed to build a basic Node.js app image on Docker. When you run a `docker build` command, Docker starts executing these instructions one at a time, iteratively.

![](./images/image-layer.png)

Each time a command is executed from the Dockerfile, a new image layer is created on top of the existing image. This process is iterated until Docker reads the last command of the Dockerfile. Each instruction created a new image.

![](./images/image-layer-execute.png)

  
# Composition of a Docker Image

typically , an image can constitute the following 

- Base Image 

  - all of the container iamges are created from the base image. this ia an emoty first layer that allows users to build their images form scratch 

  - if you do not want to create your base image from scratch, you can use an official Docker image like Centos as your base image or customize one of the official Docker images to your requirements.
<br>

-  Parent Image 

  - the parent images is most cases, the first Docker image layer. This is foundation of all other layers in your Docker. Thus, this layer provides the basic building blocks for container environments.
<br>

- Layers

   - The other Docker layers are added to the base image using a code to allow them to run in a container. Docker’s default status displays all the top-layer images, including file sizes, tags, and repositories. Caching of intermediate layers makes it easier to view the top layers. Storage drives in Docker manage the contents in image layers.

- Container layer

   - Besides creating a new container, a Docker image creates a container or writable layer. It hosts the changes that you make to your running container. This layer also stores the deleted and newly written files and changes made to the existing files. This layer is also useful when customizing containers.

- Docker manifest

The list of all image layers created by specific image names is known as a Docker manifest. These manifests are used the same way you would use an image name in the ```docker run``` and ```pull``` commands.

  
To build a Docker container image using a Dockerfile, follow these steps:

- Create a new directory and navigate to it in your terminal.
- Inside the directory, create a file named ```Dockerfile``` (without any file extension).
- Inside the Dockerfile, specify the base image you want to build from using the "FROM" directive. For example:

```bash
FROM alpine
```

- Add any additional instructions to the Dockerfile, such as installing packages, copying files, or setting environment variables.
- Save the Dockerfile.
- In your terminal, navigate to the directory containing the Dockerfile.
- Run the `docker build` command, followed by the desired name and tag for the image, and the path to the Dockerfile:

```docker build -t my-image:latest .```

The Docker engine will now build the image based on the instructions in the Dockerfile.
Once the build is complete, you can verify that the image was created by running the "docker images" command:

```bash
docker images
```

# Dockerfile instructions with example 

1.`FROM`: This instruction sets the base image for the Docker container. For example:

```FROM ubuntu:latest```

2.`RUN`: This instruction runs a command in the container. For example:

```RUN apt-get update```

3.`COPY`: This instruction copies files from the host machine to the container. For example:

```COPY app/ /app```

4.`ADD`: This instruction is similar to COPY, but it can also handle URL sources and automatically decompress compressed files. For example:

```ADD https://example.com/app.tar.gz /app```

5.`ENV`: This instruction sets environment variables in the container. For example:

```ENV LANG=en_US.UTF-8```

6.`WORKDIR`: This instruction sets the working directory for the container. For example:

```WORKDIR /app```

7.```EXPOSE```: This instruction exposes a port on the container to be accessed from the host machine. For example:

```EXPOSE 8080```

8.```CMD```: This instruction sets the default command to be run when the container is started. For example:

```CMD ["python", "app.py"]```

9.```ENTRYPOINT```: This instruction sets the default command to be run when the container is started, but it cannot be overridden by command-line arguments. For example:

```ENTRYPOINT ["python", "app.py"]```

 
# Lab 1 

 Begin by creating a new file called "Dockerfile" in your project directory.
 In the Dockerfile, we will first specify the base image that we will be using. For this example, we will be using the latest version of Ubuntu as our base image.

```bash
FROM ubuntu:latest
```

 Next, we will run the necessary commands to install Git on the base image.

```bash
RUN apt-get update
RUN apt-get install -y git

```
Once the installation is complete, we will set the default command to run when the container starts. In this case, we will set it to run the Git version command to confirm that Git is installed and functioning correctly.

```bash
CMD ["git", "--version"]
```

Save the Dockerfile and close it.
To build the Docker image, we will use the "docker build" command and specify the path to the Dockerfile as an argument.

```bash
docker build -t my_git_image .

```

This will start a new container based on the "my_git_image" image and run the default command specified in the Dockerfile. The output should show the version of Git installed in the image.

# Lab 2 

In the Dockerfile, specify the base image you want to use. For example:

```bash
FROM ubuntu:20.04

```
Add the ADD instruction to your Dockerfile to copy files or directories from your local system into the image. For example:

```bash
ADD my_file.txt /app/
ADD my_directory /app/

```

Use the RUN instruction to execute any necessary commands or installs for your image.

Use the CMD instruction to specify the command that will be run when a container is started from this image.

Build the Docker image using the following command:

```bash
docker build -t my_image_name .

```
Run the image using the following command:

```bash
docker run -d my_image_name
```

# Lab 2 

`Dockerfile`in your favorite text editor and add the following content to it:

```bash
FROM ubuntu:18.04
CMD ["echo", "Hello, World!"]
```

This `Dockerfile` specifies that we want to use the `ubuntu:18.04` base image and run the echo command with the arguments`"Hello, World!"` when the container is started.

```bash
docker build -t myimage .
docker run myimage
```

# Lab 3 

In the Dockerfile, specify the base image that your image will be built on. For this example, we will use the alpine base image, which is a lightweight version of Linux:

```bash
FROM alpine

```

Next, add the COPY instruction to copy a file or directory from your local machine into the image. For example, if you want to copy the file `hello.txt` from your current directory into the image, you would use the following instruction:

```bash 
COPY hello.txt /

```

ou can also specify a different destination for the file. For example, if you want to copy hello.txt into the /app directory inside the image, you would use the following instruction:

```bash
COPY hello.txt /app/

```

Now that you have specified the base image and the files to be copied, you can build your Docker image. Run the following command to build the image, replacing my-image with the desired name for your image:

```bash 

docker build -t my-image .

```

To verify that the image was built successfully and that the file was copied into the image, you can run the following command to view the list of available Docker images:

```bash
docker images

```

You should see your new image listed in the output. To run the image, use the docker run command, followed by the name of the image:

```bash
docker run my-image

```

# Lab 4

here is an example of creating a Python app with an Entrypoint instruction:

First, create a new directory for your app and navigate to it in the command line.
Create a file called `app.py` and add the following code:

```python
def main():
    print("Hello, world!")

if __name__ == "__main__":
    main()

```
Next, create a file called `Dockerfile` and add the following code:

```bash
FROM python:3.8-slim

COPY app.py /app/app.py

ENTRYPOINT ["python", "/app/app.py"]


```

Build the Docker image by running the following command:

```bash
docker build -t my-app .
```

Run the Docker image by using the following command:

```bash
docker run my-app

```

# Lab 5

```bash
FROM nginx:alpine
WORKDIR /var/www/html
COPY . .
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]

```

This Dockerfile will use the nginx:alpine image as a base, set the working directory to /var/www/html, copy all the files in the current directory (.) to the working directory, expose port 80, and start the nginx daemon.

Now let's create a sample HTML file called`index.html`:

```html
<html>
  <head>
    <title>Nginx Workdir Example</title>
  </head>
  <body>
    <h1>Welcome to the Nginx Workdir Example</h1>
  </body>
</html>


```
Now we can build and run the Docker container using the following commands:

```bash
docker build -t nginx-workdir .
docker run -p 8080:80 nginx-workdir


```
This will build the Docker image with the tag nginx-workdir and run the container, mapping port 8080 on the host machine to port 80 in the container.

To verify that everything is working, open your web browser and navigate to `http://localhost:8080`. You should see the content of the index.html file displayed.

That's it! You have successfully created a Docker container using the `WORKDIR` instruction to set the working directory for the nginx daemon.

# Lab 6

- Pull the Nginx image from Docker Hub:

```bash
docker pull nginx

```

- Create a directory on your local machine that you want to use as the volume for Nginx:

```bash
mkdir /path/to/volume
```

-  Run the Nginx container and mount the volume using the -v flag:

```bash
docker run -d -p 80:80 -v /path/to/volume:/usr/share/nginx/html:ro nginx
```
-  Test that the volume is working by creating a file in the volume directory on your local machine:

```echo "Hello, World!" > /path/to/volume/index.html```

5. Visit http://localhost in your web browser to see the contents of the volume displayed in the Nginx container.

Note: The -d flag runs the container in detached mode, the -p flag maps port 80 on the host to port 80 in the container, and the :ro flag makes the volume read-only inside the container.

## Storing data in a container

- Storing data in a container(mounts, volumes, etc.)<br>
- Networking in containers<br>

## Storing data in a docker container  

There are several options for storing data in a docker container:

- Use a volume: Docker volumes allow you to persist data outside of the container, so it can be shared between multiple containers or saved even if the container is stopped or removed. You can create a volume using the "docker volume create" command and then mount it to a specific directory in your container using the "-v" flag in the "docker run" command.

-  Create a volume using the docker volume create command:

```bash
$ docker volume create data-volume

```

- Run a docker container and mount the volume using the -v flag:

```bash
$ docker run -d --name my-container -v data-volume:/data ubuntu:latest

```

- Check if the volume is mounted to the container by using the docker inspect command:

```bash
$ docker inspect my-container | grep Mounts

```

You should see output similar to this:

```bash
"Mounts": [
            {
                "Type": "volume",
                "Name": "data-volume",
                "Source": "/var/lib/docker/volumes/data-volume/_data",
                "Destination": "/data",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ],

```

Now, any data that you write to the /data directory inside the container will be persisted in the volume.

For example, you can create a file in the /data directory:

```bash
$ docker exec -it my-container touch /data/test.txt


```

To verify that the data has been persisted, you can run another container and mount the same volume:

```bash
$ docker run -it --name my-container-2 -v data-volume:/data ubuntu:latest bash

```

Inside the new container, you should be able to see the test.txt file:

```bash
$ ls /data
test.txt

```
This demonstrates how you can use a volume to store data in a docker container. The data will persist even if the container is stopped or deleted, as it is stored in the volume, which is managed by Docker.

Use a bind mount:

A bind mount is a file or directory on the host machine that is mounted into a container. This allows you to access data on the host machine from within the container. You can create a bind mount using the `-v` flag in the "docker run" command, specifying the path to the file or directory on the host as well as the desired mount point in the container.

Create a directory on the host machine that you want to use as the bind mount. For example:

```bash
mkdir /data

```
Run the docker container, using the -v flag to specify the bind mount. The syntax is `-v host_directory:container_directory`. For example:

```bash 
docker run -d -p 8080:80 -v /data:/var/www/html my_web_server

```
This will mount the /data directory on the host machine to the /var/www/html directory in the container, which is typically the root directory for web server content.

- You can now add or modify files in the /data directory on the host machine and they will be automatically reflected in the container's /var/www/html directory.
- To verify that the bind mount is working, you can create a test file in the /data directory on the host machine:


```bash 
echo "This is a test file" > /data/test.txt
```
Then, open a shell in the container and check if the file exists in the `/var/www/html` directory:

```bash 
docker exec -it my_web_server /bin/bash
ls /var/www/html

```
You should see the test.txt file listed in the output.

Use a data container: A data container is a container specifically designed to store data that can be shared between multiple containers. You can create a data container using the `docker create` command, and then mount the data container's volume to a specific directory in other containers using the "-v" flag in the "docker run" command.

Create a new data container using the docker create command. For example:

```bash 
docker create -v /data --name data-container busybox /bin/true

```
This creates a data container with a volume at /data and a name of data-container.

Start the data container using the docker start command. For example:

```bash

docker run -it --volumes-from data-container --name app-container ubuntu bash

```
This creates a new container named app-container that will use the volume from the data container.

- In the app-container, you can now access the data stored in the data-container. For example:

```bash
root@7bd72f1f0f77:/# ls /data

```
This will show the contents of the /data volume in the data-container.

To stop the data container, use the docker stop command. For example:

```bash
docker stop data-container

```
This will stop the data container, but the data will still be available in the app-container.

To delete the data container, use the docker rm command. For example:

```bash
docker rm data-container

```

This will delete the data container, but the data will still be available in the app-container.

4. Use a host path: You can also store data directly on the host machine and access it from within the container using the "-v" flag in the "docker run" command, specifying the path on the host machine as the mount point in the container. However, this option is not recommended as it can lead to issues with container portability and separation of concerns.

To use a host path to store data in Docker, you will need to use the "-v" flag in the "docker run" command to specify the host path and the container path for the data volume. Here is an example using a MongoDB container:

First, create a directory on your host machine to store the data:

```bash
mkdir -p /data/mongodb
```

Run the MongoDB container, using the "-v" flag to mount the host path as a data volume in the container:

```bash
docker run -d --name mongodb -v /data/mongodb:/data/db mongo:latest
```
You can verify that the data volume is being used by the container by checking the output of the "docker inspect" command:

```bash
docker inspect mongodb | grep -i "Mounts"
```

This should return a list of all the mounted data volumes, including the host path that you specified in the "docker run" command.

Note: Make sure that the host path that you specify in the "-v" flag exists and is readable/writable by the container. Otherwise, the container may not start or may have issues accessing the data volume.

# Networking in containers

There are two types of networking in Docker:

- Bridge networking: This is the default networking mode in Docker. It creates a virtual network between containers on the same host. Each container is given its own IP address within this network, and containers can communicate with each other using this network.
  
First, create a Dockerfile that will build our container: 

```bash
  FROM alpine
  CMD ["echo", "Hello World!"]

```

Build the image using the following command:

```bash
docker build -t hello-world .
docker run -it hello-world
```

Inspect the container's networking configuration using the following command:

```bash
"NetworkSettings": {
    "Bridge": "",
    "SandboxID": "4f3c70f4f74b1dc5d5e5e3f5e5ed72c5e5e1f2613e1644b2aa5b004f2ab2d9e8",
    "HairpinMode": false,
    "LinkLocalIPv6Address": "",
    "LinkLocalIPv6PrefixLen": 0,
    "Ports": {},
    "SandboxKey": "/var/run/docker/netns/4f3c70f4f74b",
    "SecondaryIPAddresses": null,
    "SecondaryIPv6Addresses": null,
    "EndpointID": "f48c68e9ce012791d63f0b3ee3b3a85d6c7f6ed5b6c4b6e5b6d5b6c2e5e5f6d4",
    "Gateway": "172.17.0.1",
    "GlobalIPv6Address": "",
    "GlobalIPv6PrefixLen": 0,
    "IPAddress": "172.17.0.2",
    "IPPrefixLen": 16,
    "IPv6Gateway": "",
    "MacAddress": "02:42:ac:11:00:02",
    "Networks": {
        "bridge": {
            "IPAMConfig": null,
            "Links": null,
            "Aliases": null,
            "NetworkID": "5e5f5c5d5e5d5e5f5d5f5c5e5d5e5d5d5e5f5d5e5f5c5d5e5d5e5f5c5e5f5d5e",
            "EndpointID": "f48c68e9ce012791d63f0b3ee3b3a85d6c7f6ed5b6c4b6e5b6d5b6c2e5e5f6d4",
            "Gateway": "172.17.0.1",
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gate


```

- Overlay networking: This type of networking allows you to connect multiple Docker hosts into a single virtual network. This is useful for situations where you have multiple hosts running Docker containers, and you want them to be able to communicate with each other.

Overlay networking in Docker allows containers to communicate with each other across multiple hosts. This is useful for situations where you have multiple Docker hosts running and you want to create a network that spans across those hosts.

To demonstrate overlay networking, we will create two Docker hosts and a single network that will be shared between them. We will then create two containers, one on each host, and demonstrate that they can communicate with each other through the overlay network.

First, we will create two Docker hosts using Docker Machine. For this example, we will use two virtual machines running on VirtualBox.

```bash
docker-machine create --driver virtualbox host1
docker-machine create --driver virtualbox host2

```

Next, we will create an overlay network on one of the hosts. We will name this network "mynetwork" and specify the `--attachable` flag so that other containers can attach to it

```bash
eval $(docker-machine env host1)
docker network create --driver overlay --attachable mynetwork

```

Now that the network has been created, we can deploy containers on both hosts and attach them to the network.

On host1:

```bash

eval $(docker-machine env host1)
docker run -d --name container1 --network mynetwork busybox

```

On host2:

```bash
eval $(docker-machine env host2)
docker run -d --name container2 --network mynetwork busybox


```
Now that both containers are attached to the same overlay network, we can verify that they can communicate with each other.

On host1:

```bash
eval $(docker-machine env host1)
docker exec -it container1 ping -c 3 container2

```

On host2:

```bash
eval $(docker-machine env host2)
docker exec -it container2 ping -c 3 container1

```

If the ping commands are successful, it means that the containers are able to communicate with each other through the overlay network.

This is just a simple example of how overlay networking can be used in Docker. There are many other uses and configurations that can be explored.

# Docker Compose 

Docker Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application's services. Then, with a single command, you create and start all the services from your configuration.

To install Docker Compose, follow the instructions for your operating system:

```bash
# For macOS or Linux:
sudo curl -L "https://github.com/docker/compose/releases/download/1.28.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# For Windows:
Download the Docker Compose binary from https://github.com/docker/compose/releases/download/1.28.5/docker-compose-Windows-x86_64.exe and save it to a location on your PATH (e.g., C:\Program Files\Docker).


```
Once Docker Compose is installed, you can use it to create and start your multi-container Docker application by using the docker-compose up command. For example, suppose you have a YAML file called docker-compose.yml that defines your application's services like this:

```yaml
version: '3'
services:
  web:
    build: .
    ports:
      - "5000:5000"
    volumes:
      - .:/app
  redis:
    image: "redis:alpine"

```

You can start all of these services with the following command:

```bash
docker-compose up

```
This command will build the web service image, create and start the web and redis containers, and attach the terminal to the logs of the running containers.

# Docker Compose Build

```plaintext
docker-compose build
```

### Redeploy just one Service

```plaintext
docker-compose up $SERVICE_NAME
```

### start all services / container using docker compose

```plaintext
docker-compose up

// Specify a custom filepath for your 
//docker-compose file 
// (it assumes docker-compose.yml in
//your current directory by default)
ocker-compose -f custom-docker-compose.yml up

// Apply multiple compose files (changes in latter)

docker-compose -f docker-compose.yml docker-compose-production.yml
```

# sample docker-compose file

```

#docker-compose.yml file
version: '3'
services:
  # Your web application => Container
  web:
    build: .
    ports:
    - "5000:5000"

  # Redis cache container
  redis:
    image: "redis:alpine"


```

```

version: "3"

networks:
  backend:
    driver: bridge

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    container_name: app
    image: ${REGISTRY}/my-project-name
    restart: always
    ports:
      - "80:80"
      - "443:443"
    depends_on:
        - db
    networks:
      - backend
    env_file:
      - ./.env

  db:
    image: mariadb:10.5
    container_name: db
    restart: always
    networks:
        backend
    volumes:
      - mysql-data:/var/lib/mysql
    environment:
      - FOO=bar
      - SOME_ENV_VAR=${SUBSTITUTED_VARIABLE}
    env_file:
      - ./.env

volumes:
  mysql-data:
    driver: local


```

labels 

```
services:
  web:
    labels:
      com.app.description: "My web app"


```


[Docker CheatSheet](https://blog.cloudnativefolks.org/the-ultimate-docker-cheatsheet-for-everyone#heading-docker-compose)