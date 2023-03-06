---
title: " Docker Compose with Volume Mount"
description: " docker-compose.yml with volume mounting for instant updates"
---

#### Lets build docker compose file 

```yml
version: '3'
services:
  phpapp:
    image: php:7.2-apache
    ports:
      - "8080:80"
    volumes:
      - "./:/var/www/html"
```

above when we do docker compose up it will start apache with php 7.2 
it will mount corrent directory to `/var/www/html`
also it mount port 80 of the container to port 8080 on the host 


#### create index.php with following content  

```php
<?php

echo "hello world \n\n";
```

#### run with docker compose 

```sh
2-DC-Vol-Mount git:(main) ✗ docker compose up
[+] Running 15/15
 ⠿ phpapp Pulled                                                                                    13.2s
   ⠿ c9648d7fcbb6 Pull complete                                                                      3.1s
   ⠿ f88cecc04e76 Pull complete                                                                      3.1s
   ⠿ 30eb7a300f13 Pull complete                                                                      5.1s
   ⠿ c17a0a78e91d Pull complete                                                                      5.1s
   ⠿ 8d436d7bb026 Pull complete                                                                      5.6s
   ⠿ 5543a36f8eed Pull complete                                                                      5.7s
   ⠿ 28135fd83ed1 Pull complete                                                                      5.7s
   ⠿ 485201b000c7 Pull complete                                                                      5.8s
   ⠿ 9e92e56de9f5 Pull complete                                                                      5.9s
   ⠿ 709f68bc1d50 Pull complete                                                                      8.3s
   ⠿ c5ff78edaefc Pull complete                                                                      8.3s
   ⠿ 119f7607f913 Pull complete                                                                      8.4s
   ⠿ 1c04bb0b5fbe Pull complete                                                                      8.4s
   ⠿ 9639d8c8cc76 Pull complete                                                                      8.4s
[+] Running 2/1
 ⠿ Network 2-dc-vol-mount_default     Created                                                        0.1s
 ⠿ Container 2-dc-vol-mount-phpapp-1  Created                                                        0.1s
Attaching to 2-dc-vol-mount-phpapp-1
```

#### open localhost 8080 

http://localhost:8080 
