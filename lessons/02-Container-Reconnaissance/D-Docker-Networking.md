---

title: " Docker Networking "
description: " Docker Networking "

---

## Docker Networking 

The Container Netwotk Model (CNM) is the design document for Docker Networking which is implemented via 'libnetwork' in golang 

- Sandbox - isolated network stavk 
- Endpount - Virtual Ethernet Interfece 
- Network - Virtual Switch (bridge)


Note - Libnetwork -> implemenrs the control and management plane functions 

Network Specific Drivers ->  implemenrs the data plane while also handling the connectivity and isolation 


# Docker Native Network Drivers 

- Bridge  - uses a software bridge whicha allows containers connected to the same bridge network to communicate 

- Host - Connects the docker host and containers to use hosts networking directly 

- Overlay - used for multi-host networking which allows a single network to span multiple hosts such that containers on diffenet hosts can communicate at layer 2 


- Macvlan - Useful for lagacy and monitor apps which expect to directly conneted to physical network 
as it assigns completely the networkinh stack on container . No IP will be configuared with this driver 


# Docker Networking ( Hands-on )

```
$ docker network --help

```
1. check what networks are already present in the docker by default 

```
$ dokcer run -itd --name netcon ubuntu /bin/bash 
```
check for the network placement of this container in the above networks. We see that by default all containers got attached to bridge network using `network inspect command  ` this could be done 

```
$ docker network inspect bridge 
```

In the `Container  ` option abobe the information about container name , IPv4 etc present 

Run another container `net2con` from busybox image and check its network placement same as above 

```
docker run -itd --name net2con busybox /bin/bash 
```

lets us now run nginx container as `net3con-web` with exposed port 

```
docker run -itd --name net3con-web -p 5000:80 nginx 

```

to check for the port correctly setup and exposed , one can use `port ` command followed by container name .

```

docker port net3con-web
```

to visualize only the relavant information which is the ` Containers` block from `inspect` command output use `-f` option to format the JSON and parse it via `jq` ulaitity 

```
docker network inspect bridege -f "{{json.Containers}}" | jq
```

one can also oberserve the associated `bridge.id` to `docker0` default bridge with all the containers attached via `vethxxxxxx` virtual ethernet interfces 

```
$brctl show 
```

# concusion 

Cgroups and Namespaces provides essential isolation and limits the container resources.  This is highly valuable for the Docker host system in case of DoS Attacks wherein if resources  are set minimally, less harm will be there. For resource isolation, feel free to look over to the  Linux 'cgcreate', 'cgdelete', 'cgset' and related parameters like 'cpuset', 'cpuacct', etc.  Running containers and walking around the namespaces, cgroups, container networking



