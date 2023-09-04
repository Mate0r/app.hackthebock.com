# Introduction

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx cozyhosting.htb
```

# Enumeration

we do a nmap with some options :\
\
-T4 is for have more threads to accelerate the scan\
-p- is to scan all the ports\
-sV is to scan the version\
\
we run the scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -p- -sV cozyhosting.htb
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
# HTTP (port 80)
after investicating, it seems it is Spring Boot running (JSP Java Server Pages)
we found some url in actuator
```
200      GET        1l        1w       15c http://cozyhosting.htb/actuator/health
200      GET        1l        1w      495c http://cozyhosting.htb/actuator/sessions
200      GET        1l      120w     4957c http://cozyhosting.htb/actuator/env
200      GET        1l      542w   127224c http://cozyhosting.htb/actuator/beans
```

