---
title: " Docker architecture and its components "
description: " Dockef Architecture in detail  "
---


Docker architecture and its components

# Docker Architecture 

![](./images/docker-engine-architecture.svg)



Docker architecture consists of three main components:

- Docker Engine: This is the core component of Docker and is responsible for building, running, and distributing Docker containers. It is made up of a daemon, a REST API, and a CLI (command line interface).
- Docker Hub: This is a cloud-based registry service that allows users to share and store Docker images. It acts as a central repository for Docker images and makes it easy for users to find and download images that they need for their projects.
- Docker Client: This is a command-line tool that allows users to interact with the Docker daemon and perform various tasks such as building, running, and distributing Docker containers.

In addition to these main components, Docker architecture also includes other components such as Docker Compose (a tool for defining and running multi-container Docker applications), Docker Swarm (a tool for orchestrating and managing a cluster of Docker containers), and Docker Machine (a tool for creating and managing Docker hosts on various infrastructure platforms).


![](./images/docker-architecture.png)

# Docker Engine 

To interact with the Docker Engine using Go, we can use the official Docker API library for Go, which can be found at https://github.com/docker/docker.

Here are the steps to interact with the Docker Engine using Go:

1.Import the necessary libraries:

```go
import (
    "context"
    "github.com/docker/docker/api/types"
    "github.com/docker/docker/client"
)

```
2.Create a new Docker client:

```go
cli, err := client.NewClientWithOpts(client.FromEnv, client.WithAPIVersionNegotiation())
if err != nil {
    log.Fatal(err)
}

```
3.Use the client to perform various actions on the Docker Engine, such as pulling an image from a registry:

```go
reader, err := cli.ImagePull(context.Background(), "alpine:latest", types.ImagePullOptions{})
if err != nil {
    log.Fatal(err)
}

io.Copy(os.Stdout, reader)


```

Close the client when finished:

```go
reader, err := cli.ImagePull(context.Background(), "alpine:latest", types.ImagePullOptions{})
if err != nil {
    log.Fatal(err)
}

io.Copy(os.Stdout, reader)


```


Using the Docker API library for Go, we can perform various actions on the Docker Engine, such as pulling images, creating and managing containers, and more.


# Docker Hub 
<br>
First, we need to import the necessary packages for interacting with Docker Hub. This includes the "net/http" package for making HTTP requests and the "encoding/json" package for parsing the response from Docker Hub.
<br>
Next, we need to define a function that makes an HTTP GET request to the Docker Hub API endpoint for retrieving a list of all available repositories. We can use the "http.NewRequest" function to create the request and the "http.DefaultClient.Do" function to execute the request.
<br>
We can then parse the response from Docker Hub using the "json.Unmarshal" function and store the list of repositories in a variable.

Finally, we can iterate through the list of repositories and print out the name and description of each repository.

Here is the complete Golang program:


```go
package main

import (
"encoding/json"
"fmt"
"net/http"
)

func main() {
// Make HTTP GET request to Docker Hub API endpoint for retrieving list of repositories
req, err := http.NewRequest("GET", "https://hub.docker.com/v2/repositories/", nil)
if err != nil {
fmt.Println(err)
return
}
// Execute request
resp, err := http.DefaultClient.Do(req)
if err != nil {
    fmt.Println(err)
    return
}

// Parse response from Docker Hub
var data map[string][]map[string]interface{}
err = json.NewDecoder(resp.Body).Decode(&data)
if err != nil {
    fmt.Println(err)
    return
}

// Iterate through list of repositories and print out name and description
for _, repo := range data["results"] {
    fmt.Println(repo["name"], repo["description"])
}
}


```
# Docker Client


To interact with the Docker client in Go, we will need to import the github.com/docker/docker/client package and create a new Docker client using the NewClient function.

Here are the steps to follow:

1. Install the Docker SDK for Go by running `go get github.com/docker/docker/client`.
2. Import the github.com/docker/docker/client package in your Go code.
3. Create a new Docker client using the client.NewClient function. This function takes a string argument that specifies the Docker daemon URL, and returns a client.Client object.
4. Use the methods provided by the `client.Client` object to interact with the Docker daemon. Some examples of common methods include `ListContainers`, `InspectContainer`, and `CreateContainer`.
For example, to list all running containers on the Docker daemon, you could use the following code:

```go

package main

import (
	"context"
	"fmt"
	"github.com/docker/docker/client"
)

func main() {
	// Create a new Docker client
	cli, err := client.NewClient("unix:///var/run/docker.sock", "v1.39", nil, nil)
	if err != nil {
		panic(err)
	}
	
	// List all running containers
	containers, err := cli.ListContainers(context.Background(), types.ContainerListOptions{All: true})
	if err != nil {
		panic(err)
	}
	
	// Print the names of the containers
	for _, container := range containers {
		fmt.Println(container.Names[0])
	}
}


```

This code creates a new Docker client using the Unix socket located at `/var/run/docker.sock`, and then lists all running containers using the `ListContainers` method. Finally, it prints the names of the containers to the console.



#  Docker Runtime 

- Containerd 

Containerd is a runtime for managing containers on a system. It is designed to be lightweight and efficient, making it a popular choice for use in container-based environments.
<br>
Here is an example of using Containerd to run a Docker container:

1.Install ContainerD on your system:

```bash
sudo apt-get install containerd

```
2. Start the Containerd daemon:

```bash
sudo systemctl start containerd

```
3. Pull a Docker image from the Docker Hub:

```bash
docker pull ubuntu

```
Run a Docker container using Containerd:

```bash
containerd run --name my-container ubuntu
```


This will start a new Ubuntu container with the name "my-container" using ContainerD as the runtime. You can then access the container and run commands inside it just like any other Docker container.


# shim 
<br>
Docker runtime is a term that refers to the environment in which Docker containers are executed. It includes the operating system, network, and storage resources required to run the containers.

The Docker runtime also includes a shim, which is a small utility program that acts as an intermediary between the container and the host operating system. The shim is responsible for starting and stopping the container, as well as handling any errors or issues that may occur during execution.

An example of how the shim works would be as follows:

1.The user creates a Docker container and specifies the desired runtime environment (e.g. Linux, Windows, etc.)
2.The container is built and stored in a Docker image, which includes all the necessary files and dependencies for the container to run.
3.When the user runs the container, the shim is activated and begins executing the container's code.
4.The shim manages the container's execution, including starting and stopping the container, handling errors, and allocating resources such as memory and CPU.
5. Once the container finishes executing, the shim shuts it down and releases any resources that were being used.

The Docker runtime and shim work together to ensure that containers are able to run smoothly and efficiently within the specified environment.

# runc 

runc is the default runtime for Docker containers. It is a command line tool for running and managing containers according to the Open Container Initiative (OCI) specification.

Here is an example of using runc to run a Docker container:

1. First, pull a Docker image from the registry using the 'docker pull' command:

```bash
docker pull alpine

```
2. Next, create a configuration file for the container using the 'runc spec' command:

```bash
runc spec

```

3. The configuration file can then be modified to specify the container's desired settings, such as the root filesystem, network settings, and runtime options.

4. To create and start the container, use the 'runc run' command, followed by the name of the container and the configuration file:

```bash
runc run my-container config.json

```
This will start the container based on the specified configuration and run the default command specified in the Docker image.

To stop the container, use the 'runc kill' command:

```bash
runc kill my-container

```
And to delete the container, use the 'runc delete' command:

```bash

runc delete my-container

```



