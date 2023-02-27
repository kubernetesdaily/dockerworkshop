---

title: " Static Analysis of container images/library for container "
description: " Packetstreamer "

---

# Packetstreamer 

SBOM and vulnerability scanner for container images


https://github.com/deepfence/package-scanner


Download binary 

- package-scanner_Darwin_arm64.tar.gz
- package-scanner_Darwin_x86_64.tar.gz
- package-scanner_Linux_arm64.tar.gz
- package-scanner_Linux_x86_64.tar.gz
- package-scanner_darwin_amd64_v1
- package-scanner_darwin_arm64
- package-scanner_linux_amd64_v1
- package-scanner_linux_arm64


ClI usage

scan a docker image for vulnerabilities 

``` bash
./package-scannner -source nginx:latest
```

Scan a docker images , filter for critical vulnerabilities 

```bash
./package-scanner -source nginx:latest -severity critical
```

Scan a docker images with CVEs 




```bash
./package-scanner -source nginx:latest
INFO[2023-01-06T15:10:15+05:30] autodetect.go:91 trying to connect to endpoint 'unix:///var/run/docker.sock' with timeout '10s' 
INFO[2023-01-06T15:10:15+05:30] autodetect.go:116 connected successfully using endpoint: unix:///var/run/docker.sock 
INFO[2023-01-06T15:10:15+05:30] autodetect.go:91 trying to connect to endpoint 'unix:///run/containerd/containerd.sock' with timeout '10s' 
WARN[2023-01-06T15:10:25+05:30] autodetect.go:124 could not connect to endpoint 'unix:///run/containerd/containerd.sock': context deadline exceeded 
INFO[2023-01-06T15:10:25+05:30] autodetect.go:91 trying to connect to endpoint 'unix:///run/k3s/containerd/containerd.sock' with timeout '10s' 
WARN[2023-01-06T15:10:35+05:30] autodetect.go:124 could not connect to endpoint 'unix:///run/k3s/containerd/containerd.sock': context deadline exceeded 
INFO[2023-01-06T15:10:35+05:30] autodetect.go:91 trying to connect to endpoint 'unix:///var/run/crio/crio.sock' with timeout '10s' 
WARN[2023-01-06T15:10:35+05:30] autodetect.go:146 could not connect to endpoint 'unix:///var/run/crio/crio.sock': dial unix /var/run/crio/crio.sock: connect: no such file or directory 
INFO[2023-01-06T15:10:35+05:30] autodetect.go:184 container runtime detected: docker           
INFO[2023-01-06T15:10:35+05:30] run-once.go:57 generating sbom for nginx:latest ...         
INFO[2023-01-06T15:10:49+05:30] run-once.go:82 scanning sbom for vulnerabilities ...        
summary:
 total=142 critical=8 high=42 medium=79 low=13
+------------------+-----------------+-------------------------------------+----------------------------------------------------+
|      CVE ID      |    SEVERITY     |               PACKAGE               |                    DESCRIPTION                     |
+------------------+-----------------+-------------------------------------+----------------------------------------------------+
| CVE-2019-1010022 | critical        | libc-bin:2.31-13+deb11u5            | ** DISPUTED ** GNU Libc                            |
|                  |                 |                                     | current is affected by:                            |
|                  |                 |                                     | Mitigation bypass. The impact                      |
|                  |                 |                                     | is: Attacker may bypass                            |
|                  |                 |                                     | stack guard protection. The                        |
|                  |                 |                                     | component is: nptl. The attack                     |
|                  |                 |                                     | vector is: Exploit stack                           |
|                  |                 |                                     | buffer overflow vulnerability                      |
|                  |                 |                                     | and use this bypass                                |
|                  |                 |                                     | vulnerability to bypass stack                      |
|                  |                 |                                     | guard. NOTE: Upstream comments                     |
|                  |                 |                                     | indicate "this is being                            |
|                  |                 |                                     | treated as a non-security bug                      |
|                  |                 |                                     | and no real threat."                               |
| CVE-2019-1010022 | critical        | libc6:2.31-13+deb11u5               | ** DISPUTED ** GNU Libc                            |
|                  |                 |                                     | current is affected by:                            |
|                  |                 |                                     | Mitigation bypass. The impact                      |
|                  |                 |                                     | is: Attacker may bypass                            |
|                  |                 |                                     | stack guard protection. The                        |
|                  |                 |                                     | component is: nptl. The attack                     |
|                  |                 |                                     | vector is: Exploit stack                           |
|                  |                 |                                     | buffer overflow vulnerability                      |
|                  |                 |                                     | and use this bypass                                |
|                  |                 |                                     | vulnerability to bypass stack                      |
|                  |                 |                                     | guard. NOTE: Upstream comments                     |
|                  |                 |                                     | indicate "this is being                            |
|                  |                 |                                     | treated as a non-security bug                      |
|                  |                 |                                     | and no real threat."                               |
| CVE-2017-9117    | critical        | libtiff5:4.2.0-1+deb11u1            | In LibTIFF 4.0.7, the                              |
|                  |                 |                                     | program processes BMP images                       |
|                  |                 |                                     | without verifying that                             |
|                  |                 |                                     | biWidth and biHeight in the                        |
|                  |                 |                                     | bitmap-information header                          |
|                  |                 |                                     | match the actual input,                            |
|                  |                 |                                     | leading to a heap-based buffer                     |
|                  |                 |                                     | over-read in bmp2tiff.                             |
| CVE-2019-8457    | critical        | libdb5.3:5.3.28+dfsg1-0.8           | SQLite3 from 3.6.0 to and                          |
|                  |                 |                                     | including 3.27.2 is vulnerable                     |
|                  |                 |                                     | to heap out-of-bound read in                       |
|                  |                 |                                     | the rtreenode() function when                      |
|                  |                 |                                     | handling invalid rtree tables.                     |
| CVE-2005-2541    | critical        | tar:1.34+dfsg-1                     | Tar 1.15.1 does not properly                       |
|                  |                 |                                     | warn the user when extracting                      |
|                  |                 |                                     | setuid or setgid files,                            |
|                  |                 |                                     | which may allow local users                        |
|                  |                 |                                     | or remote attackers to gain                        |
|                  |                 |                                     | privileges.                                        |
| CVE-2022-3970    | critical        | libtiff5:4.2.0-1+deb11u1            | A vulnerability was found in LibTIFF.              |
|                  |                 |                                     | It has been classified as critical. This           |
|                  |                 |                                     | affects the function TIFFReadRGBATileExt           |
|                  |                 |                                     | of the file libtiff/tif_getimage.c.                |
|                  |                 |                                     | The manipulation leads to integer                  |
|                  |                 |                                     | overflow. It is possible to initiate               |
|                  |                 |                                     | the attack remotely. The exploit has               |
|                  |                 |                                     | been disclosed to the public and may               |
|                  |                 |                                     | be used. The name of the patch is                  |
|                  |                 |                                     | 227500897dfb07fb7d27f7aa570050e62617e3be.          |
|                  |                 |                                     | It is recommended to apply a patch to fix          |
|                  |                 |                                     | this issue. The identifier VDB-213549 was          |
|                  |                 |                                     | assigned to this vulnerability.                    |
| CVE-2022-32221   | critical        | libcurl4:7.74.0-1.3+deb11u3         | When doing HTTP(S) transfers,                      |
|                  |                 |                                     | libcurl might erroneously                          |
|                  |                 |                                     | use the read callback                              |
|                  |                 |                                     | (`CURLOPT_READFUNCTION`) to                        |
|                  |                 |                                     | ask for data to send, even                         |
|                  |                 |                                     | when the `CURLOPT_POSTFIELDS`                      |
|                  |                 |                                     | option has been set, if the                        |
|                  |                 |                                     | same handle previously was                         |
|                  |                 |                                     | used to issue a `PUT` request                      |
|                  |                 |                                     | which used that callback.                          |
|                  |                 |                                     | This flaw may surprise the                         |
|                  |                 |                                     | application and cause it to                        |
|                  |                 |                                     | misbehave and either send off                      |
|                  |                 |                                     | the wrong data or use memory                       |
|                  |                 |                                     | after free or similar in the                       |
|                  |                 |                                     | subsequent `POST` request. The                     |
|                  |                 |                                     | problem exists in the logic                        |
|                  |                 |                                     | for a reused handle when it is                     |
|                  |                 |                                     | changed from a PUT to a POST.                      |
| CVE-2022-32221   | critical        | curl:7.74.0-1.3+deb11u3             | When doing HTTP(S) transfers,                      |
|                  |                 |                                     | libcurl might erroneously                          |
|                  |                 |                                     | use the read callback                              |
|                  |                 |                                     | (`CURLOPT_READFUNCTION`) to                        |
|                  |                 |                                     | ask for data to send, even                         |
|                  |                 |                                     | when the `CURLOPT_POSTFIELDS`                      |
|                  |                 |                                     | option has been set, if the                        |
|                  |                 |                                     | same handle previously was                         |
|                  |                 |                                     | used to issue a `PUT` request                      |
|                  |                 |                                     | which used that callback.                          |
|                  |                 |                                     | This flaw may surprise the                         |
|                  |                 |                                     | application and cause it to                        |
|                  |                 |                                     | misbehave and either send off                      |
|                  |                 |                                     | the wrong data or use memory                       |
|                  |                 |                                     | after free or similar in the                       |
|                  |                 |                                     | subsequent `POST` request. The                     |
|                  |                 |                                     | problem exists in the logic                        |
|                  |                 |                                     | for a reused handle when it is                     |
|                  |                 |                                     | changed from a PUT to a POST.                      |
| CVE-2021-33560   | high            | libgcrypt20:1.8.7-6                 | Libgcrypt before 1.8.8 and                         |
|                  |                 |                                     | 1.9.x before 1.9.3 mishandles                      |
|                  |                 |                                     | ElGamal encryption because                         |
|                  |                 |                                     | it lacks exponent blinding                         |
|                  |                 |                                     | to address a side-channel                          |
|                  |                 |                                     | attack against mpi_powm,                           |
|                  |                 |                                     | and the window size is not                         |
|                  |                 |                                     | chosen appropriately. This,                        |
|                  |                 |                                     | for example, affects use of                        |
|                  |                 |                                     | ElGamal in OpenPGP.                                |
| CVE-2019-20838   | high            | libpcre3:2:8.39-13                  | libpcre in PCRE before 8.43                        |
|                  |                 |                                     | allows a subject buffer                            |
|                  |                 |                                     | over-read in JIT when UTF                          |
|                  |                 |                                     | is disabled, and \X or \R                          |
|                  |                 |                                     | has more than one fixed                            |
|                  |                 |                                     | quantifier, a related issue to                     |
|                  |                 |                                     | CVE-2019-20454.                                    |
| CVE-2022-29458   | high            | ncurses-base:6.2+20201114-2         | ncurses 6.3 before patch                           |
|                  |                 |                                     | 20220416 has an out-of-bounds                      |
|                  |                 |                                     | read and segmentation                              |
|                  |                 |                                     | violation in convert_strings                       |
|                  |                 |                                     | in tinfo/read_entry.c in the                       |





```

## docker image standlone scanner

```bash
docker run --rm -v /var/run/docker.sock:/var/run/docker.sock sangam14/package-scanner:latest -source nginx:latest
```
