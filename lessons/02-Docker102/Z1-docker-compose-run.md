---
title: " Docker Compose CLI - run Command "
description: " run service containers"
---


```

➜  example-voting-app git:(main) docker compose run db                  

PostgreSQL Database directory appears to contain a database; Skipping initialization

2023-03-04 19:16:58.496 UTC [1] LOG:  starting PostgreSQL 15.2 on aarch64-unknown-linux-musl, compiled by gcc (Alpine 12.2.1_git20220924-r4) 12.2.1 20220924, 64-bit
2023-03-04 19:16:58.496 UTC [1] LOG:  listening on IPv4 address "0.0.0.0", port 5432
2023-03-04 19:16:58.496 UTC [1] LOG:  listening on IPv6 address "::", port 5432
2023-03-04 19:16:58.498 UTC [1] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
2023-03-04 19:16:58.503 UTC [24] LOG:  database system was shut down at 2023-03-04 19:15:07 UTC
2023-03-04 19:16:58.510 UTC [1] LOG:  database system is ready to accept connections
^C2023-03-04 19:17:11.082 UTC [1] LOG:  received fast shutdown request
2023-03-04 19:17:11.088 UTC [1] LOG:  aborting any active transactions
2023-03-04 19:17:11.108 UTC [1] LOG:  background worker "logical replication launcher" (PID 27) exited with exit code 1
2023-03-04 19:17:11.108 UTC [22] LOG:  shutting down
2023-03-04 19:17:11.110 UTC [22] LOG:  checkpoint starting: shutdown immediate
2023-03-04 19:17:11.117 UTC [22] LOG:  checkpoint complete: wrote 3 buffers (0.0%); 0 WAL file(s) added, 0 removed, 0 recycled; write=0.003 s, sync=0.001 s, total=0.009 s; sync files=2, longest=0.001 s, average=0.001 s; distance=0 kB, estimate=0 kB
2023-03-04 19:17:11.127 UTC [1] LOG:  database system is shut down
➜  example-voting-app git:(main) docker compose run worker
[+] Running 2/2
 ⠿ Container example-voting-app-db-1     Created                                                                                                                                     0.0s
 ⠿ Container example-voting-app-redis-1  Created                                                                                                                                     0.0s
[+] Running 2/2
 ⠿ Container example-voting-app-db-1     Started                                                                                                                                     0.5s
 ⠿ Container example-voting-app-redis-1  Started                                                                                                                                     0.5s
Connected to db
Found redis at 172.20.0.3
Connecting to redis
^C%                                                                                                                                                                                       
➜  example-voting-app git:(main) 

```