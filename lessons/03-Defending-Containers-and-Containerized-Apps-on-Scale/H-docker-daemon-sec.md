---

title: " Docker Daemon security configurations"
description: "  "

---
 
 
 # docker daemon security configuration  


Install docker CE 19.03 

```bash

# yum install -y yum-utils device-mapper-persistent-data lvm2
# yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

# yum install -y docker-ce

[root@localhost docker]# docker --version
Docker version 19.03.8, build afacb8b

```


# Daemon security configuration

There is no configuration file by default, which needs to be created separately/etc/docker/daemon.json, the following configurations are all local test examples configured on this file.


```json
{
 "icc": false,
 "log-level": "info",
 "log-driver": "json-file",
 "log-opts": {
 "max-size": "10m",
 "max-file":"5",
 "labels": "somelabel",
 "env": "os,customer"
 },
 "iptables": true,
 "userns-remap": "default",
 "userland-proxy": false,
 "experimental": false,
 "selinux-enabled": true,
 "live-restore": true,
 "no-new-privileges": true,
 "cgroup-parent": "/foobar",
 "seccomp-profile": "/etc/docker/seccomp/default-no-chmod.json",
 "tls": true,
 "tlsverify": true,
 "tlscacert": "/etc/docker/CA/ca.pem",
 "tlscert": "/etc/docker/CA/server-cert.pem",
 "tlskey": "/etc/docker/CA/server-key.pem"
}


```

# configure access to docker daemon through HTTPS and certificate authentication

Server certificate

Create a host and define a domain (IP can also be used). The corresponding certificate will be generated according to the domain. It is generally used to register the CN in the certificate:

Create certificate Directory:

```bash
$ mkdir -p /etc/docker/dockerd/CA && cd /etc/docker/dockerd/CA
```


Generate the key certificate and fill in the key certificate password twice:
```bash
$ openssl genrsa -aes256 -out ca-key.pem 4096
```
To generate a CA certificate, you need to enter the basic information of the registration certificate:
```bash
$ openssl req -new -x509 -days 365 -key ca-key.pem -sha256 -out ca.pem
```
Create server certificate:

```bash
$ openssl genrsa -out server-key.pem 4096

$ openssl req -subj "/CN=localhsot" -sha256 -new -key server-key.pem -out server.csr
```

Set the IP address specified by the certificate:


```bash
$ echo subjectAltName = DNS:localhost,IP:127.0.0.1 >> extfile.cnf
```
Set the extended usage property of the docker daemon key to server authentication only:
```bash
$ echo extendedKeyUsage = serverAuth >> extfile.cnf
```
Generate server cert certificate:
```bash
$ openssl x509 -req -days 3650 -sha256 -in server.csr -CA ca.pem -CAkey ca-key.pem -CAcreateserial -out server-cert.pem -extfile extfile.cnf
```
Client certificate

Create client certificate: (or current directory)
```bash
$ openssl genrsa -out key.pem 4096
$ openssl req -subj '/CN=localhost' -new -key key.pem -out client.csr

```
To make the key suitable for client authentication, create an extended profile:


```bash
$ echo extendedKeyUsage = clientAuth >> extfile.cnf
```

Generate client cert certificate:

```bash
$ openssl x509 -req -days 3650 -sha256 -in client.csr -CA ca.pem -CAkey ca-key.pem -CAcreateserial -out cert.pem -extfile extfile.cnf

```

use

Give corresponding permissions to the certificate:

```bash
$ chmod -v 0400 ca-key.pem key.pem server-key.pem
$ chmod -v 0444 ca.pem server-cert.pem cert.pem

[root@localhost CA]# ls
ca-key.pem ca.pem ca.srl cert.pem client.csr extfile.cnf key.pem server-cert.pem server.csr server-key.pem

```
Server configuration /etc/docker/daemon.json

```json

"tls": true,
"tlsverify": true,
"tlscacert": "/etc/docker/CA/ca.pem",
"tlscert": "/etc/docker/CA/server-cert.pem",
"tlskey": "/etc/docker/CA/server-key.pem"
```

Client configuration

Set the client certificate on the server and place it in the corresponding location:

```bash
$ cp -v {ca,cert,key}.pem ~/.docker
$ export DOCKER_HOST=tcp://$HOST:2376 DOCKER_TLS_VERIFY=1

```

Simulate the test as follows:

```json
$ curl https://$HOST:2376/images/json 
 --cert ~/.docker/cert.pem 
 --key ~/.docker/key.pem 
 --cacert ~/.docker/ca.pem
 
[{"Containers":-1,"Created":1540777343,"Id":"sha256:55e7b305dc477345434ce3bd3941940481f982eea31c8f28c0670d59c63d544b","Labels":nu
```

# using namespace isolation technology

Namespace is an isolation technology. Docker uses the isolation technology to open a specific namespace and create some special processes, but the use of namespace is conditional. The system will create a dockremap and map it to the container through the ID values corresponding to / etc / subuid and / etc / subuid; The actual situation still uses the ordinary permission of dockremap to achieve the effect of automatic isolation.



Modify first/etc/sysctl.conf
```conf
# echo “user.max_user_namespaces=15076” >> /etc/sysctl.conf
```
stay /etc/docker/daemon.json Add the configuration item “userns remap”: “default”

Be careful when modifying this configuration. If you have deployed a set of docker environment, after enabling this option, you will switch to the isolated environment, and the previous docker container will not be used!


```

[root@localhost docker]# cat /etc/subuid
dockremap:100000:65536

```

#  setting the partition of docker

Create a separate partition for the container. The default partition isvarlibdocker, including local images, containers, networks and other related things.

```bash

root@localhost docker]# ls /var/lib/docker
```

100000.100000  builder  buildkit  containers  image  network  overlay2  plugins  runtimes  swarm  tmp  trust  volumes

You can use “data root”: “to configure the default partition location.

# limit traffic between default bridge containers

When the docker service is started, a forwarding policy will be added to the forward chain of iptables by default. Whether the policy is accept or drop depends on whether — ICC = true (default) or — ICC = false is configured. If — iptables = false is manually specified, iptables rules will not be added.


By default, all network communication is allowed between containers on the same host on the default bridge. If not required, the communication between all containers is limited. Link specific containers that need to communicate together, or create a custom network and join only containers that need to communicate with the custom network.


Configure to limit the traffic “ICC” between containers on the default bridge: false

# configuration log

Configure the centralized remote log, set the log process — log level level to info, log record format JSON, local log record

```json

"log-level": "info",
"log-driver": "json-file",
"log-opts": {
 "max-size": "10m",
 "max-file":"5",
 "labels": "somelabel",
 "env": "os,customer"
},

```

![Alt text](./images/config-remote.jpg)


The docker logging driver receives the container log and forwards it to a remote destination or file. The default logging driver isjson-file。 It stores container logs on local disk in JSON format. Docker has a plug-in architecture for logging, so there are plug-ins for open source tools and commercial tools:


Journaled – stores the container log in the system log

Syslog driver – supports UDP, TCP, TLS

Fluent D – supports connecting TCP or UNIX sockets to fluent D

Splunk – http / HTTPS forwarding to Splunk server

Gel – UDP logs forwarded to graylog2

Example fluent


```
{
 "log-driver": "fluentd",
 "log-opts": {
 "fluentd-address": "fluentdhost:24224"
 }
 }

```

Using syslog

```
{
 "log-driver": "syslog",
 "log-opts": {
 "syslog-address": "udp://1.2.3.4:1111"
 }
}
```

# setting ulimit


```
{
 "default-ulimits": {
 "nofile": {
  "Name": "nofile",
  "Hard": 64000,
  "Soft": 64000
 }
 }
}
```

#  setting CGroup

The cggroup parent option allows you to set the default cggroup parent for the container. If this option is not set, the default value for FS CGroup driver is / docker; For SYSTEMd CGroup driver, the default is system slice 。

If CGroup has a forward slash (/), CGroup is created under the root CGroup, otherwise CGroup is created under the daemon CGroup.


Assuming that the daemon runs in CGroup daemon CGroup, then — CGroup parent = / foobar creates a CGroup in / sys / FS / CGroup / memory / foobar, while — CGroup parent = foobar creates a CGroup/sys/fs/cgroup/memory/daemoncgroup/foobar Create CGroup in.

SYSTEMd CGroup driver has different rules for – CGroup parent. System D represents the hierarchy by slice, and the name of the slice encodes the position in the tree. Therefore, the — CGroup parent of SYSTEMd CGroup should be the slice name. Names can contain a series of names separated by dashes that describe the path from the root slice to the slice. For example, — CGroup parent = user-a-b.slice indicates that the memory of the container is CGroup /sys/fs/cgroup/memory/user.slice/user-a.slice/user-a-b.slice/docker-.scope Created in.

You can also use container run to set it. Using the — CGroup parent option on docker create and docker run will take precedence over the — CGroup parent option on the daemon.


# configuring seccomp

For the test configuration file used, it is forbidden to use the Chmod command in docker

```
https://github.com/docker/labs/blob/master/security/seccomp/seccomp-profiles/default-no-chmod.json
[root@localhost docker]# docker run --rm -it alpine sh
/ # ls bin etc lib mnt proc run srv tmp var
dev home media opt root sbin sys usr / # touch foo.sh
/ # chmod +x foo.sh
chmod: foo.sh: Operation not permitted
/ # exit




```
It can actually complete some system related calls of prohibition, permission and alarm. Refer to:https://github.com/torvalds/linux/blob/master/arch/x86/entry/syscalls/syscall_64.tbl

# disable the experimental function of docker

Set “experimental”: false

2.11 restrict containers from raising rights through suid or sgid

The no new privileges security option prevents application processes within the container from gaining new privileges during execution.

For example: there is a program with setuid / setgid bit set in the image, such as sudo. The process in the container also has (file) permission to execute the program. Any operation attempting to obtain privileges through facilities such as setuid / setgid will be rejected.

#  Daemon configuration example description (Linux)


```bash
{
  "authorization-plugins": [],//access authorization plugin
  "data-root": "", //the root directory of docker data persistent storage, the default is /var/lib/docker
  "dns": [], //DNS server
  "dns-opts": [],//DNS configuration options, such as ports, etc.
  "dns-search": [],//DNS search domain name
  "exec-opts": [], //execution options
  "exec-root": "",//The root directory of the file in the execution state
  "experimental": false,//whether to enable experimental features
  "features": {},//Enable or disable specific features. Such as: {"buildkit": true} makes buildkit the default docker image builder.
  "storage-driver": "",//Storage driver type
  "storage-opts": [],//storage options
  "labels": [],//key-value pair label docker metadata
  "live-restore": true, //whether to keep the container alive when dockerd hangs up (to avoid the container exit caused by the docker service exception)
  "log-driver": "json-file",//The driver of the container log
  "log-opts": {
  "max-size": "10m",
  "max-file": "5",
  "labels": "somelabel",
  "env": "os,customer"
  },//Options for container logs
  "mtu": 0,//Set container network MTU (Maximum Transmission Unit)
  "pidfile": "",//The location of the daemon PID file
  "cluster-store": "",//URL of the cluster storage system
  "cluster-store-opts": {},//Configure cluster storage
  "cluster-advertise": "",//External address name
  "max-concurrent-downloads": 3,//Set the maximum concurrency of each pull process
  "max-concurrent-uploads": 5,//Set the maximum concurrency of each push process
  "default-shm-size": "64M",//Set the default shared memory size
  "shutdown-timeout": 15,//Set the shutdown timeout period
  "debug": true,//Enable debug mode
  "hosts": [],//The listening address of the dockerd daemon process
  "log-level": "",//log level
  "tls": true, //Enable the Transport Layer Security Protocol TLS
  "tlsverify": true, //Enable the transport layer security protocol and verify the remote address
  "tlscacert": "",//CA signature file path
  "tlscert": "",//TLS certificate file path
  "tlskey": "",//TLS key file path
  "swarm-default-advertise-addr": "", //swarm external address
  "api-cors-header": "",//Set CORS (Cross-origin resource sharing) header
  "selinux-enabled": false,//Enable selinux (mandatory access control for users, processes, applications, files)
  "userns-remap": "",//Set user/group for user namespace
  "group": "", //Docker is in the group
  "cgroup-parent": "",//Set the parent class of cgroup of all containers
  "default-ulimits": {
  "nofile": {
   "Name": "nofile",
   "Hard": 64000,
   "Soft": 64000
  }
  },//Set the ulimit of all containers
  "init": false,//The container performs initialization to forward signals or control (reap) processes
  "init-path": "/usr/libexec/docker-init", //docker-init file path
  "ipv6": false,//support IPV6 network
  "iptables": false,//Enable firewall rules
  "ip-forward": false, //Open net.ipv4.ip_forward
  "ip-masq": false,//Enable ip masking (the technology of rewriting the source IP address or destination IP address when the IP packet passes through a router or firewall)
  "userland-proxy": false, //userland proxy
  "userland-proxy-path": "/usr/libexec/docker-proxy", //userland proxy path
  "ip": "0.0.0.0",//Default IP
  "bridge": "",//Attach the container to the bridge identifier on the bridge network
  "bip": "",//Specify bridge IP
  "fixed-cidr": "",//(ipv4) subnetting, that is, limiting the range of ip address allocation to control the network segment to which the container belongs to achieve network access between containers (the same host or between different hosts)
  "fixed-cidr-v6": "", //(ipv6) subnetting
  "default-gateway": "",//default gateway
  "default-gateway-v6": "",//default ipv6 gateway
  "icc": false,//Inter-container communication
  "raw-logs": false, //raw logs (no color, full timestamp)
  "allow-nondistributable-artifacts": [],//Registry warehouse submitted by products that are not distributed externally
  "registry-mirrors": [],//registry warehouse mirror acceleration address
  "seccomp-profile": "", //seccomp configuration file
  "insecure-registries": [],//Configure non-https registry address
  "no-new-privileges": false, //Disable new privileges
  "default-runtime": "runc", //OCI alliance (The Open Container Initiative) default runtime environment
  "oom-score-adjust": -500,//Priority of memory overflow being killed (-1000~1000)
  "node-generic-resources": ["NVIDIA-GPU=UUID1", "NVIDIA-GPU=UUID2"],//Resource nodes announced to the public
  "runtimes": {
  "cc-runtime": {
   "path": "/usr/bin/cc-runtime"
  },
  "custom": {
   "path": "/usr/local/bin/my-runc-replacement",
   "runtimeArgs": [
   "--debug"
   ]
  }
  },//Runtime
  "default-address-pools": [
  {"base":"172.80.0.0/16","size":24}, //Default dhcp assigned address
  {"base":"172.90.0.0/16","size":24}
  ]
}

```
