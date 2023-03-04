---
title: " Docker Compose CLI - Kill Command "
description: " Force stop service containers. "
---



#### Forces running containers to stop by sending a SIGKILL signal. Optionally the signal can be passed :

```
 example-voting-app git:(main) docker-compose kill -s SIGINT
[+] Running 4/0
 ⠿ Container example-voting-app-redis-1   Killed                                                                                                                                     0.0s
 ⠿ Container example-voting-app-worker-1  Killed                                                                                                                                     0.0s
 ⠿ Container example-voting-app-db-1      Killed                                                                                                                                     0.0s
 ⠿ Container example-voting-app-result-1  Killed                                                                                                                                     0.0s
```