# Introduction
<img width="684" alt="image" src="https://github.com/Mate0r/app.hackthebock.com/assets/94843357/30184ea8-6e74-4147-8627-a560a4492ada">

# Nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx jupiter.htb
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- jupiter.htb
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# user.txt

## HTTP (port 80)

