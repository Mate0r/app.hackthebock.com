# #1 Introduction
<img width="691" alt="image" src="https://github.com/Mate0r/app.hackthebock.com/assets/94843357/17b93d9f-213a-43e4-8df5-45d166672edb">

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

