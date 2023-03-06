---
title: "Build a Base Image from Scratch"
description: "image without any base"
---


#### write simple c program 

```c
#include<stdio.h>

int main()
{
printf("dockerworkshop");
}
```

#### Compile C program 

```sh
gcc -o hello hello.c 

✗ ./hello 
dockerworkshop%     

```

#### create dockerfile with following content : 


```dockerfile
FROM scratch
ADD hello /
CMD ["/hello"]
```

### Build Dockerfile without any base image 

```sh
Dockerfile git:(main) ✗ docker build -t sangam14/hello-scratch -f dockerfile.hello .
[+] Building 0.1s (5/5) FINISHED                                                                                                                                                                  
 => [internal] load build definition from dockerfile.hello                                                                                                                                   0.0s
 => => transferring dockerfile: 87B                                                                                                                                                          0.0s
 => [internal] load .dockerignore                                                                                                                                                            0.0s
 => => transferring context: 2B                                                                                                                                                              0.0s
 => [internal] load build context                                                                                                                                                            0.0s
 => => transferring context: 33.47kB                                                                                                                                                         0.0s
 => [1/1] ADD hello /                                                                                                                                                                        0.0s
 => exporting to image                                                                                                                                                                       0.0s
 => => exporting layers                                                                                                                                                                      0.0s
 => => writing image sha256:769934a6858c0910a3682e966da6c8d9c15b0324307b092eb77258a9a08879ce                                                                                                 0.0s
 => => naming to docker.io/sangam14/hello-scratch       

 ```

#### run docker image 

```sh
 docker run sangam14/hello-scratch 
 dockerworkshop

```





