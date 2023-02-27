
Orignally published on [CloudNativeFolks]()

`ThreatMapper`

ThreatMapper hunts for threats in your production platforms, and ranks these threats based on their risk-of-exploit. It uncovers vulnerable software components, exposed secrets and deviations from good security practice. ThreatMapper uses a combination of agent-based inspection and agent-less monitoring to provide the widest possible coverage to detect threats.

[![Readme Card](https://github-readme-stats.vercel.app/api/pin/?username=deepfence&repo=Threatmapper)](https://github.com/deepfence/Threatmapper)

ThreatMapper carries on the good 'shift left' security practices that you already employ in your development pipelines. It continues to monitor running applications against emerging software vulnerabilities and monitors the host and cloud configuration against industry-expert benchmarks.

The ThreatMapper Management Console is a container-based application that can be deployed on a single docker host or in a Kubernetes cluster.

# What exactly this management console useful for you?

*   Topology-based learning of your live infrastructure
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671720063844/e189658b-8bdf-4ede-a19f-ce04cf8b089e.png)
    

*   Identify Threats and Vulnerabilities and generate SBOM (software bill of material )
    

*   ThreatGraph visualization, you can then identify the issues that present the greatest risk to the security of your applications, and prioritize these for planned protection or remediation.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671720087970/619f0c7e-d251-4934-b165-6157cb3f3730.png )
    

*   Manage the users who can access the console.
    
*   Configure Infrastructure API access and interrogate platform configurations.
    
*   Visualize and drill down into Kubernetes clusters, virtual machines, containers and images, running processes, and network connections in near real-time.
    
*   Invoke vulnerability scans on running containers and applications and review the results, ranked by the risk of exploit.
    
*   Invoke compliance scans on infrastructure configuration ('agentless') and on infrastructure hosts ('agent-based), - manually or automatically when they are added to a cluster.
    
*   Scan container registries for vulnerabilities, to review workloads before they are deployed.
    
*   Scan image builds during the CI/CD pipeline, supporting CircleCI, Jenkins, and GitLab.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671720117091/e979935e-efa6-41b6-8a9d-75c08eb78771.png)
    
*   Scan containers and host filesystems for unprotected secrets, including access tokens, keys and passwords.
    
*   Configure integrations with external notification, SIEM and ticketing systems, including Slack, PagerDuty, Jira, Splunk, ELK, Sumo Logic, and AWS S3. ThreatMapper supports multiple production deployments simultaneously so that you can visualize and scan workloads across a large production estate.
    

in a short one-stop solution for your cloud-native security issues! and it's 100% open source

login to AWS account:- console.aws.amazon.com

## Deploy ThreatMapper Console using Docker Compose on Linux VM using AWS EC2

*   Create Ubuntu Instance on AWS EC2
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671720154113/3f081a20-b6ce-4813-8673-17d3c9abe78e.png)
    

select instance type as per requirements

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671720172313/ad01eb1e-b960-42e2-a317-dcefc1df36d5.png )

Click on the launch instance

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671720215112/24a709fc-590c-4070-9e71-01dc27bb61b4.png )

hurry!

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671720236656/29ee7c7d-20b6-4a3d-a16e-4b85e169d8a8.png )

access AWS instance using CLI

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671720271742/4b073e41-046e-4a25-9fba-bcbc446f397b.png )

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671720285296/ac528a2e-54df-4ef9-98b8-2d2d0d292c22.png )

```rust
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
ca-certificates is already the newest version (20211016).
ca-certificates set to manually installed.
software-properties-common is already the newest version (0.99.22.3).
software-properties-common set to manually installed.
The following additional packages will be installed:
  libcurl4
The following NEW packages will be installed:
  apt-transport-https
The following packages will be upgraded:
  curl libcurl4
2 upgraded, 1 newly installed, 0 to remove and 76 not upgraded.
Need to get 485 kB of archives.
After this operation, 169 kB of additional disk space will be used.
Get:1 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy-updates/universe amd64 apt-transport-https all 2.4.8 [1506 B]
Get:2 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy-updates/main amd64 curl amd64 7.81.0-1ubuntu1.6 [194 kB]
Get:3 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libcurl4 amd64 7.81.0-1ubuntu1.6 [290 kB]
Fetched 485 kB in 0s (11.0 MB/s)  
Selecting previously unselected package apt-transport-https.
(Reading database ... 63926 files and directories currently installed.)
Preparing to unpack .../apt-transport-https_2.4.8_all.deb ...
Unpacking apt-transport-https (2.4.8) ...
Preparing to unpack .../curl_7.81.0-1ubuntu1.6_amd64.deb ...
Unpacking curl (7.81.0-1ubuntu1.6) over (7.81.0-1ubuntu1.4) ...
Preparing to unpack .../libcurl4_7.81.0-1ubuntu1.6_amd64.deb ...
Unpacking libcurl4:amd64 (7.81.0-1ubuntu1.6) over (7.81.0-1ubuntu1.4) ...
Setting up apt-transport-https (2.4.8) ...
Setting up libcurl4:amd64 (7.81.0-1ubuntu1.6) ...
Setting up curl (7.81.0-1ubuntu1.6) ...
Processing triggers for man-db (2.10.2-1) ...
Processing triggers for libc-bin (2.35-0ubuntu3.1) ...
Scanning processes...                                                                                                                       
Scanning linux images...                                                                                                                    

Running kernel seems to be up-to-date.
```

# Install docker

```rust
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable"
apt-cache policy docker-ce
sudo apt install -y docker-ce
Warning: apt-key is deprecated. Manage keyring files in trusted.gpg.d instead (see apt-key(8)).
OK
Repository: 'deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable'
Description:
Archive for codename: focal components: stable
More info: https://download.docker.com/linux/ubuntu
Adding repository.
Press [ENTER] to continue or Ctrl-c to cancel.
Found existing deb entry in /etc/apt/sources.list.d/archive_uri-https_download_docker_com_linux_ubuntu-jammy.list
Adding deb entry to /etc/apt/sources.list.d/archive_uri-https_download_docker_com_linux_ubuntu-jammy.list
Found existing deb-src entry in /etc/apt/sources.list.d/archive_uri-https_download_docker_com_linux_ubuntu-jammy.list
Adding disabled deb-src entry to /etc/apt/sources.list.d/archive_uri-https_download_docker_com_linux_ubuntu-jammy.list
Hit:1 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy InRelease
Hit:2 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy-updates InRelease                            
Hit:3 http://us-east-1.ec2.archive.ubuntu.com/ubuntu jammy-backports InRelease                          
Hit:4 https://download.docker.com/linux/ubuntu focal InRelease                                          
Hit:5 http://security.ubuntu.com/ubuntu jammy-security InRelease                                        
Reading package lists... Done
W: https://download.docker.com/linux/ubuntu/dists/focal/InRelease: Key is stored in legacy trusted.gpg keyring (/etc/apt/trusted.gpg), see the DEPRECATION section in apt-key(8) for details.
docker-ce:
  Installed: 5:20.10.21~3-0~ubuntu-focal
  Candidate: 5:20.10.21~3-0~ubuntu-focal
  Version table:
 *** 5:20.10.21~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
        100 /var/lib/dpkg/status
     5:20.10.20~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     5:20.10.19~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     5:20.10.18~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     5:20.10.17~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     5:20.10.16~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     5:20.10.15~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     5:20.10.14~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     5:20.10.13~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     5:20.10.12~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     5:20.10.11~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     5:20.10.10~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     5:20.10.9~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     5:20.10.8~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     5:20.10.7~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     5:20.10.6~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     5:20.10.5~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     5:20.10.4~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     5:20.10.3~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     5:20.10.2~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     5:20.10.1~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     5:20.10.0~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     5:19.03.15~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     5:19.03.14~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     5:19.03.13~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     5:19.03.12~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     5:19.03.11~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     5:19.03.10~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
     5:19.03.9~3-0~ubuntu-focal 500
        500 https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
```

# verify whether docker is installed or not

```rust
docker 

Usage:  docker [OPTIONS] COMMAND

A self-sufficient runtime for containers

Options:
      --config string      Location of client config files (default "/home/ubuntu/.docker")
  -c, --context string     Name of the context to use to connect to the daemon (overrides DOCKER_HOST env var and default context set
                           with "docker context use")
  -D, --debug              Enable debug mode
  -H, --host list          Daemon socket(s) to connect to
  -l, --log-level string   Set the logging level ("debug"|"info"|"warn"|"error"|"fatal") (default "info")
      --tls                Use TLS; implied by --tlsverify
      --tlscacert string   Trust certs signed only by this CA (default "/home/ubuntu/.docker/ca.pem")
      --tlscert string     Path to TLS certificate file (default "/home/ubuntu/.docker/cert.pem")
      --tlskey string      Path to TLS key file (default "/home/ubuntu/.docker/key.pem")
      --tlsverify          Use TLS and verify the remote
  -v, --version            Print version information and quit

Management Commands:
  app*        Docker App (Docker Inc., v0.9.1-beta3)
  builder     Manage builds
  buildx*     Docker Buildx (Docker Inc., v0.9.1-docker)
  config      Manage Docker configs
  container   Manage containers
  context     Manage contexts
  image       Manage images
  manifest    Manage Docker image manifests and manifest lists
  network     Manage networks
  node        Manage Swarm nodes
  plugin      Manage plugins
  scan*       Docker Scan (Docker Inc., v0.21.0)
  secret      Manage Docker secrets
  service     Manage services
  stack       Manage Docker stacks
  swarm       Manage Swarm
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes

Commands:
  attach      Attach local standard input, output, and error streams to a running container
  build       Build an image from a Dockerfile
  commit      Create a new image from a container's changes
  cp          Copy files/folders between a container and the local filesystem
  create      Create a new container
  diff        Inspect changes to files or directories on a container's filesystem
  events      Get real time events from the server
  exec        Run a command in a running container
  export      Export a container's filesystem as a tar archive
  history     Show the history of an image
  images      List images
  import      Import the contents from a tarball to create a filesystem image
  info        Display system-wide information
  inspect     Return low-level information on Docker objects
  kill        Kill one or more running containers
  load        Load an image from a tar archive or STDIN
  login       Log in to a Docker registry
  logout      Log out from a Docker registry
  logs        Fetch the logs of a container
  pause       Pause all processes within one or more containers
  port        List port mappings or a specific mapping for the container
  ps          List containers
  pull        Pull an image or a repository from a registry
  push        Push an image or a repository to a registry
  rename      Rename a container
  restart     Restart one or more containers
  rm          Remove one or more containers
  rmi         Remove one or more images
  run         Run a command in a new container
  save        Save one or more images to a tar archive (streamed to STDOUT by default)
  search      Search the Docker Hub for images
  start       Start one or more stopped containers
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop one or more running containers
  tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
  top         Display the running processes of a container
  unpause     Unpause all processes within one or more containers
  update      Update configuration of one or more containers
  version     Show the Docker version information
  wait        Block until one or more containers stop, then print their exit codes

Run 'docker COMMAND --help' for more information on a command.
```

# install docker-compose

```rust
sudo sysctl -w vm.max_map_count=262144 
sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-linux-x86_64 -o /usr/local/bin/docker-compose
sudo mv /usr/local/bin/docker-compose /usr/bin/docker-compose
sudo chmod +x /usr/bin/docker-compose
docker-compose --version
vm.max_map_count = 262144
```

# docker-compose up

```rust
 sudo docker-compose -f docker-compose.yml up --detach
```

# pulling all docker images required for the management console

```rust
[+] Running 132/133al-router Pulled                                                                                                    5.1s
 ⠧ deepfence-ui Pulling                                                                                                               55.8s
   ⠿ ee30d1e46960 Pull complete                                                                                                       18.1s
   ⠿ cbdf55152882 Pull complete                                                                                                       20.7s
   ⠿ 02145a3d08dc Pull complete                                                                                                       24.4s
   ⠿ 3b43024e0402 Pull complete                                                                                                       25.5s
   ⠿ 000a421a9381 Pull complete                                                                                                       25.8s
   ⠿ 273e2d14a316 Pull complete                                                                                                       26.1s
   ⠿ 35f2174af386 Pull complete                                                                                                       52.7s
   ⠿ 3bf6d37d02af Pull complete                                                                                                       53.2s
   ⠿ 2ebf2edf21e9 Pull complete                                                                                                       53.7s
   ⠿ 70ecf58c4679 Pull complete                                                                                                       54.1s
 ⠿ deepfence-package-scanner Pulled                                                                                                    8.8s
   ⠿ 51dd59142225 Pull complete                                                                                                        4.0s
   ⠿ afd427f95343 Pull complete                                                                                                        5.2s
   ⠿ ad4b784c8601 Pull complete                                                                                                        7.5s
 ⠿ deepfence-backend Pulled                                                                                                           48.2s
 ⠿ deepfence-console-agent Pulled                                                                                                     46.4s
   ⠿ e0c9ebf1f052 Pull complete                                                                                                       25.5s
   ⠿ 9341ad80650d Pull complete                                                                                                       26.0s
   ⠿ b6a94e577185 Pull complete                                                                                                       26.9s
   ⠿ f2f4f6c497c7 Pull complete                                                                                                       27.4s
   ⠿ 83dc6dff4d1f Pull complete                                                                                                       27.8s
   ⠿ 2c66b377cc17 Pull complete                                                                                                       28.1s
   ⠿ ae95bb3e76fc Pull complete                                                                                                       29.9s
   ⠿ 58edf371ef84 Pull complete                                                                                                       30.5s
   ⠿ b5b91601778a Pull complete                                                                                                       30.9s
   ⠿ e7ca0b813b35 Pull complete                                                                                                       31.4s
   ⠿ bac5f99ed5ff Pull complete                                                                                                       32.2s
   ⠿ 0b03b6c6e703 Pull complete                                                                                                       32.7s
   ⠿ 00ef62c6beae Pull complete                                                                                                       33.1s
   ⠿ 2317941b432c Pull complete                                                                                                       33.4s
   ⠿ c78d4ef87429 Pull complete                                                                                                       33.8s
   ⠿ 52d6b999791e Pull complete                                                                                                       37.0s
   ⠿ 3c66ae264813 Pull complete                                                                                                       40.4s
   ⠿ fa91d8140bb4 Pull complete                                                                                                       44.0s
   ⠿ 415ec3af8b5c Pull complete                                                                                                       45.0s
 ⠿ deepfence-diagnosis Pulled                                                                                                          9.3s
   ⠿ d8089fb41a4e Pull complete                                                                                                        7.2s
   ⠿ 761247dc597b Pull complete                                                                                                        7.9s
 ⠿ deepfence-vulnerability-mapper Pulled                                                                                               9.0s
   ⠿ 9621f1afde84 Pull complete                                                                                                        4.8s
   ⠿ d2f87f45af97 Pull complete                                                                                                        5.8s
   ⠿ 0df38fc1dcc4 Pull complete                                                                                                        5.8s
   ⠿ 5ce3bac9b38d Pull complete                                                                                                        6.0s
   ⠿ b8a17e0751b8 Pull complete                                                                                                        7.7s
 ⠿ deepfence-redis Pulled                                                                                                             20.4s
   ⠿ 192e03523482 Pull complete                                                                                                       14.1s
   ⠿ 7151bccd2756 Pull complete                                                                                                       14.9s
   ⠿ e599fac432b2 Pull complete                                                                                                       16.0s
   ⠿ 720d86c10923 Pull complete                                                                                                       16.5s
   ⠿ 40911e48517b Pull complete                                                                                                       16.9s
   ⠿ 115eb662e680 Pull complete                                                                                                       17.3s
   ⠿ 31b9b283aa20 Pull complete                                                                                                       17.9s
   ⠿ b748b52207f7 Pull complete                                                                                                       18.3s
   ⠿ 5b0f449535b8 Pull complete                                                                                                       18.9s
 ⠿ deepfence-api Pulled                                                                                                               48.3s
   ⠿ 31b3f1ad4ce1 Pull complete                                                                                                        1.8s
   ⠿ f335cc1597f2 Pull complete                                                                                                        2.0s
   ⠿ 0375df124bb5 Pull complete                                                                                                        2.6s
   ⠿ 90a356bcda5b Pull complete                                                                                                        2.7s
   ⠿ c82e0170c13b Pull complete                                                                                                        2.9s
   ⠿ 38dc58c5f029 Pull complete                                                                                                        3.0s
   ⠿ 0189a7ca7a09 Pull complete                                                                                                       40.8s
   ⠿ 8d069dc99fe4 Pull complete                                                                                                       45.2s
   ⠿ 50e62824984f Pull complete                                                                                                       45.6s
   ⠿ c12613b81b5c Pull complete                                                                                                       45.8s
   ⠿ c8901960f9b5 Pull complete                                                                                                       45.9s
   ⠿ a5981c5eb3db Pull complete                                                                                                       46.3s
   ⠿ 860d4f2e0b1a Pull complete                                                                                                       46.8s
   ⠿ 8a3f7b35b548 Pull complete                                                                                                       47.0s
   ⠿ 37c02a7c1188 Pull complete                                                                                                       47.4s
 ⠿ deepfence-celery Pulled                                                                                                            48.2s
 ⠿ deepfence-postgres Pulled                                                                                                          28.2s
   ⠿ 7902437d3a12 Pull complete                                                                                                       10.9s
   ⠿ 709e2267bc98 Pull complete                                                                                                       11.5s
   ⠿ 10c5a0a9c34e Pull complete                                                                                                       21.4s
   ⠿ b46af7f38693 Pull complete                                                                                                       22.4s
   ⠿ 65aa0c237f80 Pull complete                                                                                                       23.3s
   ⠿ f6493ce74812 Pull complete                                                                                                       24.0s
   ⠿ eaac3b44f9d0 Pull complete                                                                                                       24.5s
   ⠿ 3b6db84bcdcc Pull complete                                                                                                       25.1s
   ⠿ 120ba5542ffd Pull complete                                                                                                       25.5s
   ⠿ 9fd0e93acee4 Pull complete                                                                                                       25.8s
   ⠿ 257465d6d91b Pull complete                                                                                                       26.1s
   ⠿ 568fad078a9c Pull complete                                                                                                       26.5s
 ⠿ deepfence-init-container Pulled                                                                                                    14.8s
   ⠿ 2ad0d7925a78 Pull complete                                                                                                       12.6s
   ⠿ 3e00d8e06113 Pull complete                                                                                                       13.2s
 ⠿ deepfence-es Pulled                                                                                                                41.1s
   ⠿ 4e9f2cdf4387 Pull complete                                                                                                       15.3s
   ⠿ 2e70516637d4 Pull complete                                                                                                       37.1s
   ⠿ ba468cc9ce22 Pull complete                                                                                                       37.6s
   ⠿ a2c864bf08ba Pull complete                                                                                                       38.1s
   ⠿ 7869dc55514f Pull complete                                                                                                       38.8s
   ⠿ 6320c0eaee7c Pull complete                                                                                                       39.5s
[+] Running 133/133al-router Pulled                                                                                                    5.1s
 ⠿ deepfence-ui Pulled                                                                                                                55.9s
   ⠿ ee30d1e46960 Pull complete                                                                                                       18.1s
   ⠿ cbdf55152882 Pull complete                                                                                                       20.7s
   ⠿ 02145a3d08dc Pull complete                                                                                                       24.4s
   ⠿ 3b43024e0402 Pull complete                                                                                                       25.5s
   ⠿ 000a421a9381 Pull complete                                                                                                       25.8s
   ⠿ 273e2d14a316 Pull complete                                                                                                       26.1s
   ⠿ 35f2174af386 Pull complete                                                                                                       52.7s
   ⠿ 3bf6d37d02af Pull complete                                                                                                       53.2s
   ⠿ 2ebf2edf21e9 Pull complete                                                                                                       53.7s
   ⠿ 70ecf58c4679 Pull complete                                                                                                       54.1s
 ⠿ deepfence-package-scanner Pulled                                                                                                    8.8s
   ⠿ 51dd59142225 Pull complete                                                                                                        4.0s
   ⠿ afd427f95343 Pull complete                                                                                                        5.2s
   ⠿ ad4b784c8601 Pull complete                                                                                                        7.5s
 ⠿ deepfence-backend Pulled                                                                                                           48.2s
 ⠿ deepfence-console-agent Pulled                                                                                                     46.4s
   ⠿ e0c9ebf1f052 Pull complete                                                                                                       25.5s
   ⠿ 9341ad80650d Pull complete                                                                                                       26.0s
   ⠿ b6a94e577185 Pull complete                                                                                                       26.9s
   ⠿ f2f4f6c497c7 Pull complete                                                                                                       27.4s
   ⠿ 83dc6dff4d1f Pull complete                                                                                                       27.8s
   ⠿ 2c66b377cc17 Pull complete                                                                                                       28.1s
   ⠿ ae95bb3e76fc Pull complete                                                                                                       29.9s
   ⠿ 58edf371ef84 Pull complete                                                                                                       30.5s
   ⠿ b5b91601778a Pull complete                                                                                                       30.9s
   ⠿ e7ca0b813b35 Pull complete                                                                                                       31.4s
   ⠿ bac5f99ed5ff Pull complete                                                                                                       32.2s
   ⠿ 0b03b6c6e703 Pull complete                                                                                                       32.7s
   ⠿ 00ef62c6beae Pull complete                                                                                                       33.1s
   ⠿ 2317941b432c Pull complete                                                                                                       33.4s
   ⠿ c78d4ef87429 Pull complete                                                                                                       33.8s
   ⠿ 52d6b999791e Pull complete                                                                                                       37.0s
   ⠿ 3c66ae264813 Pull complete                                                                                                       40.4s
   ⠿ fa91d8140bb4 Pull complete                                                                                                       44.0s
   ⠿ 415ec3af8b5c Pull complete                                                                                                       45.0s
 ⠿ deepfence-diagnosis Pulled                                                                                                          9.3s
   ⠿ d8089fb41a4e Pull complete                                                                                                        7.2s
   ⠿ 761247dc597b Pull complete                                                                                                        7.9s
 ⠿ deepfence-vulnerability-mapper Pulled                                                                                               9.0s
   ⠿ 9621f1afde84 Pull complete                                                                                                        4.8s
   ⠿ d2f87f45af97 Pull complete                                                                                                        5.8s
   ⠿ 0df38fc1dcc4 Pull complete                                                                                                        5.8s
   ⠿ 5ce3bac9b38d Pull complete                                                                                                        6.0s
   ⠿ b8a17e0751b8 Pull complete                                                                                                        7.7s
 ⠿ deepfence-redis Pulled                                                                                                             20.4s
   ⠿ 192e03523482 Pull complete                                                                                                       14.1s
   ⠿ 7151bccd2756 Pull complete                                                                                                       14.9s
   ⠿ e599fac432b2 Pull complete                                                                                                       16.0s
   ⠿ 720d86c10923 Pull complete                                                                                                       16.5s
   ⠿ 40911e48517b Pull complete                                                                                                       16.9s
   ⠿ 115eb662e680 Pull complete                                                                                                       17.3s
   ⠿ 31b9b283aa20 Pull complete                                                                                                       17.9s
   ⠿ b748b52207f7 Pull complete                                                                                                       18.3s
   ⠿ 5b0f449535b8 Pull complete                                                                                                       18.9s
 ⠿ deepfence-api Pulled                                                                                                               48.3s
   ⠿ 31b3f1ad4ce1 Pull complete                                                                                                        1.8s
   ⠿ f335cc1597f2 Pull complete                                                                                                        2.0s
   ⠿ 0375df124bb5 Pull complete                                                                                                        2.6s
   ⠿ 90a356bcda5b Pull complete                                                                                                        2.7s
   ⠿ c82e0170c13b Pull complete                                                                                                        2.9s
   ⠿ 38dc58c5f029 Pull complete                                                                                                        3.0s
   ⠿ 0189a7ca7a09 Pull complete                                                                                                       40.8s
   ⠿ 8d069dc99fe4 Pull complete                                                                                                       45.2s
   ⠿ 50e62824984f Pull complete                                                                                                       45.6s
   ⠿ c12613b81b5c Pull complete                                                                                                       45.8s
   ⠿ c8901960f9b5 Pull complete                                                                                                       45.9s
   ⠿ a5981c5eb3db Pull complete                                                                                                       46.3s
   ⠿ 860d4f2e0b1a Pull complete                                                                                                       46.8s
   ⠿ 8a3f7b35b548 Pull complete                                                                                                       47.0s
   ⠿ 37c02a7c1188 Pull complete                                                                                                       47.4s
 ⠿ deepfence-celery Pulled                                                                                                            48.2s
 ⠿ deepfence-postgres Pulled                                                                                                          28.2s
   ⠿ 7902437d3a12 Pull complete                                                                                                       10.9s
   ⠿ 709e2267bc98 Pull complete                                                                                                       11.5s
   ⠿ 10c5a0a9c34e Pull complete                                                                                                       21.4s
   ⠿ b46af7f38693 Pull complete                                                                                                       22.4s
   ⠿ 65aa0c237f80 Pull complete                                                                                                       23.3s
   ⠿ f6493ce74812 Pull complete                                                                                                       24.0s
   ⠿ eaac3b44f9d0 Pull complete                                                                                                       24.5s
   ⠿ 3b6db84bcdcc Pull complete                                                                                                       25.1s
   ⠿ 120ba5542ffd Pull complete                                                                                                       25.5s
   ⠿ 9fd0e93acee4 Pull complete                                                                                                       25.8s
   ⠿ 257465d6d91b Pull complete                                                                                                       26.1s
   ⠿ 568fad078a9c Pull complete                                                                                                       26.5s
 ⠿ deepfence-init-container Pulled                                                                                                    14.8s
   ⠿ 2ad0d7925a78 Pull complete                                                                                                       12.6s
   ⠿ 3e00d8e06113 Pull complete                                                                                                       13.2s
 ⠿ deepfence-es Pulled                                                                                                                41.1s
   ⠿ 4e9f2cdf4387 Pull complete                                                                                                       15.3s
   ⠿ 2e70516637d4 Pull complete                                                                                                       37.1s
   ⠿ ba468cc9ce22 Pull complete                                                                                                       37.6s
   ⠿ a2c864bf08ba Pull complete                                                                                                       38.1s
   ⠿ 7869dc55514f Pull complete                                                                                                       38.8s
   ⠿ 6320c0eaee7c Pull complete                                                                                                       39.5s
 ⠿ deepfence-internal-router Pulled                                                                                                    5.1s
 ⠿ deepfence-secret-scanner Pulled                                                                                                    43.2s
   ⠿ c963e9db8328 Pull complete                                                                                                       34.6s
   ⠿ 7f20bc208900 Pull complete                                                                                                       35.0s
   ⠿ a11f9e68c7c1 Pull complete                                                                                                       36.3s
   ⠿ bac7f7e29d55 Pull complete                                                                                                       36.5s
   ⠿ a34450f81e53 Pull complete                                                                                                       37.2s
   ⠿ 8dc83348f20e Pull complete                                                                                                       41.0s
   ⠿ 1789eac203f6 Pull complete                                                                                                       41.6s
 ⠿ deepfence-fetcher Pulled                                                                                                           44.4s
   ⠿ 59bf1c3509f3 Pull complete                                                                                                       22.1s
   ⠿ 7f019d2ddd3b Pull complete                                                                                                       23.2s
   ⠿ d365c5dd6d5a Pull complete                                                                                                       23.7s
   ⠿ 86b2e2904ff3 Pull complete                                                                                                       24.2s
   ⠿ 9e8d01c5b686 Pull complete                                                                                                       42.5s
 ⠿ deepfence-router Pulled                                                                                                             5.1s
   ⠿ df9b9388f04a Pull complete                                                                                                        1.8s
   ⠿ 7c1ae225ee09 Pull complete                                                                                                        1.9s
   ⠿ d67475b58d2b Pull complete                                                                                                        2.5s
   ⠿ 403f08e781eb Pull complete                                                                                                        2.6s
   ⠿ 452f212a53d1 Pull complete                                                                                                        2.8s
   ⠿ 437cebde658b Pull complete                                                                                                        2.8s
   ⠿ 7c9ad45672b2 Pull complete                                                                                                        2.9s
   ⠿ 7d72372a1383 Pull complete                                                                                                        3.1s
   ⠿ d6bb5a9a1f12 Pull complete                                                                                                        3.2s
   ⠿ 7cf149b1a854 Pull complete                                                                                                        3.3s
   ⠿ b6e2429d9e38 Pull complete                                                                                                        3.3s
   ⠿ 92dcd490ec00 Pull complete                                                                                                        3.4s
   ⠿ 617b7a99eac3 Pull complete                                                                                                        3.5s
   ⠿ 259e00ee3450 Pull complete                                                                                                        3.6s
   ⠿ 83bec234123b Pull complete                                                                                                        3.7s
   ⠿ 928a4d3b5efe Pull complete                                                                                                        3.8s
 ⠿ deepfence-topology Pulled                                                                                                          34.0s
   ⠿ a0d0a0d46f8b Pull complete                                                                                                       16.0s
   ⠿ 127cfa2b873d Pull complete                                                                                                       16.6s
   ⠿ 15a88de45267 Pull complete                                                                                                       30.3s
   ⠿ 35233bb75987 Pull complete                                                                                                       32.0s
   ⠿ 8133cfb2a953 Pull complete                                                                                                       32.4s
[+] Running 19/19
 ⠿ Network ubuntu_deepfence_net              Created                                                                                   0.3s
 ⠿ Volume "ubuntu_deepfence_data"            Created                                                                                   0.0s
 ⠿ Container deepfence-postgres              Started                                                                                  16.5s
 ⠿ Container deepfence-secret-scanner        Started                                                                                  18.4s
 ⠿ Container deepfence-vulnerability-mapper  Started                                                                                  17.3s
 ⠿ Container deepfence-diagnosis             Started                                                                                  17.5s
 ⠿ Container deepfence-package-scanner       Started                                                                                  17.7s
 ⠿ Container deepfence-console-agent         Started                                                                                  16.0s
 ⠿ Container deepfence-es-master             Started                                                                                  17.1s
 ⠿ Container df-init-container               Started                                                                                  16.0s
 ⠿ Container deepfence-topology              Started                                                                                  17.3s
 ⠿ Container deepfence-router                Started                                                                                  18.4s
 ⠿ Container deepfence-internal-router       Started                                                                                  17.5s
 ⠿ Container deepfence-redis                 Started                                                                                   2.3s
 ⠿ Container deepfence-ui                    Started                                                                                   2.8s
 ⠿ Container deepfence-fetcher               Started                                                                                   2.4s
 ⠿ Container deepfence-celery                Started                                                                                   3.5s
 ⠿ Container deepfence-api                   Started                                                                                   3.4s
 ⠿ Container deepfence-backend               Started                                                                                   3.1s
```

## check all running docker containers

```rust

docker ps
CONTAINER ID   IMAGE                                                 COMMAND                  CREATED              STATUS              PORTS                                                                      NAMES
4514ee6713ac   deepfenceio/deepfence_api_ce:1.4.1                    "/app/code/dockerify…"   About a minute ago   Up About a minute                                                                              deepfence-api
1ba3ffed69da   deepfenceio/deepfence_api_ce:1.4.1                    "/app/code/dockerify…"   About a minute ago   Up About a minute                                                                              deepfence-backend
8bab0ec9f33c   deepfenceio/deepfence_api_ce:1.4.1                    "/app/code/dockerify…"   About a minute ago   Up About a minute                                                                              deepfence-celery
22656ac2f42e   deepfenceio/deepfence_fetcher_ce:1.4.1                "/usr/bin/start_fetc…"   About a minute ago   Up About a minute   8006/tcp                                                                   deepfence-fetcher
53e90c2fb2ee   deepfenceio/deepfence_ui_ce:1.4.1                     "/home/deepfence/ent…"   About a minute ago   Up About a minute                                                                              deepfence-ui
ef95a5d836af   deepfenceio/deepfence_redis_ce:1.4.1                  "/usr/local/bin/star…"   About a minute ago   Up About a minute   6379/tcp                                                                   deepfence-redis
41861a1a902d   deepfenceio/deepfence_elastic_ce:1.4.1                "/usr/bin/startEs.sh…"   About a minute ago   Up About a minute   127.0.0.1:9200->9200/tcp, 127.0.0.1:9300->9300/tcp                         deepfence-es-master
d7f746d048f1   deepfenceio/deepfence_router_ce:1.4.1                 "docker-entrypoint.s…"   About a minute ago   Up About a minute   0.0.0.0:80->80/tcp, :::80->80/tcp, 0.0.0.0:443->443/tcp, :::443->443/tcp   deepfence-router
c3e6524325b1   deepfenceio/deepfence_vulnerability_mapper_ce:1.4.1   "/entrypoint.sh"         About a minute ago   Up About a minute   8001/tcp                                                                   deepfence-vulnerability-mapper
4331c9eeb50f   deepfenceio/deepfence_router_ce:1.4.1                 "docker-entrypoint.s…"   About a minute ago   Up About a minute   127.0.0.1:8443->443/tcp                                                    deepfence-internal-router
16fae4515f45   deepfenceio/deepfence_diagnosis_ce:1.4.1              "/home/diagnosis"        About a minute ago   Up About a minute                                                                              deepfence-diagnosis
515a139b6c8f   deepfenceio/deepfence_agent_ce:1.4.1                  "/usr/local/bin/star…"   About a minute ago   Up About a minute                                                                              deepfence-console-agent
10ac5757f982   deepfenceio/deepfence_postgres_ce:1.4.1               "docker-entrypoint.s…"   About a minute ago   Up About a minute   5432/tcp                                                                   deepfence-postgres
50621d650d96   deepfenceio/deepfence_package_scanner_ce:1.4.1        "/usr/local/bin/pack…"   About a minute ago   Up About a minute   8002/tcp, 8005/tcp                                                         deepfence-package-scanner
0e96086bc3d0   deepfenceio/deepfence_discovery_ce:1.4.1              "/home/deepfence/ent…"   About a minute ago   Up About a minute   127.0.0.1:8004->8004/tcp                                                   deepfence-topology
bda5019a336a   deepfenceio/deepfence_secret_scanner_ce:1.4.1         "/home/deepfence/usr…"   About a minute ago   Up About a minute                                                                              deepfence-secret-scanner
```

access threatmapper console using AWS instance public IP

54.152.156.120 (in your case check your EC2 instance public IP )

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671720365441/b1c13f69-94ee-4db1-85e7-705c85adbe4a.png )

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671720385153/020c94de-a9bb-4d2b-a4c1-8b7101377565.png )

next part of this you will find here you can do the same deployment using Kubernetes or Helm