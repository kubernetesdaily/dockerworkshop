---
title: " Named Volume Mount - Data persistentence "
description: " Named volume sharing data "
---

### create volume 

```sh
 6-DC-name-volume git:(main) ✗ docker volume create --name my-vol 
my-vol
```


#### create docker-compose.yml with following content 

```yml
version: '3.7'

services:
  db:
    image: mysql:latest
    restart: always
    container_name: myphpapp-db
    environment:
       MYSQL_ROOT_PASSWORD: somepass
       MYSQL_DATABASE: somedatabase
    volumes:
      - my-vol:/var/lib/mysql

volumes:
  my-vol:
    name: my-vol
```

#### run docker compose 

```sh

docker compose up -d 
WARN[0000] volume "my-vol" already exists but was not created by Docker Compose. Use `external: true` to use an existing volume 
[+] Running 1/1
 ⠿ Container myphpapp-db  Started                                                                         0.3s
➜  6-DC-name-volume git:(main) ✗ 

```
#### start new container with ubuntu 
```sh
docker run -v my-vol:/mydata --rm -it ubuntu /bin/bash
```

show the database data files 

```sh
root@1666e5d6f315:/# cd mydata/
root@1666e5d6f315:/mydata# ls
'#ib_16384_0.dblwr'   binlog.000001   client-cert.pem   mysql                public_key.pem    undo_001
'#ib_16384_1.dblwr'   binlog.000002   client-key.pem    mysql.ibd            server-cert.pem   undo_002
'#innodb_redo'        binlog.index    ib_buffer_pool    mysql.sock           server-key.pem
'#innodb_temp'        ca-key.pem      ibdata1           performance_schema   somedatabase
 auto.cnf             ca.pem          ibtmp1            private_key.pem      sys
root@1666e5d6f315:/mydata# 
root@1666e5d6f315:/mydata# exit 
exit
```

### share data between two containers? lets try ?

```sh
6-DC-name-volume git:(main) ✗ docker volume create --name Datastore1
Datastore1
```
#### open a shell woth datastore1 in /mydatabase
```sh
docker run -v Datastore1:/mydatastore --rm -it ubuntu /bin/bash
```
#### write a new text file 

```sh
echo "hello datastore1" > /mydatastore/hello.txt
root@4b119677c00b:/# ls
bin  boot  dev  etc  home  lib  media  mnt  mydatastore  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@4b119677c00b:/# cat  mydatastore/hello.txt 
hello datastore1
root@4b119677c00b:/# 

```

#### add another line 

```sh
root@4b119677c00b:/# echo "\n\nhello datastore 2" >> /mydatastore/hello.txt
root@4b119677c00b:/# cat  mydatastore/hello.txt 
hello datastore1
\n\nhello datastore 2
root@4b119677c00b:/# 
```