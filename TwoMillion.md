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
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    nginx
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
# HTTP (port 80)

Let's enumerate with feroxbuster while we'll try some file manually

http://2million.htb/

http://2million.htb/api/v1/user/login
http://2million.htb/api/v1/user/register
http://2million.htb/api/v1/user/auth

http://2million.htb/api/v1/user/vpn/generate
http://2million.htb/api/v1/user/vpn/regenerate
http://2million.htb/api/v1/user/vpn/download

http://2million.htb/home/rules
http://2million.htb/home/access
http://2million.htb/home/changelog
