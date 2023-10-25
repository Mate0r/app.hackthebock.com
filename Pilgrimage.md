# Introduction
<img width="690" alt="image" src="https://github.com/Mate0r/app.hackthebock.com/assets/94843357/91eeee22-63a2-4bfa-9996-ca4c0f548cb9">

# Nmap
we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx pilgrimage.htb
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- pilgrimage.htb
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)
80/tcp open  http    nginx 1.18.0
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# user.txt

## HTTP (port 80)

we found a /.git/ folder
dashboard.php
index.php
index.php
login.php
logout.php$
register.php

https://github.com/Sybil-Scan/imagemagick-lfi-poc
