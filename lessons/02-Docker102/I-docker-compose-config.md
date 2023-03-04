---
title: " Docker Compose CLI - Config Command "
description: " Parse, resolve and render compose file in canonical format "
---


---
title: " Docker Compose CLI - Build Command "
description: " Docker Compose Build Command  "
---


### Docker Compose Build Command

```
docker compose build [OPTIONS] [SERVICE...]

```

If you change a service’s Dockerfile or the contents of its build directory, run docker compose build to rebuild it.

--build-arg		Set build-time variables for services.
--no-cache		Do not use cache when building the image
--progress	auto	Set type of progress output (auto, tty, plain, quiet)
--pull		Always attempt to pull a newer version of the image.
--push		Push service images.
--quiet , -q		Don’t print anything to STDOUT
--ssh		Set SSH authentications used when building service images. (use ‘default’ for using your default SSH Agent)


create dockerfile with following content 


```
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


```
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

create requirements.txt fil with following content 


```
flask
redis

```

create docker-compose.yml with following content 

```
version: "3.9"
services:
  web:
    build: .
    ports:
      - "8000:5000"
  redis:
    image: "redis:alpine"
```

### docker compose config 

```
7-DC-CLI git:(main) ✗ docker compose config 
name: 7-dc-cli
services:
  redis:
    image: redis:alpine
    networks:
      default: null
  web:
    build:
      context: /Users/sangambiradar/Documents/GitHub/dockerworkshop/workshop/Docker102/Docker-Compose/7-DC-CLI
      dockerfile: Dockerfile
    networks:
      default: null
    ports:
    - mode: ingress
      target: 5000
      published: "8000"
      protocol: tcp
networks:
  default:
    name: 7-dc-cli_default
```