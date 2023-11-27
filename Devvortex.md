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
we found this is a 4.2.6 Joomla
