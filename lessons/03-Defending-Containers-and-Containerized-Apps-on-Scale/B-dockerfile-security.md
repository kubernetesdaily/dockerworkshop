---

title: "DockerFile Security Best Practices "
description: "DockerFile Security Best Practices "

---


# DockerFile Security Best Practices 

- Choose Minimal Base Images 

![](./images/minimal.gif)

```Dockerfile
FROM alpine
WORKDIR /app
COPY package.json /app
RUN npm install
CMD [“node”,“index.js”]

```


- Remove Cache Packages
![](./images/cache-package.jpeg)

```Dockerfile
FROM alpine
RUN apk add nginx && rm -rf /var/cache/apt/*
COPY index.html /var/www/html/
EXPOSE 80
CMD [“nginx”,“-g”,“daemon off;”]
```

```Dockerfile
FROM alpine
RUN apk add –no-cache nginx
COPY index.html /var/www/html/
EXPOSE 80
CMD [“nginx”,“-g”,“daemon off;”]
```

- avoid multilayers 

![](https://miro.medium.com/max/1000/0*FMAsrFS11TCJaQl5.gif)

```Dockerfile 
FROM alpine
RUN apk update
RUN apk add curl
RUN apk add nodejs
RUN apk add nginx-1.16.1-r6
RUN apk add nginx-mod-http-geoip2-1.16.1-r6
COPY index.html /var/www/html/
EXPOSE 80
CMD [“nginx”,“-g”,“daemon off;”]
```

```Dockerfile
FROM alpine
RUN apk update && apk add curl nginx nginx-mod-http-geoip2-1.16.1-r6 \
rm -rf /var/cache/apt/*
COPY index.html /var/www/html/
EXPOSE 80
CMD [“nginx”,“-g”,“daemon off;”]

```
- Don't ignore `.dockerignore`

![](https://miro.medium.com/max/800/0*FNJQYGBJTCJ6CwwI.gif)

```
node_modules
.env
secrets/
*pem
*.md
```

```Dockerfile
FROM node:10
WORKDIR /nodeapp
COPY package.json ./
RUN npm install
COPY . .
EXPOSE 8888
CMD [ “node”, “index.js” ]
```

- choose slim variant  


![](https://s3.amazonaws.com/media-p.slid.es/uploads/1936196/images/8841214/pasted-from-clipboard.png)

- cut the root

![](https://s3.amazonaws.com/media-p.slid.es/uploads/1936196/images/8841215/pasted-from-clipboard.png)

```Dockerfile
FROM node:10
WORKDIR /app
COPY package.json ./
RUN npm install
COPY . .
EXPOSE 8888
CMD [ “node”, “index.js” ]
```

```Dockerfile
FROM node:10
RUN user add -m nodeapp
USER nodeappuser
RUN whoami
WORKDIR /app
COPY package.json ./
RUN npm install
COPY . .
EXPOSE 8888
CMD [ “node”, “index.js” ]

```

## Remove Unwanted 

![](https://miro.medium.com/max/1060/0*5Kgip-LsvTRHXb7f.gif)


TAG Wisely



![](https://miro.medium.com/max/960/0*XchFJrkVCTwcC1Aj.gif)

So No to latest Tags

![](https://s3.amazonaws.com/media-p.slid.es/uploads/1936196/images/8841223/pasted-from-clipboard.png)

Public Private Registry 

![](https://miro.medium.com/max/960/0*cWv3QB9YHc_5a3D_.gif)

keep it single 
![](https://miro.medium.com/max/700/0*RJiU_CQIcwj5t-6I.gif)

## Avoid Hard Coding

![](https://miro.medium.com/max/1000/0*cg2kGD2eKdypyJzT.gif)

```
ARG VERSION
FROM node:$VERSION
WORKDIR /app
COPY package.json ./
RUN npm install
COPY . .
EXPOSE 8888
CMD [ “node”, “index.js” ]

```

```
docker build -t testimage –build-arg VERSION=10 .
docker build -t testimage –build-arg VERSION=9 .
```

adding metadata 

![](https://miro.medium.com/max/1374/0*Pz7-FhTCNRu7Qs9B.jpg)

```
FROM node:10
LABEL version=“1.0” maintainer=“Sangam Biradar <cXXXXXXo@gmail.com>”
WORKDIR /app
COPY package.json ./
RUN npm install
COPY . .
EXPOSE 8888
CMD [ “node”, “index.js” ]

```

Rego Policies 

- write custom policies 

```json

package play


suspicious_env_keys = [
    "passwd",
    "password",
    "secret",
    "key",
    "access",
    "api_key",
    "apikey",
    "token",
]

pkg_update_commands = [
    "apk upgrade",
    "apt-get upgrade",
    "dist-upgrade",
]

image_tag_list = [
    "latest",
    "LATEST",
]

# Looking for suspicious environemnt variables
deny[msg] {    
    input[i].Cmd == "env"
    val := input[i].Value
    contains(lower(val[_]), suspicious_env_keys[_])
    msg = sprintf("Suspicious ENV key found: %s", [val])
}

# Looking for latest docker image used
warn[msg] {
    input[i].Cmd == "from"
    val := split(input[i].Value[0], ":")
    count(val) == 1
    msg = sprintf("Do not use latest tag with image: %s", [val])
}

# Looking for latest docker image used
warn[msg] {
    input[i].Cmd == "from"
    val := split(input[i].Value[0], ":")
    contains(val[1], image_tag_list[_])
    msg = sprintf("Do not use latest tag with image: %s", [input[i].Value])
}

# Looking for apk upgrade command used in Dockerfile
deny[msg] {
    input[i].Cmd == "run"
    val := concat(" ", input[i].Value)
    contains(val, pkg_update_commands[_])
    msg = sprintf("Do not use upgrade commands: %s", [val])
}

# Looking for ADD command instead using COPY command
deny[msg] {
    input[i].Cmd == "add"
    val := concat(" ", input[i].Value)
    msg = sprintf("Use COPY instead of ADD: %s", [val])
}

# sudo usage
deny[msg] {
    input[i].Cmd == "run"
    val := concat(" ", input[i].Value)
    contains(lower(val), "sudo")
    msg = sprintf("Avoid using 'sudo' command: %s", [val])
}

# # No Healthcheck usage
# deny[msg] {
#     input[i].Cmd == "healthcheck"
#     msg := "no healthcheck"
# }

```
[Rego Playground](https://play.openpolicyagent.org/p/epcbtaBtSF)

