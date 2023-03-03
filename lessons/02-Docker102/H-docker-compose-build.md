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
FROM php:apache-buster
COPY index.php /var/www/html

```

