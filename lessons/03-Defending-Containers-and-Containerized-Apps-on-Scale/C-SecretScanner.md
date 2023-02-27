---

title: " SecretScanner - Finding secrets and passwords in container images and file systems "
description: " SecretScanner "

---

Published on [CloudNativeFolks](https://blog.cloudnativefolks.org/finding-leaked-secrets-in-your-container-images-and-file-system-with-secretscanner)

One of most common mistake is leaking Secrets via docker images or file system in this blogpost we will use [Deepfence's](https://deepfence.io) Open Source Project [SecretScanner](https://github.com/deepfence/SecretScanner)

few days back I found this interesting tweet discussion from cybersecurity community members !

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Find secrets and passwords in container images and file systems <a href="https://t.co/ccejD09KR6">https://t.co/ccejD09KR6</a></p>&mdash; Nicolas Krassas (@Dinosn) <a href="https://twitter.com/Dinosn/status/1571745912086736909?ref_src=twsrc%5Etfw">September 19, 2022</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

here you see article around Uber security incidence  

%[https://www.uber.com/newsroom/security-update/] 

lets understand why secret scanner help you securing your container images and file system

If you see DockerHub most of images and public and thousands of secrets leaked over the year

If you see of of example from past supply chain attack one of docker image Codecov contain git credentials that allow attacker to gain access to price Codecov's private git repositories and committed backdoors by attacker to there product and later that effected to 22000 Codecov's users

## lets write `insecure.DockerFile`

```plaintext
FROM python:3.9-slim
ENV DATABASE_PASSWORD "SuperSecret"
```

## Don't Store Secrets in Images

Secrets are sensitive pieces of information such as passwords, database credentials, SSH keys, tokens, and TLS certificates, to name a few. These should not be baked into your images without being encrypted since unauthorized users who gain access to the image can merely examine the layers to extract the secrets.

Instead, they should be injected via:

* Environment variables (at run-time)
    
* Build-time arguments (at build-time)
    
* An orchestration tool like Docker Swarm (via Docker secrets) or Kubernetes (via Kubernetes secrets)
    
* Also, you can help prevent leaking secrets by adding common secret files and folders to your .dockerignore file:
    

```plaintext
**/.env
**/.aws
**/.ssh
```

be explicit about what files are getting copied over to the image rather than copying all files recursively:

```plaintext
# BAD
COPY . .

# GOOD
copy ./app.py .
```

## Environment Variables

You can pass secrets via environment variables, but they will be visible in all child processes, linked containers, and logs, as well as via docker inspect. It's also difficult to update them.

```plaintext
$ docker run --detach --env "DATABASE_PASSWORD=SuperSecret" python:3.9-slim

d92cf5cf870eb0fdbf03c666e7fcf18f9664314b79ad58bc7618ea3445e39239


$ docker inspect --format='{{range .Config.Env}}{{println .}}{{end}}' d92cf5cf870eb0fdbf03c666e7fcf18f9664314b79ad58bc7618ea3445e39239

DATABASE_PASSWORD=SuperSecret
PATH=/usr/local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
LANG=C.UTF-8
GPG_KEY=E3FF2839C048B25C084DEBE9B26995E310250568
PYTHON_VERSION=3.9.7
PYTHON_PIP_VERSION=21.2.4
PYTHON_SETUPTOOLS_VERSION=57.5.0
PYTHON_GET_PIP_URL=https://github.com/pypa/get-pip/raw/c20b0cfd643cd4a19246ccf204e2997af70f6b21/public/get-pip.py
PYTHON_GET_PIP_SHA256=fa6f3fb93cce234cd4e8dd2beb54a51ab9c247653b52855a48dd44e6b21ff28b
```

## Build-time Arguments

```plaintext

docker build --build-arg "DATABASE_PASSWORD=SuperSecret" .
```

rather then build time arguments you can use Multi-stage build so `docker history` will be ignored . The multi-stage build only retains the history for the final image. Keep in mind that you can use this functionality for permanent secrets that you need for your application, like a database credential.

You can also use the --secret option in Docker build to pass secrets to Docker images that do not get stored in the images.

refer create secret doc \[https://docs.docker.com/engine/reference/commandline/secret\_create/ \] \`\`\`

# "docker" &gt; secrets.txt

FROM alpine

# shows secret from default secret location:

RUN --mount=type=secret,id=mysecret cat /run/secrets/mysecret

```plaintext
This will mount the secret from the secrets.txt file.
```

docker build --no-cache --progress=plain --secret id=mysecret,src=secrets.txt .

# output

... #4 \[1/2\] FROM docker.io/library/alpine #4 sha256:665ba8b2cdc0cb0200e2a42a6b3c0f8f684089f4cd1b81494fbb9805879120f7 #4 CACHED

#5 \[2/2\] RUN --mount=type=secret,id=mysecret cat /run/secrets/mysecret #5 sha256:75601a522ebe80ada66dedd9dd86772ca932d30d7e1b11bba94c04aa55c237de #5 0.635 docker #5 DONE 0.7s

# exporting to image

```plaintext
lets see the leaked secret
```

```❯ docker history 49574a19241c IMAGE CREATED CREATED BY SIZE COMMENT 49574a19241c 5 minutes ago CMD \["/bin/sh"\] 0B buildkit.dockerfile.v0 5 minutes ago RUN /bin/sh -c cat /run/secrets/mysecret # b… 0B buildkit.dockerfile.v0 4 weeks ago /bin/sh -c #(nop) CMD \["/bin/sh"\] 0B 4 weeks ago /bin/sh -c #(nop) ADD file:aad4290d27580cc1a… 5.6MB ```

but most modern way to scan docker images and file system you can use open source tool secretscanner that make developer life easy to detect leaked secretes

pull latest secretscanner image

```plaintext

docker pull deepfenceio/deepfence_secret_scanner:latest
```

 Pull docker image that you want to scan

```plaintext
docker pull node:8.11
```

 Hund Secrets

```plaintext
docker run -it --rm --name=deepfence-secretscanner -v $(pwd):/home/deepfence/output -v /var/run/docker.sock:/var/run/docker.sock deepfenceio/deepfence_secret_scanner:latest -image-name node:8.11
```

output

```plaintext
Scanning image /tmp/Deepfence/SecretScanning/df_node811/save-output.tar for secrets...
    {
      "Image Layer ID": "23d81b1ef111d5b6cec4559da8d7ad53f9ce5314134c9a232ecbb050b0269f76",
      "Matched Rule ID": 85,
      "Matched Rule Name": "Potential Linux passwd file",
      "Matched Part": "path",
      "String to Match": "",
      "Signature to Match": "etc/passwd$",
      "Severity": "medium",
      "Severity Score": 5.00,
      "Starting Index of Match in Original Content": 0,
      "Relative Starting Index of Match in Displayed Substring": 0,
      "Relative Ending Index of Match in Displayed Substring": 10,
      "Full File Name": "etc/passwd",
      "Matched Contents": "etc/passwd"
    },
    {
      "Image Layer ID": "23d81b1ef111d5b6cec4559da8d7ad53f9ce5314134c9a232ecbb050b0269f76",
      "Matched Rule ID": 69,
      "Matched Rule Name": "Shell profile configuration file",
      "Matched Part": "filename",
      "String to Match": "",
      "Signature to Match": "^\\.?(bash_|zsh_)?profile$",
      "Severity": "medium",
      "Severity Score": 5.00,
      "Starting Index of Match in Original Content": 0,
      "Relative Starting Index of Match in Displayed Substring": 0,
      "Relative Ending Index of Match in Displayed Substring": 7,
      "Full File Name": "etc/profile",
      "Matched Contents": "profile"
    },
    {
      "Image Layer ID": "23d81b1ef111d5b6cec4559da8d7ad53f9ce5314134c9a232ecbb050b0269f76",
      "Matched Rule ID": 84,
      "Matched Rule Name": "Potential Linux shadow file",
      "Matched Part": "path",
      "String to Match": "",
      "Signature to Match": "etc/shadow$",
      "Severity": "medium",
      "Severity Score": 5.00,
      "Starting Index of Match in Original Content": 0,
      "Relative Starting Index of Match in Displayed Substring": 0,
      "Relative Ending Index of Match in Displayed Substring": 10,
      "Full File Name": "etc/shadow",
      "Matched Contents": "etc/shadow"
    },
    {
      "Image Layer ID": "23d81b1ef111d5b6cec4559da8d7ad53f9ce5314134c9a232ecbb050b0269f76",
      "Matched Rule ID": 68,
      "Matched Rule Name": "Shell configuration file",
      "Matched Part": "filename",
      "String to Match": "",
      "Signature to Match": "^\\.?(bash|zsh|csh)rc$",
      "Severity": "medium",
      "Severity Score": 5.00,
      "Starting Index of Match in Original Content": 0,
      "Relative Starting Index of Match in Displayed Substring": 0,
      "Relative Ending Index of Match in Displayed Substring": 7,
      "Full File Name": "etc/skel/.bashrc",
      "Matched Contents": ".bashrc"
    },
    {
      "Image Layer ID": "23d81b1ef111d5b6cec4559da8d7ad53f9ce5314134c9a232ecbb050b0269f76",
      "Matched Rule ID": 69,
      "Matched Rule Name": "Shell profile configuration file",
      "Matched Part": "filename",
      "String to Match": "",
      "Signature to Match": "^\\.?(bash_|zsh_)?profile$",
      "Severity": "medium",
      "Severity Score": 5.00,
      "Starting Index of Match in Original Content": 0,
      "Relative Starting Index of Match in Displayed Substring": 0,
      "Relative Ending Index of Match in Displayed Substring": 8,
      "Full File Name": "etc/skel/.profile",
      "Matched Contents": ".profile"
    },
    {
      "Image Layer ID": "23d81b1ef111d5b6cec4559da8d7ad53f9ce5314134c9a232ecbb050b0269f76",
      "Matched Rule ID": 68,
      "Matched Rule Name": "Shell configuration file",
      "Matched Part": "filename",
      "String to Match": "",
      "Signature to Match": "^\\.?(bash|zsh|csh)rc$",
      "Severity": "medium",
      "Severity Score": 5.00,
      "Starting Index of Match in Original Content": 0,
      "Relative Starting Index of Match in Displayed Substring": 0,
      "Relative Ending Index of Match in Displayed Substring": 7,
      "Full File Name": "root/.bashrc",
      "Matched Contents": ".bashrc"
    },
    {
      "Image Layer ID": "23d81b1ef111d5b6cec4559da8d7ad53f9ce5314134c9a232ecbb050b0269f76",
      "Matched Rule ID": 69,
      "Matched Rule Name": "Shell profile configuration file",
      "Matched Part": "filename",
      "String to Match": "",
      "Signature to Match": "^\\.?(bash_|zsh_)?profile$",
      "Severity": "medium",
      "Severity Score": 5.00,
      "Starting Index of Match in Original Content": 0,
      "Relative Starting Index of Match in Displayed Substring": 0,
      "Relative Ending Index of Match in Displayed Substring": 8,
      "Full File Name": "root/.profile",
      "Matched Contents": ".profile"
    },
    {
      "Image Layer ID": "23d81b1ef111d5b6cec4559da8d7ad53f9ce5314134c9a232ecbb050b0269f76",
      "Matched Rule ID": 88,
      "Matched Rule Name": "Environment configuration file",
      "Matched Part": "filename",
      "String to Match": "",
      "Signature to Match": "^\\.?env$",
      "Severity": "medium",
      "Severity Score": 5.00,
      "Starting Index of Match in Original Content": 0,
      "Relative Starting Index of Match in Displayed Substring": 0,
      "Relative Ending Index of Match in Displayed Substring": 3,
      "Full File Name": "usr/bin/env",
      "Matched Contents": "env"
    },
    {
      "Image Layer ID": "23d81b1ef111d5b6cec4559da8d7ad53f9ce5314134c9a232ecbb050b0269f76",
      "Matched Rule ID": 69,
      "Matched Rule Name": "Shell profile configuration file",
      "Matched Part": "filename",
      "String to Match": "",
      "Signature to Match": "^\\.?(bash_|zsh_)?profile$",
      "Severity": "medium",
      "Severity Score": 5.00,
      "Starting Index of Match in Original Content": 0,
      "Relative Starting Index of Match in Displayed Substring": 0,
      "Relative Ending Index of Match in Displayed Substring": 7,
      "Full File Name": "usr/share/base-files/profile",
      "Matched Contents": "profile"
    },
    {
      "Image Layer ID": "23d81b1ef111d5b6cec4559da8d7ad53f9ce5314134c9a232ecbb050b0269f76",
      "Matched Rule ID": 69,
      "Matched Rule Name": "Shell profile configuration file",
      "Matched Part": "filename",
      "String to Match": "",
      "Signature to Match": "^\\.?(bash_|zsh_)?profile$",
      "Severity": "medium",
      "Severity Score": 5.00,
      "Starting Index of Match in Original Content": 0,
      "Relative Starting Index of Match in Displayed Substring": 0,
      "Relative Ending Index of Match in Displayed Substring": 7,
      "Full File Name": "usr/share/doc/adduser/examples/adduser.local.conf.examples/profile",
      "Matched Contents": "profile"
    },
    {
      "Image Layer ID": "45475acd15f3bb8a3b04367eda1a2553d6b18d95723eb51737bc9d9a24227336",
      "Matched Rule ID": 0,
      "Matched Rule Name": "Potential cryptographic private key",
      "Matched Part": "extension",
      "String to Match": ".pem",
      "Signature to Match": "",
      "Severity": "low",
      "Severity Score": 2.50,
      "Starting Index of Match in Original Content": 0,
      "Relative Starting Index of Match in Displayed Substring": 0,
      "Relative Ending Index of Match in Displayed Substring": 4,
      "Full File Name": "etc/ssl/certs/ACCVRAIZ1.pem",
      "Matched Contents": ".pem"
    }
```

if you see about scan report its provide `Image Layer ID` and `Matched Rule Name` that describe about type of secret its detect also its provide `"Severity": "type", "Severity Score": 2.50`, with `"Matched Contents": "<contenttype">`

## use .dockerignore file

What should you add to the .dockerignore file?

* Temporary files and folders
    
* Build logs
    
* Local secrets
    
* Local development files like docker-compose.yml
    
* Version control folders like ".git", ".hg", and ".svn" Example:
    

```plaintext
**/.git
**/.gitignore
**/.vscode
**/coverage
**/.env
**/.aws
**/.ssh
Dockerfile
README.md
docker-compose.yml
**/.DS_Store
**/venv
**/env
```

Check it out SecretScanner and support this project by giving gitstar !

[![Readme Card](https://github-readme-stats.vercel.app/api/pin/?username=deepfence&repo=SecretScanner)](https://github.com/deepfence/SecretScanner)

