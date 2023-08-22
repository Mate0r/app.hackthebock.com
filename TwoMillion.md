# Introduction

We first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.11.221 twomillion.htb
```

# Enumeration

We do a nmap with some options :\
\
-T4 is for have more threads to accelerate the scan\
-p- is to scan all the ports\
-sV is to scan the version\
\
we run the scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -p- -sV twomillion.htb
```

We got this result
```bash

```
