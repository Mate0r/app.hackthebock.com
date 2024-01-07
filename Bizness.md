# #1 Introduction
![image](https://github.com/Mate0r/app.hackthebock.com/assets/94843357/61902059-4a15-4f8e-b432-4249c79305c8)

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx bizness.htb
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- bizness.htb
```

we got these open ports
```bash
PORT      STATE SERVICE    VERSION
22/tcp    open  ssh        OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
80/tcp    open  http       nginx 1.18.0
443/tcp   open  ssl/http   nginx 1.18.0
34399/tcp open  tcpwrapped
45911/tcp open  tcpwrapped
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# #3 user.txt


we have a JSESSIONID so we have nodejs ? (maybe a deserialization ?)
or maybe a tomcat since we have .jvm1 at the end so there is a servlet

JSESSIONID	"BE10755A7A9054FB77506FFA467A65A8.jvm1"


with ffuf, we found one url :
https://bizness.htb/control
