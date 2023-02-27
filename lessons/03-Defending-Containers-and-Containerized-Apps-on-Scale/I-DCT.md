---

title: " Content Trust and Integrity checks "
description: "  "

---


Content Trust and Integrity checks

Docker Content Trust (DCT) makes it simple and easy to verify the integrity and the publisher of images that you download and run. This is especially important when pulling images over untrusted networks such as the internet.


```bash
docker trust key generate sangam
Generating key for sangam...
Enter passphrase for new nigel key with ID 1f78609: 
Repeat passphrase for new nigel key with ID 1f78609: 
Successfully generated and loaded private key.... public key available: /root/sangam.pub


```

```bash
$ docker trust signer add --key sangam.pub sangam sangam14/dct
Adding signer "nigel" to sangam14/dct...
Initializing signed repository for sangam/dct...
Enter passphrase for root key with ID aee3314: 
Enter passphrase for new repository key with ID 1a18dd1: 
Repeat passphrase for new repository key with ID 1a18dd1: 
Successfully initialized "sangam14/dct"
Successfully added signer: nigel to sangam14/dct
```

```bash
docker trust sign sangam14/dct:signed

docker trust inspect sangam14/dct:signed --pretty
```

# Enabling DCT 


```bash
$ export DOCKER_CONTENT_TRUST=1

```

# verify the image


```bash
docker image pull sangam14/dct:unsigned
No valid trust data for unsigned
```