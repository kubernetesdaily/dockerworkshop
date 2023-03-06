---
title: " Docker Compose CLI - Build Command "
description: " Docker Compose Build Command  "
---


### Docker Compose Build Command

```sh
docker compose build [OPTIONS] [SERVICE...]

```

If you change a service’s Dockerfile or the contents of its build directory, run docker compose build to rebuild it.

`--build-arg`		Set build-time variables for services.<br>
`--no-cache	`	Do not use cache when building the image
`--progress	auto`	Set type of progress output (auto, tty, plain, quiet)  .<br>
`--pull	`	Always attempt to pull a newer version of the image..<br>
`--push	`	Push service images..<br>
`--quiet , -q`		Don’t print anything to STDOUT.<br>
`--ssh	`	Set SSH authentications used when building .service images. (use ‘default’ for using your default SSH Agent)


#### create dockerfile with following content 


```dockerfile
# syntax=docker/dockerfile:1
FROM python:3.7-alpine
WORKDIR /code
ENV FLASK_APP=app.py
ENV FLASK_RUN_HOST=0.0.0.0
RUN apk add --no-cache gcc musl-dev linux-headers
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
EXPOSE 5000
COPY . .
CMD ["flask", "run"]

```

create app.py with following content 


```python
import time

import redis
from flask import Flask

app = Flask(__name__)
cache = redis.Redis(host='redis', port=6379)

def get_hit_count():
    retries = 5
    while True:
        try:
            return cache.incr('hits')
        except redis.exceptions.ConnectionError as exc:
            if retries == 0:
                raise exc
            retries -= 1
            time.sleep(0.5)

@app.route('/')
def hello():
    count = get_hit_count()
    return 'Hello World! I have been seen {} times.\n'.format(count)


```

#### create requirements.txt  with following content 


```txt
flask
redis

```

create docker-compose.yml with following content 

```yml
version: "3.9"
services:
  web:
    build: .
    ports:
      - "8000:5000"
  redis:
    image: "redis:alpine"
```

### docker compose build 

```sh

 7-DC-CLI git:(main) ✗ docker compose build 
[+] Building 27.1s (15/15) FINISHED                                                                                   
 => [internal] booting buildkit                                                                                  0.5s
 => => starting container buildx_buildkit_great_brahmagupta0                                                     0.5s
 => [internal] load build definition from Dockerfile                                                             0.0s
 => => transferring dockerfile: 319B                                                                             0.0s
 => [internal] load .dockerignore                                                                                0.0s
 => => transferring context: 2B                                                                                  0.0s
 => resolve image config for docker.io/docker/dockerfile:1                                                       3.8s
 => docker-image://docker.io/docker/dockerfile:1@sha256:39b85bbfa7536a5feceb7372a0817649ecb2724562a38360f4d6a77  3.1s
 => => resolve docker.io/docker/dockerfile:1@sha256:39b85bbfa7536a5feceb7372a0817649ecb2724562a38360f4d6a7782a4  0.0s
 => => sha256:9d0cd65540a143ce38aa0be7c5e9efeed30d3580d03667f107cd76354f2bee65 10.82MB / 10.82MB                 3.0s
 => => extracting sha256:9d0cd65540a143ce38aa0be7c5e9efeed30d3580d03667f107cd76354f2bee65                        0.1s
 => [internal] load metadata for docker.io/library/python:3.7-alpine                                             3.6s
 => [1/6] FROM docker.io/library/python:3.7-alpine@sha256:c9c2d6f97a00b211def3818830883495417e3b1fd34783ce6135c  3.9s
 => => resolve docker.io/library/python:3.7-alpine@sha256:c9c2d6f97a00b211def3818830883495417e3b1fd34783ce6135c  0.0s
 => => sha256:57a125a213d772ab0750422ad92f1cc7d1d97e0b09ad4ed766d9cd50db8e2b50 2.88MB / 2.88MB                   1.9s
 => => sha256:2bd7dfc9c660f2a9be2e537fd8f7f3c289ea1ba335c957472bd9a51630c9b449 230B / 230B                       0.6s
 => => sha256:6b1178b7c6752e0d2eb293ee14286ec1f75d0f868d9826bcaba24fe33affea99 11.01MB / 11.01MB                 3.0s
 => => sha256:cdba2f6867522ee9d2e8dcc9b1ad3ec5bafe75da976a3e613ff26cbc98738b48 624.86kB / 624.86kB               3.5s
 => => sha256:af6eaf76a39c2d3e7e0b8a0420486e3df33c4027d696c076a99a3d0ac09026af 3.26MB / 3.26MB                   0.6s
 => => extracting sha256:af6eaf76a39c2d3e7e0b8a0420486e3df33c4027d696c076a99a3d0ac09026af                        0.1s
 => => extracting sha256:cdba2f6867522ee9d2e8dcc9b1ad3ec5bafe75da976a3e613ff26cbc98738b48                        0.1s
 => => extracting sha256:6b1178b7c6752e0d2eb293ee14286ec1f75d0f868d9826bcaba24fe33affea99                        0.2s
 => => extracting sha256:2bd7dfc9c660f2a9be2e537fd8f7f3c289ea1ba335c957472bd9a51630c9b449                        0.0s
 => => extracting sha256:57a125a213d772ab0750422ad92f1cc7d1d97e0b09ad4ed766d9cd50db8e2b50                        0.1s
 => [internal] load build context                                                                                0.0s
 => => transferring context: 1.08kB                                                                              0.0s
 => [2/6] WORKDIR /code                                                                                          0.1s
 => [3/6] RUN apk add --no-cache gcc musl-dev linux-headers                                                      3.1s
 => [4/6] COPY requirements.txt requirements.txt                                                                 0.0s
 => [5/6] RUN pip install -r requirements.txt                                                                    3.1s
 => [6/6] COPY . .                                                                                               0.0s
 => exporting to docker image format                                                                             5.6s
 => => exporting layers                                                                                          3.4s
 => => exporting manifest sha256:198e97700c3c0f4a28b481ceacda99d7dacf2c8f42c86f7f4ffd6e5094098322                0.0s
 => => exporting config sha256:dba54b65e3daa61d0ce759dc20145718068c33daf2eaf908c03e03934024df88                  0.0s
 => => sending tarball                                                                                           2.2s
 => importing to docker                                                                                          1.4s
➜  7-DC-CLI git:(main) ✗ 


```