---
title: " Setup Remote network "
description: " weave driver "
---

#### Docker network managment command

```sh
docker network --help
clear
```

### Starting on ubuntu-node01: Install and setup the weave driver

```sh
sudo curl -L git.io/weave -o /usr/local/bin/weave
sudo chmod a+x /usr/local/bin/weave
export CHECKPOINT_DISABLE=1
weave launch
eval $(weave env)
```

clear

#### Now on ubuntu-node02: Install and setup the weave driver
```sh
sudo curl -L git.io/weave -o /usr/local/bin/weave
sudo chmod a+x /usr/local/bin/weave
export CHECKPOINT_DISABLE=1
weave launch
eval $(weave env)
```
clear

#### Now, back on ubuntu-node01:Bring node02 in as a peer on node01's weave network
```sh
weave connect ubuntu-node02
```
#### Starting with ubuntu-node01:Run a container detached on node01
```sh
docker container run -d --name app01 alpine tail -f /dev/null
```
#### Now, launch a container on ubuntu-node02: Run a container detached on node02
```sh
docker container run -d --name app02 alpine tail -f /dev/null
```
#### Since we are on node02, we will check there first...
#### From inside the app02 container running on node02,
####  let's ping the app01 container running on node01
```sh
docker container exec -it app02 ping -c 4 app01
```
#### Similarly, from inside the app01 container running on node01,
#### let's ping the app02 container running on node02
```sh
docker container exec -it app01 ping -c 4 app02
```
clear
