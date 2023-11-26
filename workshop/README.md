## Introduction to Containers 

1. [What is a container?](./Intro-Containers/what-is-container.md)
2. Basics of a container and its challenges
     - [Container vs. Virtualization](/Intro-Containers/containervsVM.md)
     - [Container Advantages](./Intro-Containers/Container-adv-dis.md#container-advantages)
     - [Container Disadvantages](./Intro-Containers/Container-adv-dis.md#container-disadvantages)
3. Container fundamentals
    - [Namespaces](./Intro-Containers/Namespaces.md)
    - [Cgroup](./Intro-Containers/Cgroup.md)
    - [Capabilities](./Intro-Containers/Capabilities.md)
4. [Docker architecture and its components](./Intro-Containers/docker-architecture.md#docker-architecture)
    - [Docker CLI](./Intro-Containers/docker-architecture.md#docker-client)
    - [Docker Engine (Daemon, API)](./Intro-Containers/docker-architecture.md#docker-engine)
    - [Docker Runtime (containerd, shim, runc)](./Intro-Containers/docker-architecture.md#docker-runtime)
5. [Interacting with container ecosystem](./Intro-Containers/Intracting-with-container.md)
     - [Docker images and image layers](./Intro-Containers/Intracting-with-container.md#understand-image-layers) <br>
     - [Build Container images using Dockerfile](./Intro-Containers/Intracting-with-container.md#dockerfile-instructions-with-example)<br>
     - [Storing data in a container(mounts, volumes, etc.)](./Intro-Containers/Intracting-with-container.md#storing-data-in-a-container)<br>
     - [Networking in containers](./Intro-Containers/Intracting-with-container.md#networking-in-containers)<br>
     - [Docker Compose](./Intro-Containers/Intracting-with-container.md#docker-compose)
  
6. Managing / Orchestrating multiple containers
   - Using CLI/API to manage multiple containers
   - Docker Compose
   - Kubernetes
   - Nomad
7. Docker alternatives(Podman, rkt)

     
## Container Reconnaissance

1. Overview of Container Security
2. [Attack surface of the container ecosystem](./container-sec/attack-surface.md)
3. Analysis of the attack surface
    - Using native tools
    - Using third-party tools
4. Identifying the components and their security state
    1. Get an inventory of containers
      - [Environment variables](./container-sec/Envir-variables.md)
      - [Docker volumes](./container-sec/Docker-volumes.md)
      - [Networking](./container-sec/Docker-Networking.md)
      - Ports used/Port forwarding
     2. Capabilities and namespaces in Docker
5. Hands-on Exercises:
   - [Docker Security Benchmark](./container-sec/Auditing-docker-sec.md)
   - [CIS Docker Benchmark - InSpec Profile](https://dev-sec.io/baselines/docker/)
   - [Lynis](https://github.com/CISOfy/Lynis)

## Defending Containers and Containerized Apps on Scale

1. Container image security
   1. [Building secure container images](/container-app-sec/Container-image-security.md)
     - [Choosing base images](/container-app-sec/Container-image-security.md#choosing-base-images)
     - [Distroless images](/container-app-sec/Container-image-security.md#distroless-images)
     - [Scratch images]((/container-app-sec/Container-image-security.md#scratch-images))
     - [DockerFile Security Best Practices](/container-app-sec/dockerfile-security.md)
     - [SecretScanner - Finding secrets and passwords in container images and file systems](/container-app-sec/SecretScanner.md)
     - [YaraHunter - Malware Scanner for Container Images](/container-app-sec/Yarahunter.md)

   2. [Security Linting of Dockerfiles](./container-app-sec/security-linting-dockerfile.md)
   3. [Static Analysis of container images/library for container](./container-app-sec/packetscanner.md)
   
2. Docker host security configurations
    - [Kernel Hardening using SecComp and AppArmor](/container-app-sec/SecComp-AppArmor.md)
    - [Custom policy creation using SecComp and AppArmor](/container-app-sec/SecComp-AppArmor.md)
3. Docker Daemon security configurations
    1. [Daemon security configuration](./container-app-sec/docker-daemon-sec.md#daemon-security-configuration)<br>
    2. [configure access to docker daemon through HTTPS and certificate authentication](/container-app-sec/docker-daemon-sec.md#configure-access-to-docker-daemon-through-https-and-certificate-authentication)<br>
    3. [using namespace isolation technology](./container-app-sec/docker-daemon-sec.md#using-namespace-isolation-technology)<br>
    4. [setting the partition of docker](/container-app-sec/docker-daemon-sec.md#setting-the-partition-of-docker)<br>
    5. [limit traffic between default bridge containers](/container-app-sec/docker-daemon-sec.md#limit-traffic-between-default-bridge-containers)<br>
    6. [configuration log](/container-app-sec/docker-daemon-sec.md#configuration-log)<br>
    7. [setting ulimit](/container-app-sec/docker-daemon-sec.md#setting-ulimit)<br>
    8. [setting CGroup](/container-app-sec/docker-daemon-sec.md#setting-cgroup)<br>
    9. [configuring seccomp](/container-app-sec/docker-daemon-sec.md#configuring-seccomp) <br>
    10. [disable the experimental function of docker](/container-app-sec/docker-daemon-sec.md#disable-the-experimental-function-of-docker) <br>
    11. [Daemon configuration example description (Linux)](/container-app-sec/docker-daemon-sec.md#daemon-configuration-example-description-linux)<br>

4. Network Security in containers
   - Segregating networks
5. Misc Docker Security Configurations
   - [Content Trust and Integrity checks](/container-app-sec/DCT.md)
6. [Docker Registry security configurations](./container-app-sec/docker-registry.md)
7. Docker Tools, Techniques and Tactics
   1. Tools
    - [Dockerscan](./container-app-sec/dockerscan.md) 
    - [Dive](/container-app-sec/Dive.md)
    - Dockle
 
8. Hands-on Exercises:

   - [Scanning Docker for vulnerabilities with ThraetMapper](/container-app-sec/ThreatMapper-app.md)
   
## Security Monitoring of Containers

1. Monitoring and incident response in containers
2. [Docker events](./container-monitoring.md/docker-events.md)
3. Docker logs
4. Docker runtime prevention
5. Hands-on Exercises:
   - [Weave Scope - Container monitoring and visualization](/container-monitoring.md/Sock-shop-weave.md)
   - Anchore Engine – Policy creation and enforcement
   - VMWare Harbor – Securing Docker image with Harbor

## Docker Attack - Use Cases 

1. [Kinising Malware attack](./use-cases/kinsing-Malware-attack.md) 
2. [Doki Malware Attack](./use-cases/Doki-malware-attack.md) 

// write c program bubble sort 

