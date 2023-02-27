
## SecComp and AppArmor

Docker works with major Linux MAC technologies such as AppArmor and SELinux.

Depending on your Linux distribution, Docker applies a default AppArmor profile to all new containers. According to the Docker documentation, this default profile is “moderately protective while providing wide application compatibility”.

Docker also lets you start containers without a policy applied, as well as giving you the ability to customize policies to meet specific requirements. This is also very powerful, but can also be prohibitively complex.

## seccomp

Docker uses seccomp, in filter mode, to limit the syscalls a container can make to the host’s kernel.

As per the Docker security philosophy, all new containers get a default seccomp profile configured with sensible defaults. This is intended to provide moderate security without impacting application compatibility.

![](/img/seccomp.png)

As always, you can customize seccomp profiles, and you can pass a flag to Docker so that containers can be started without a seccomp profile.

As with many of the technologies already mentioned, seccomp is extremely powerful. However, the Linux syscall table is long, and configuring the appropriate seccomp policies can be prohibitively complex.

# Concluding Linux security technologies

Docker supports most of the important Linux security technologies and ships with sensible defaults that add security but aren’t too restrictive. The figure below shows how these technologies form multiple layers of potential security.

Some of these technologies can be complicated to customize as they require deep knowledge of how the Linux kernel works. Hopefully, they will get simpler to configure in the future, but for now, the default configurations that ship with Docker might be a good place to start.



## SecComp Exercise 1 


```
grep SECCOMP /boot/config-$(uname -r)
```
get the fitst test for seccomp in strict mode 

```seccomp_strict.c
#include <fcntl.h>
#include <stdio.h>
#include <unistd.h>
#include <string.h>
#include <linux/seccomp.h>
#include <sys/prctl.h>


int main(int argc, char **argv)
{
        int output = open("output.txt", O_WRONLY);
        const char *val = "test";

        printf("Calling prctl() to set seccomp strict mode...\n");
        prctl(PR_SET_SECCOMP, SECCOMP_MODE_STRICT);

        printf("Writing to an already open file...\n");
        write(output, val, strlen(val)+1);

        printf("Trying to open file for reading...\n");
        int input = open("output.txt", O_RDONLY);

        printf("You will not see this message--the process will be killed first\n");
}
```

```
sudo gcc seccomp_stric.c -o seccomp_strict 
./seccomp_strict
```

open()system call is not allowed by secccomp strict mode 

get the file ro test for seccomp in filter mode 

```
#include <seccomp.h>
#include <unistd.h>
#include <stdio.h>
#include <errno.h>

void main(void)
{
    /* initialize the libseccomp context */
    scmp_filter_ctx ctx = seccomp_init(SCMP_ACT_KILL);

    /* allow exiting */
    seccomp_rule_add(ctx, SCMP_ACT_ALLOW, SCMP_SYS(exit_group), 0);

    /* allow getting the current pid */
    seccomp_rule_add(ctx, SCMP_ACT_ALLOW, SCMP_SYS(getpid), 0);

    /* allow changing data segment size, as required by glibc */
    seccomp_rule_add(ctx, SCMP_ACT_ALLOW, SCMP_SYS(brk), 0);

    /* allow writing up to 512 bytes to fd 1 */
    seccomp_rule_add(ctx, SCMP_ACT_ALLOW, SCMP_SYS(write), 2,
        SCMP_A0(SCMP_CMP_EQ, 1),
        SCMP_A2(SCMP_CMP_LE, 512));

    /* if writing to any other fd, return -EBADF */
    seccomp_rule_add(ctx, SCMP_ACT_ERRNO(EBADF), SCMP_SYS(write), 1,
        SCMP_A0(SCMP_CMP_NE, 1));

    /* load and enforce the filters */
    seccomp_load(ctx);
    seccomp_release(ctx);

    printf("this process is %d\n", getpid());
}

```

we have first initiazed the seccomp in filter mode . previouly have used `prctl(PR_SET_SECCOMP, SECCOMP_MODE_STRICT);` to set the seccomp in strict mode . 

```
sudo gcc seccomp_bpf.c -o seccomp_bpf
```
install libseccomp-dev 

``` 
sudo apt-get install libseccomp-dev
```


```
sudo gcc seccomp_bpf.c -o seccomp_bpf -lseccomp
```

```
./seccomp_bpf
```

output 

```
initiating seccomp ...
add rule to allow exit_group
add rule to allow getpid
add rule to allow brk
add rule to allow write upto 512 bytes to fd 1
add rule to allow write to any other fd except 1
loading seccomp filter ...

```

inspecting the output one can look here that all the rules added to seccomp BPF folter with a process
ID since we have added the rule to allow getpid() system call . 



open seccomp_bpf.c and add the following code to the end of the file


```
#include <unistd.h>
#include <stdint.h>
#include <stdio.h>
#include <sys/prctl.h>
#include <sys/syscall.h>
#include <linux/seccomp.h>

/* "mov al,42; ret" aka "return 42" */
static const unsigned char code[] = "\xb0\x2a\xc3";

void main(void)
{
    int fd[2], ret;

    /* spawn child process, connected by a pipe */
    pipe(fd);
    if (fork() == 0) {
        close(fd[0]);

        /* enter mode 1 seccomp and execute untrusted bytecode */
        prctl(PR_SET_SECCOMP, SECCOMP_MODE_STRICT);
        ret = (*(uint8_t(*)())code)();

        /* send result over pipe, and exit */
        write(fd[1], &ret, sizeof(ret));
        syscall(SYS_exit, 0);
    } else {
        close(fd[1]);

        /* read the result from the pipe, and print it */
        read(fd[0], &ret, sizeof(ret));
        printf("untrusted bytecode returned %d\n", ret);
    }
}

```

```
$ suod gcc seccomp_bpf.c -o seccomp_bpf2 -lseccomp
$ ./seccomp_bpf2
initiating seccomp ...
add rule to allow exit_group
add rule to allow getpid
add rule to allow brk
add rule to allow write upto 512 bytes to fd 1
add rule to allow write to any other fd except 1
loading seccomp filter ...
this process is -9 
```


## seccomp in docker 

step 1. check the  SECCOMP is working and configured in docker daemon

``` 
docker info | grep seccomp
```

step 2. check the seccomp profile of the container

```
docker inspect --format='{{json .HostConfig.SecurityOpt}}' <container_name>
```

seccomp-profiles/deny.json

```
{
    "defaultAction": "SCMP_ACT_ERRNO",
    "architectures": [
        "SCMP_ARCH_X86_64",
        "SCMP_ARCH_X86",
        "SCMP_ARCH_X32"
    ],
    "syscalls" : [

    ]
}

```

a docker seccomp profile consists of 3 required entries viz. defaultAction, architectures and syscalls. the possible action of precedence are 

* SCMP_ACT_KILL
  - kill with am status of 0x80 + 31(SIGSYS) = 159
* SCMP_ACT_TRAP
  - send SIGSYS signal without executing the syscall
* SCMP_ACT_ERRNO
  - set erno withou executing the syscall
* SCMP_ACT_TRACE
   - invoke a ptrace to make a decision or set errno to Enosys 
* SCMP_ACT_ALLOW
  - allow the syscall to execute

```

docker run --security-opt seccomp=seccomp-profiles/deny.json -it ubuntu bash
```
using `--security-opt seccomp=seccomp-profiles/deny.json` we have set the seccomp profile to the container . 
notice here that since not even single system call is allwoed , the docker container is not able to run . 



create sc-custom.js 

```
{
    "defaultAction": "SCMP_ACT_ALLOW",
    "architectures": [
        "SCMP_ARCH_X86_64",
        "SCMP_ARCH_X86",
        "SCMP_ARCH_X32"
    ],
    "syscalls" : [
        {
            "names":"mkdir",
            "action": "SCMP_ACT_ALLOW",
            "args::[]
        
        },
        {
            "names": "chmod" ,
            "action": "SCMP_ACT_ERRNO",
            "args::[]
        }

    ]
}

```

before going to run this commands see what 
all system call actally take place while hitting mkdir command inside an alphine
container using strace 

```
docker run -rm -it --security-opt seccomp=unconfined alphine sh 
``` 

```
apk add strace
strace mkdir test
exit
```

we have installed strace utility inside the container and then run the mkdir command .the system call oberved include  `execve ` , 
`arch_prtctl` ` mprotect` `brk` `access` `openat` `mkdirat` `fstat` `close` `exit_group` . test directory is created inside the container . 


Now run another contaiet with same aphine image with new `sc-custom.json` seccomp profile . 

```
docker run -rm -it --security-opt seccomp=sc-custom.json alphine sh 
``` 

```
ls 
mkdir test
```
mkdir: can't create directory 'test': Operation not permitted

```
# chmod /etc/
```
clearly , the seccomp profile attavhed is blocking the mkdir and chmod system call . 
lets comfirm it with strace utility agin 


```
apk add strace
strace mkdir test
```

as expected the systm call got rejected with `Operation not permitted` error . since also we have added a deny condition for chmod system call . 

Run a docker container with the same seccomp file with chmpd over file 777 permission 

```
$ docker run -rm -it --security-opt seccomp=sc-custom.json alphine sh chmod 777 /etc/passwd
```
this os also denyed by the seccomp profile . 

for more play with `default.json` seccomp 
profile available on moby github repo 



## app armor

appArmor is a MAC (Mandatory Access Control) system that is used to restrict the access of a process to the system resources . its implements a task centered policy with task 
"profile" being created and loaded from user space 

tasks on the syste, that do not have profi;e defined for them run an uncofined state which  is equivalant to standared Linux DAC (Discretionary Access Control) . permissions .

appArmor works on file paths . it comes as default LSM for ubuntu and SUSE . 

1. let check our dpcler version and service status 

```

$ docker version
$ systemctl status docker 
```



2. check AppArmor in docker info 

```
docker info -f '{{json .SecurityOptions}}'
```

3. chheck AppArmor status . it might required sudo access 

```
$ apparmor_status
$ sudo apparmor_status
```
this will provide us the infromation about all the profile loaded and the mode of profiles with process 

the apparmor_status and aa-status can used interchangeably . just check if they are available with your system installation or not gernerally they comes in package call 
`apparmor-utils` .
```

$ which apparmor_status
$ which aa-status
```
and one can gain insight about the number of profile also 

```
$ sudp aa-status --help 
$ sudo aa-status --enabled [No error output means apparmor is enabled]
$ sudo aa-status --profiles [prints the no of loaded policies ]
$ sudo aa-status --enforce [prints the no of enforced policies ]

```

Install an AppArmor Profile generator tool called `bane`

```
# Export the sha256sum for verification.
$ export BANE_SHA256="e70b1d67333975eb705b08045de9558483daae05792a1ff28dcec28d4c164386"

# Download and check the sha256sum.
$ curl -fSL "https://github.com/genuinetools/bane/releases/download/v0.4.4/bane-freebsd-amd64" -o "/usr/local/bin/bane" \
	&& echo "${BANE_SHA256}  /usr/local/bin/bane" | sha256sum -c - \
	&& chmod a+x "/usr/local/bin/bane"

$ echo "bane installed!"

# Run it!
$ bane -h
```

get the sample TOML file for creation of AppArmor profile from bane Github 


```
$ sudo curl -o sample.toml https://raw.githubusercontent.com/genuinetools/bane/master/sample.toml
$ ls 
```
the parts of the sample.toml file are 

a. `name` key value pair is the name of the profile .

```
# name of the profile, we will auto prefix with `docker-`
# so the final profile name will be `docker-nginx-sample`
Name = "nginx-sample"

```

b . `Filesystem ` table with different arrays like ReadOnlyPaths , LogOnWritePaths , WritePaths , ReadPaths , NoAccessPaths , ReadOnlyPaths . 

```

[Filesystem]
# read only paths for the container
ReadOnlyPaths = [
	"/bin/**",
	"/boot/**",
	"/dev/**",
	"/etc/**",
	"/home/**",
	"/lib/**",
	"/lib64/**",
	"/media/**",
	"/mnt/**",
	"/opt/**",
	"/proc/**",
	"/root/**",
	"/sbin/**",
	"/srv/**",
	"/tmp/**",
	"/sys/**",
	"/usr/**",
]

# paths where you want to log on write
LogOnWritePaths = [
	"/**"
]

# paths where you can write
WritablePaths = [
	"/var/run/nginx.pid"
]

# allowed executable files for the container
AllowExec = [
	"/usr/sbin/nginx"
]

# denied executable files
DenyExec = [
	"/bin/dash",
	"/bin/sh",
	"/usr/bin/top"
]

```

C. Capabilties table allow array for allowing Linux capabilities . 

```
# allowed capabilities
[Capabilities]
Allow = [
	"chown",
	"dac_override",
	"setuid",
	"setgid",
	"net_bind_service"
]


```

D . Network table with Raw , Packet , Protocols array .

```

[Network]
# if you don't need to ping in a container, you can probably
# set Raw to false and deny network raw
Raw = false
Packet = false
Protocols = [
	"tcp",
	"udp",
	"icmp"
]

```

build the sample file with bane and check apparmor status if this profile gets enforced 

```
$ sudo bane sample.toml
$ sudo aa-status | grep docker 

```
notice that there was already loaded `docker-default` profile .


```
$ sudo ls /etc/apparmor.d/containers/
docker-ngnix-sample
```

```
sudo cat /etc/apparmor.d/containers/docker-nginx-sample
```

```
    #include <tunables/global>

    profile docker-nginx-sample flags=(attach_disconnected,mediate_deleted) {
      #include <abstractions/base>
      #include <abstractions/nameservice>
      #include <abstractions/openssl>

      /bin/** r,
      /boot/** r,
      /dev/** r,
      /etc/** r,
      /home/** r,
      /lib/** r,
      /lib64/** r,
      /media/** r,
      /mnt/** r,
      /opt/** r,
      /proc/** r,
      /root/** r,
      /sbin/** r,
      /srv/** r,
      /tmp/** r,
      /sys/** r,
      /usr/** r,
      /var/run/nginx.pid rw,
      /usr/sbin/nginx ix,
      deny /bin/dash ix,
      deny /bin/sh ix,
      deny /usr/bin/top ix,
      capability chown,
      capability dac_override,
      capability setuid,
      capability setgid,
      capability net_bind_service,
      network raw,
      network packet,
      deny network raw,
      deny network packet,
      deny network tcp,
      deny network udp,
      deny network icmp,
    }

    .....

```

Apply the above bane generated profile to the container before that lets 
analyze some commands that we can perfectly run within a container not attached to this profile 


```
$ docker run -it --rm --name without-aa -p 4444:80 nginx bash 
# sh
# dash
# bash 
# exit
# exist 
```
in this ngnix container we are able to run many variants of shell like bash sh and dash without any error 


now attach th profile and try to achive the same 

``` 
$ docker run -it --rm --name with-aa --security-opt="apparmor:docker-nginx-sample" -p 4444:80  nginx bash 
```
As expected, the attached AppArmor profile is not allowing us to spawn shells inside the container. This  is how an AppArmor profile can be attached to a Docker container using `--security-opt` and the different  executables and capabilities can be controlled.  Till now, we have seen that Docker uses many Linux technologies, such as Capabilities,  AppArmor and SecComp for defense. However, AppArmor can protect a Docker Host even  when the other lines of defense such as SecComp and Capabilities are not effective.  Remember that if you are not explicitly defining any AppArmor profile, the `default-docker`  AppArmor profile will get automatically attached. Until and unless `--security-opt  apparmor=unconfined` is not present during the container run command execution `default-docker`
 apparmor profile will be remain loaded .

 