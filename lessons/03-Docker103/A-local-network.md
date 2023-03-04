---
title: " Setup local network "
description: " overlay network "
---

# Docker network create command syntax
```
docker network create --help
```
# view current networks
```
docker network ls
clear
```

# Create a new overlay network, with all default options
```
docker network create -d overlay defaults-over
```
# Create a new overlay network with specific IP settings

```
docker network create -d overlay \
--subnet=172.30.0.0/24 \
--ip-range=172.30.0.0/28 \
--gateway=172.30.0.254 \
specifics-over
```

# view current networks again
```
docker network ls
```
# Initial validation
```
docker network inspect specifics-over --format '{{json .IPAM.Config}}' | jq
```
clear

# Create service tester1
```
docker service create --detach --replicas 3 --name tester1 \
--network specifics-over alpine tail -f /dev/null
```

# Create service tester2
```
docker service create --detach --replicas 3 --name tester2 \
--network specifics-over alpine tail -f /dev/null
```

# get the container names
```
docker container ls
```
# From a container in the tester1 service ping the tester2 service by name

```
docker container exec -it tester1.3.<GET THE NAME> ping -c 3 tester2
```

