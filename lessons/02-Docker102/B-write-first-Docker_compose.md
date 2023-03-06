---
title: " Write First Docker Compose file "
description: "Docker Compose"
---

cat Dockerfile 

```dockerfile
FROM php:7.2-apache
COPY index.php /var/www/html

```

add index.php file  

```php
<?php

echo "hello world \n\n";
```

creat docker-compose.yaml 

```yml
version: '3'
services:
  phpapp:
    ports:
      - "8080:80"
    build:
      context: ./
      dockerfile: Dockerfile

```

to run docker compose 

```sh

docker compose up --build
```

output 

```sh
 1-DC git:(main) ✗ docker compose up 
[+] Building 21.2s (10/10) FINISHED                                                                                                                         
 => [internal] booting buildkit                                                                                                                        3.8s
 => => pulling image moby/buildkit:buildx-stable-1                                                                                                     2.8s
 => => creating container buildx_buildkit_great_brahmagupta0                                                                                           1.1s
 => [internal] load build definition from Dockerfile                                                                                                   0.0s
 => => transferring dockerfile: 85B                                                                                                                    0.0s
 => [internal] load .dockerignore                                                                                                                      0.0s
 => => transferring context: 2B                                                                                                                        0.0s
 => [internal] load metadata for docker.io/library/php:7.2-apache                                                                                      4.3s
 => [auth] library/php:pull token for registry-1.docker.io                                                                                             0.0s
 => [internal] load build context                                                                                                                      0.0s
 => => transferring context: 67B                                                                                                                       0.0s
 => [1/2] FROM docker.io/library/php:7.2-apache@sha256:4dc0f0115acf8c2f0df69295ae822e49f5ad5fe849725847f15aa0e5802b55f8                                8.3s
 => => resolve docker.io/library/php:7.2-apache@sha256:4dc0f0115acf8c2f0df69295ae822e49f5ad5fe849725847f15aa0e5802b55f8                                0.0s
 => => sha256:9639d8c8cc76eb6501b4135a054c1a85bd7397db1010d043bcc03d32bf6d79b6 895B / 895B                                                             0.5s
 => => sha256:119f7607f913c50661a95311027b7b944c7bf2ee9e7af5361a14b72ce4d36b34 247B / 247B                                                             0.3s
 => => sha256:c5ff78edaefc7917757ec4e434738d5561d016bedd20ebdeeee362ec53c8d200 2.27kB / 2.27kB                                                         0.6s
 => => sha256:1c04bb0b5fbe7c2927e6282625ccf4c5e0399e9da46f4a84bb8072dc98add6e5 214B / 214B                                                             0.5s
 => => sha256:709f68bc1d50527b9d9de50c516cb337468feb59c6135b414dda628d902a1bc8 13.52MB / 13.52MB                                                       3.2s
 => => sha256:9e92e56de9f5b0243dc28d80127f51f9f773d64ce4ae1ad58cfff06c2dcd4c29 494B / 494B                                                             0.8s
 => => sha256:485201b000c7ca5a73c2f9d047a204442ea5871a8a1405809d623884d83afc05 12.65MB / 12.65MB                                                       0.9s
 => => sha256:28135fd83ed1ac66e6f4288fabe4c84616cfe420e8e6808ba92d052771325ab4 517B / 517B                                                             0.5s
 => => sha256:5543a36f8eed548f436944bf39a2e918115fc9b30b54416c1081566f25716010 475B / 475B                                                             0.6s
 => => sha256:8d436d7bb0262f042a9554b5c54b25bc1d19cabbb9435747672c34d12f8dc1d0 18.58MB / 18.58MB                                                       1.5s
 => => sha256:c17a0a78e91d3ac0ec4a0c0566f57580154ead8d8967258efe94989d86cd05bd 269B / 269B                                                             1.4s
 => => sha256:30eb7a300f132babe7d5ed65f9e81a1fdd4542ecf70ac29a91bc290484dbc5e5 70.34MB / 70.34MB                                                       3.2s
 => => sha256:c9648d7fcbb6d597cf33916d8fcd207fde8ec05d764b4480d4f3e884e142a902 25.86MB / 25.86MB                                                       3.1s
 => => sha256:f88cecc04e76783f0006b9fed72be749e834825383e941e16de2565a0e4a8cc3 229B / 229B                                                             0.5s
 => => extracting sha256:c9648d7fcbb6d597cf33916d8fcd207fde8ec05d764b4480d4f3e884e142a902                                                              0.6s
 => => extracting sha256:f88cecc04e76783f0006b9fed72be749e834825383e941e16de2565a0e4a8cc3                                                              0.0s
 => => extracting sha256:30eb7a300f132babe7d5ed65f9e81a1fdd4542ecf70ac29a91bc290484dbc5e5                                                              1.2s
 => => extracting sha256:c17a0a78e91d3ac0ec4a0c0566f57580154ead8d8967258efe94989d86cd05bd                                                              0.0s
 => => extracting sha256:8d436d7bb0262f042a9554b5c54b25bc1d19cabbb9435747672c34d12f8dc1d0                                                              0.2s
 => => extracting sha256:5543a36f8eed548f436944bf39a2e918115fc9b30b54416c1081566f25716010                                                              0.0s
 => => extracting sha256:28135fd83ed1ac66e6f4288fabe4c84616cfe420e8e6808ba92d052771325ab4                                                              0.0s
 => => extracting sha256:485201b000c7ca5a73c2f9d047a204442ea5871a8a1405809d623884d83afc05                                                              0.1s
 => => extracting sha256:9e92e56de9f5b0243dc28d80127f51f9f773d64ce4ae1ad58cfff06c2dcd4c29                                                              0.0s
 => => extracting sha256:709f68bc1d50527b9d9de50c516cb337468feb59c6135b414dda628d902a1bc8                                                              0.2s
 => => extracting sha256:c5ff78edaefc7917757ec4e434738d5561d016bedd20ebdeeee362ec53c8d200                                                              0.0s
 => => extracting sha256:119f7607f913c50661a95311027b7b944c7bf2ee9e7af5361a14b72ce4d36b34                                                              0.0s
 => => extracting sha256:1c04bb0b5fbe7c2927e6282625ccf4c5e0399e9da46f4a84bb8072dc98add6e5                                                              0.0s
 => => extracting sha256:9639d8c8cc76eb6501b4135a054c1a85bd7397db1010d043bcc03d32bf6d79b6                                                              0.0s
 => [2/2] COPY index.php /var/www/html                                                                                                                 0.3s
 => exporting to docker image format                                                                                                                   4.3s
 => => exporting layers                                                                                                                                0.0s
 => => exporting manifest sha256:4389df930ccac33c104717e827e5a6dc3de4ef60632784ca9ea76806b1bf88b7                                                      0.0s
 => => exporting config sha256:bcbc4a7a409f828a025ca857acb752536f4cece24a93121e6a920e0c4d60050f                                                        0.0s
 => => sending tarball                                                                                                                                 4.2s
 => importing to docker                                                                                                                                2.7s
[+] Running 2/2
 ⠿ Network 1-dc_default     Created                                                                                                                    0.1s
 ⠿ Container 1-dc-phpapp-1  Created                                                                                                                    0.3s
Attaching to 1-dc-phpapp-1
1-dc-phpapp-1  | AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 172.18.0.2. Set the 'ServerName' directive globally to suppress this message
1-dc-phpapp-1  | AH00558: apache2: Could not reliably determine the server's fully qualified domain name, using 172.18.0.2. Set the 'ServerName' directive globally to suppress this message
1-dc-phpapp-1  | [Fri Mar 03 00:44:20.034230 2023] [mpm_prefork:notice] [pid 1] AH00163: Apache/2.4.38 (Debian) PHP/7.2.34 configured -- resuming normal operations
1-dc-phpapp-1  | [Fri Mar 03 00:44:20.034331 2023] [core:notice] [pid 1] AH00094: Command line: 'apache2 -D FOREGROUND'
1-dc-phpapp-1  | 172.18.0.1 - - [03/Mar/2023:00:44:54 +0000] "GET / HTTP/1.1" 200 244 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/16.3 Safari/605.1.15"
1-dc-phpapp-1  | 172.18.0.1 - - [03/Mar/2023:00:44:54 +0000] "GET /favicon.ico HTTP/1.1" 404 489 "http://localhost:8080/" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/16.3 Safari/605.1.15"
1-dc-phpapp-1  | 172.18.0.1 - - [03/Mar/2023:00:45:02 +0000] "GET / HTTP/1.1" 200 244 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/16.3 Safari/605.1.15"
¸


```

#### open browser 

```sh
http://localhost:8080

```