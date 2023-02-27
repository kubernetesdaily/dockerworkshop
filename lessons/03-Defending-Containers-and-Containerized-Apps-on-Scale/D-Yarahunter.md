---

title: " YaraHunter - Malware Scanner for Container Images "
description: " YaraHunter "

---


## What is YARA ?

YARA was originally developed by Victor Alvarez of VirusTotal and released on github in 2013 .

> YARA , the "Pattern matching Swiss knife for malware researchers available on [github](https://github.com/VirusTotal/yara)

#### Install yara on Mac

```plaintext
brew install yara
```

#### Yara is extremely popular within because of different use case :

* Identify and classify malware
    
* Find new samples based on family-specific pattern
    
* Deploy Yara Rules to identify samples and compromised device for incident response
    

#### Why Yara ?

* Before Yara it was difficult for malware researcher To detect and classify large volumes of malware samples
    
* can effective be used of hunting evolving malwares
    
* Since Yara Rules are completely base on text , using an easy to understand syntax , they can be developed quickly
    

## How to write custom detection in YARA

1. Start of the rule: Every YARA rule should be declared by using the keyword rule followed by an identifier , or unique rule would like to give your rule
    
2. Add your meta section : the meta section can be used to provide comments or details about your rule information provided under meta will not be used for any variation of malware detection
    
3. Declare string : this is where you can declare a variable and set it value each variable is indicated using $ sign followed by the variable name
    
4. add your condition section : the condition section is where the rule declares what conditions must be met in order to rule to trigger a match .
    

### my first yara rule

```plaintext
rule Hello_World
  {
          condition:
                  true
}
```

verify yara is installed or not

```plaintext
yara --help
YARA 4.2.3, the pattern matching swiss army knife.
Usage: yara [OPTION]... [NAMESPACE:]RULES_FILE... FILE | DIR | PID

Mandatory arguments to long options are mandatory for short options too.

       --atom-quality-table=FILE           path to a file with the atom quality table
  -C,  --compiled-rules                    load compiled rules
  -c,  --count                             print only number of matches
  -d,  --define=VAR=VALUE                  define external variable
       --fail-on-warnings                  fail on warnings
  -f,  --fast-scan                         fast matching mode
  -h,  --help                              show this help and exit
  -i,  --identifier=IDENTIFIER             print only rules named IDENTIFIER
       --max-process-memory-chunk=NUMBER   set maximum chunk size while reading process memory (default=1073741824)
  -l,  --max-rules=NUMBER                  abort scanning after matching a NUMBER of rules
       --max-strings-per-rule=NUMBER       set maximum number of strings per rule (default=10000)
  -x,  --module-data=MODULE=FILE           pass FILE's content as extra data to MODULE
  -n,  --negate                            print only not satisfied rules (negate)
  -N,  --no-follow-symlinks                do not follow symlinks when scanning
  -w,  --no-warnings                       disable warnings
  -m,  --print-meta                        print metadata
  -D,  --print-module-data                 print module data
  -e,  --print-namespace                   print rules' namespace
  -S,  --print-stats                       print rules' statistics
  -s,  --print-strings                     print matching strings
  -L,  --print-string-length               print length of matched strings
  -g,  --print-tags                        print tags
  -r,  --recursive                         recursively search directories
       --scan-list                         scan files listed in FILE, one per line
  -z,  --skip-larger=NUMBER                skip files larger than the given size when scanning a directory
  -k,  --stack-size=SLOTS                  set maximum stack size (default=16384)
  -t,  --tag=TAG                           print only rules tagged as TAG
  -p,  --threads=NUMBER                    use the specified NUMBER of threads to scan a directory
  -a,  --timeout=SECONDS                   abort scanning after the given number of SECONDS
  -v,  --version                           show version information
```

#### Now let the computer greet you:

```plaintext
$ yara hello.yara /yara/malware/somefile.txt
```

```plaintext
rule GoodMorning
  {
          condition:
                hour < 12 and hour >= 4
}
```

#### Now let the computer greet you:

```plaintext
yara hello.yara -d hour=11 ./yara/malware/somefile.txt
GoodMorning ./yara/malware/somefile.txt
```

#### structure of YARA rule file

rule RULE\_NAME { // Rule definition goes here! // Comments in Yara rules look like this! }

The “meta” section of a rule contains the description, author, reference, date, hash, and any other relevant details of the rule. This section is optional and will not be used to classify malware. `meta: description = "This is just an example" threat_level = 3 in_the_wild = true`

The “strings” section contains string patterns that are used to identify malware. Each string in the “strings” section is identified with a variable name starting with a dollar sign.

```plaintext
strings:
       $a = {6A 40 68 00 30 00 00 6A 14 8D 91}
       $b = {8D 4D B0 2B C1 83 C0 27 99 6A 4E 59 F7 F9}
       $c = "UVODFRYSIHLNWPEJXQZAKCBGMT"
```

You should put signature strings that are indicative of the malware here. This example uses hex strings and text strings. But you can also use regex patterns. `strings: $a = {6A 40 68 00 30 00 00 6A 14 8D 91} // Hex strings are enclosed within curly brackets. $b = "UVODFRYSIHLNWPEJXQZAKCBGMT" // Plain text strings are enclosed within double quotes. $c = /md5: [0-9a-fA-F]{32}/ // Regex patterns are enclosed within slashes.` Finally, the “condition” section describes how the string patterns in the “strings” section should be used to identify a piece of malware. You can use boolean (and, or, not), relational (&gt;, &lt;, =, and more), and arithmetic (+, -, \*, /, %) expressions in this section. In our example, the rule specifies that if one of the strings $a, $b, or $c is present, the file is a silent banker trojan. `condition: $a or $b or $c` You can also define more complicated conditions like these. ``` condition: #a &gt; 2 and $b // If $a occurs more than twice and if $b is present

condition: ($a and $b) or ($b and $c) // If both $a and $b are present, or both $b and $c are present ```

## Lets Bring Malware scanner for cloud-native, as part of CI/CD and at Runtime

Deepfence's [YaraHunter](https://github.com/deepfence/YaraHunter) - scans container images, running Docker containers, and filesystems to find indicators of malware. It uses a [YARA ruleset](https://github.com/deepfence/yara-rules/blob/main/malware.yar) to identify resources that match known malware signatures, and may indicate that the container or filesystem has been compromised.

### YaRadare can be used to detect malware in cloud-native applications in the following ways:

* At rest: scan local container images, before they are deployed, to verify they do not contain malware
    
* At runtime: scan running docker containers, if, for example, you observe unusual network traffic or CPU activity
    
* Against filesystems: at any time, YaRadare can scan a local directory or filesystem for indicators of compromise
    

we all build and scan images but we never take look at malware that packaged inside container or even file system YaraHunter can hunt malware that present at your docker images or Kubernetes cluster or you can use in CI/CD pipeline

#### pull image you want to scan

Images may be compromised with the installation of a cryptominer such as XMRig. In the following example, we'll scan a legitimiate cryptominer image that contains the same xmrig software that is often installed through an exploit:

```plaintext
 docker pull metal3d/xmrig
```

#### Run YaraHunter as Docker Container and get result in json

![](https://raw.githubusercontent.com/deepfence/YaraHunter/main/docs/docs/yarahunter/img/yarahunter.svg)

``` bash
docker run -it --rm --name=deepfence-yarahunter  
\-v /var/run/docker.sock:/var/run/docker.sock  
\-v /tmp:/home/deepfence/output  
deepfenceio/yara-hunter:latest --image-name metal3d/xmrig:latest  
\--json-filename=xmrig-scan.json
```

```bash

 Using default tag: latest latest: Pulling from metal3d/xmrig 2408cc74d12b: Pull complete 75fcf72b2223: Pull complete 4e7c4ed53fb2: Pull complete Digest: sha256:c3c27a8b2f6beede6d9c0a7e5b79bb7a7b0002cca40565e7bfd2e447f3a2a628 Status: Downloaded newer image for metal3d/xmrig:latest docker.io/metal3d/xmrig:latest WARNING: The requested image's platform (linux/amd64) does not match the detected host platform (linux/arm64/v8) and no specific platform was requested INFO\[2022-09-09 00:53:43\] trying to connect to endpoint 'unix:///var/run/docker.sock' with timeout '10s' INFO\[2022-09-09 00:53:43\] connected successfully using endpoint: unix:///var/run/docker.sock INFO\[2022-09-09 00:53:43\] trying to connect to endpoint 'unix:///run/containerd/containerd.sock' with timeout '10s' WARN\[2022-09-09 00:53:53\] could not connect to endpoint 'unix:///run/containerd/containerd.sock': context deadline exceeded INFO\[2022-09-09 00:53:53\] trying to connect to endpoint 'unix:///run/k3s/containerd/containerd.sock' with timeout '10s' WARN\[2022-09-09 00:54:03\] could not connect to endpoint 'unix:///run/k3s/containerd/containerd.sock': context deadline exceeded INFO\[2022-09-09 00:54:03\] container runtime detected: docker  
{ "Timestamp": "2022-09-09 00:54:27.639796179 +00:00", "Image Name": "metal3d/xmrig:latest", "Image ID": "a01f1ffa6691423ef43bfaee2a9c9f30fe08ee6df8d9d6586ae9692d90789c5a", "Malware match detected are": \[ { "Image Layer ID": "bad74b706fcd3e01f4af74337744cbcc84ab60da82c40dd588469c6360258789", "Matched Rule Name": "XMRIG\_Miner", "Strings to match are": \[ "stratum+tcp" \], "Category": \[\], "File Name": "/tmp/Deepfence/YaRadare/df\_metal3dxmriglatest/ExtractedFiles/bad74b706fcd3e01f4af74337744cbcc84ab60da82c40dd588469c6360258789/usr/local/bin/xmrig", "ref":"https://gist.github.com/GelosSnake/c2d4d6ef6f93ccb7d3afb5b1e26c7b4e ", "Summary": "The matched rule file's ref is https://gist.github.com/GelosSnake/c2d4d6ef6f93ccb7d3afb5b1e26c7b4e ." } , { "Image Layer ID": "bad74b706fcd3e01f4af74337744cbcc84ab60da82c40dd588469c6360258789", "Matched Rule Name": "XMRIG\_Miner", "Strings to match are": \[ "stratum+tcp" \], "Category": \[\], "File Name": "/tmp/Deepfence/YaRadare/df\_metal3dxmriglatest/ExtractedFiles/bad74b706fcd3e01f4af74337744cbcc84ab60da82c40dd588469c6360258789/xmrig-6.18.0/build/CMakeFiles/xmrig.dir/src/base/net/stratum/Url.cpp.o", "ref":"https://gist.github.com/GelosSnake/c2d4d6ef6f93ccb7d3afb5b1e26c7b4e ", "Summary": "The matched rule file's ref is https://gist.github.com/GelosSnake/c2d4d6ef6f93ccb7d3afb5b1e26c7b4e ." } , { "Image Layer ID": "bad74b706fcd3e01f4af74337744cbcc84ab60da82c40dd588469c6360258789", "Matched Rule Name": "Cerberus", "Strings to match are": \[ "cerberus" \], "Category": \["RAT","memory"\], "File Name": "/tmp/Deepfence/YaRadare/df\_metal3dxmriglatest/ExtractedFiles/bad74b706fcd3e01f4af74337744cbcc84ab60da82c40dd588469c6360258789/xmrig-6.18.0/src/3rdparty/fmt/README.rst", "description":"Cerberus ", "author":"Jean-Philippe Teissier / @Jipe\_ ", "date":"2013-01-12 ", "filetype":"memory ", "version":"1.0 ", "Summary": "The file /tmp/Deepfence/YaRadare/df\_metal3dxmriglatest/ExtractedFiles/bad74b706fcd3e01f4af74337744cbcc84ab60da82c40dd588469c6360258789/xmrig-6.18.0/src/3rdparty/fmt/README.rst has a memory match.The file has a rule match that Cerberus .The matched rule file's author is Jean-Philippe Teissier / @Jipe\_ .The matched rule file's date is 2013-01-12 .The matched rule file's filetype is memory .The matched rule file's version is 1.0 ." } , { "Image Layer ID": "bad74b706fcd3e01f4af74337744cbcc84ab60da82c40dd588469c6360258789", "Matched Rule Name": "XMRIG\_Miner", "Strings to match are": \[ "stratum+tcp" \], "Category": \[\], "File Name": "/tmp/Deepfence/YaRadare/df\_metal3dxmriglatest/ExtractedFiles/bad74b706fcd3e01f4af74337744cbcc84ab60da82c40dd588469c6360258789/xmrig-6.18.0/src/base/net/stratum/Url.cpp", "ref":"https://gist.github.com/GelosSnake/c2d4d6ef6f93ccb7d3afb5b1e26c7b4e ", "Summary": "The matched rule file's ref is https://gist.github.com/GelosSnake/c2d4d6ef6f93ccb7d3afb5b1e26c7b4e ." } , { "Image Layer ID": "bad74b706fcd3e01f4af74337744cbcc84ab60da82c40dd588469c6360258789", "Matched Rule Name": "XMRIG\_Miner", "Strings to match are": \[ "stratum+tcp" \], "Category": \[\], "File Name": "/tmp/Deepfence/YaRadare/df\_metal3dxmriglatest/ExtractedFiles/bad74b706fcd3e01f4af74337744cbcc84ab60da82c40dd588469c6360258789/usr/local/bin/xmrig", "ref":"https://gist.github.com/GelosSnake/c2d4d6ef6f93ccb7d3afb5b1e26c7b4e ", "Summary": "The matched rule file's ref is https://gist.github.com/GelosSnake/c2d4d6ef6f93ccb7d3afb5b1e26c7b4e ." } , { "Image Layer ID": "bad74b706fcd3e01f4af74337744cbcc84ab60da82c40dd588469c6360258789", "Matched Rule Name": "XMRIG\_Miner", "Strings to match are": \[ "stratum+tcp" \], "Category": \[\], "File Name": "/tmp/Deepfence/YaRadare/df\_metal3dxmriglatest/ExtractedFiles/bad74b706fcd3e01f4af74337744cbcc84ab60da82c40dd588469c6360258789/xmrig-6.18.0/build/CMakeFiles/xmrig.dir/src/base/net/stratum/Url.cpp.o", "ref":"https://gist.github.com/GelosSnake/c2d4d6ef6f93ccb7d3afb5b1e26c7b4e ", "Summary": "The matched rule file's ref is https://gist.github.com/GelosSnake/c2d4d6ef6f93ccb7d3afb5b1e26c7b4e ." } , { "Image Layer ID": "bad74b706fcd3e01f4af74337744cbcc84ab60da82c40dd588469c6360258789", "Matched Rule Name": "Cerberus", "Strings to match are": \[ "cerberus" \], "Category": \["RAT","memory"\], "File Name": "/tmp/Deepfence/YaRadare/df\_metal3dxmriglatest/ExtractedFiles/bad74b706fcd3e01f4af74337744cbcc84ab60da82c40dd588469c6360258789/xmrig-6.18.0/src/3rdparty/fmt/README.rst", "description":"Cerberus ", "author":"Jean-Philippe Teissier / @Jipe\_ ", "date":"2013-01-12 ", "filetype":"memory ", "version":"1.0 ", "Summary": "The file /tmp/Deepfence/YaRadare/df\_metal3dxmriglatest/ExtractedFiles/bad74b706fcd3e01f4af74337744cbcc84ab60da82c40dd588469c6360258789/xmrig-6.18.0/src/3rdparty/fmt/README.rst has a memory match.The file has a rule match that Cerberus .The matched rule file's author is Jean-Philippe Teissier / @Jipe\_ .The matched rule file's date is 2013-01-12 .The matched rule file's filetype is memory .The matched rule file's version is 1.0 ." } , { "Image Layer ID": "bad74b706fcd3e01f4af74337744cbcc84ab60da82c40dd588469c6360258789", "Matched Rule Name": "XMRIG\_Miner", "Strings to match are": \[ "stratum+tcp" \], "Category": \[\], "File Name": "/tmp/Deepfence/YaRadare/df\_metal3dxmriglatest/ExtractedFiles/bad74b706fcd3e01f4af74337744cbcc84ab60da82c40dd588469c6360258789/xmrig-6.18.0/src/base/net/stratum/Url.cpp", "ref":"https://gist.github.com/GelosSnake/c2d4d6ef6f93ccb7d3afb5b1e26c7b4e ", "Summary": "The matched rule file's ref is https://gist.github.com/GelosSnake/c2d4d6ef6f93ccb7d3afb5b1e26c7b4e ." }

\] }

```

Check it out YaraHunter and support this project by giving gitstar ! 

[![Readme Card](https://github-readme-stats.vercel.app/api/pin/?username=deepfence&repo=YaraHunter)](https://github.com/deepfence/YaraHunter)


