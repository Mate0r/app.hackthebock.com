# #1 Introduction
<img width="692" alt="image" src="https://github.com/Mate0r/app.hackthebock.com/assets/94843357/05e5079a-22d0-4d14-bb1f-24a3fd580502">

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx cybermonday.htb
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- cybermonday.htb
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)
80/tcp open  http    nginx 1.25.1
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# #3 user.txt

## HTTP (port 80)

we first start by doing a nikto while we'll explore the website \
the website use PHP/8.1.20

by seeing the 404 error page, it seems there is a laravel running
<img width="1342" alt="image" src="https://github.com/Mate0r/app.hackthebock.com/assets/94843357/be049187-e056-4f56-8733-91ebb4ddebb2">

there is a .htaccess at the root that confirms this is a laravel

so we have a Laravel running with PHP/8.1.20 on a nginx 1.25.1






there is a /login/ url that we can maybe exploit

there is cookies that are base64 encoded that we can maybe exploit



