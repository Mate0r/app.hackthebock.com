# Introduction

We first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.11.221 twomillion.htb
```

# Enumeration

We do a nmap with some options :\
\
-T4 is for have more threads to accelerate the scan\
-p- is to scan all the ports\
-sV is to scan the version\
\
we run the scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -p- -sV twomillion.htb
```

We got this result
```bash
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    nginx
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
# HTTP (port 80)

Let's enumerate with feroxbuster while we'll try some file manually

http://2million.htb/

http://2million.htb/api/v1/user/
http://2million.htb/api/v1/user/login
http://2million.htb/api/v1/user/register
http://2million.htb/api/v1/user/auth

http://2million.htb/api/v1/user/vpn/
http://2million.htb/api/v1/user/vpn/generate
http://2million.htb/api/v1/user/vpn/regenerate
http://2million.htb/api/v1/user/vpn/download

http://2million.htb/api/v1/machines/
http://2million.htb/api/v1/machines/ping/"+t+"?api_token="+e

http://2million.htb/api/v1/shouts/
http://2million.htb/api/v1/shouts/support/
http://2million.htb/api/v1/shouts/support/get/
http://2million.htb/api/v1/shouts/support/get/html/" + t + "?api_token=" + e
http://2million.htb/api/v1/shouts/support/new/?api_token=" + e

http://2million.htb/api/v1/shouts/team/
http://2million.htb/api/v1/shouts/team/get/
http://2million.htb/api/v1/shouts/team/get/html/" + t + "?api_token=" + e
http://2million.htb/api/v1/shouts/team/new/?api_token=" + e

http://2million.htb/api/v1/stats/
http://2million.htb/api/v1/stats/daily/
http://2million.htb/api/v1/stats/daily/owns/" + t

http://2million.htb/api/v1/stats/global

http://2million.htb/api/v1/users
http://2million.htb/api/v1/users/htb/
http://2million.htb/api/v1/users/htb/connection/
http://2million.htb/api/v1/users/htb/connection/status?api_token=" + t



www-data@2million:~/html$ cat .env
DB_HOST=127.0.0.1
DB_DATABASE=htb_prod
DB_USERNAME=admin
DB_PASSWORD=SuperDuperPass123

+----+----------------------+----------------------------+--------------------------------------------------------------+----------+
| id | username             | email                      | password                                                     | is_admin |
+----+----------------------+----------------------------+--------------------------------------------------------------+----------+
| 11 | TRX                  | trx@hackthebox.eu          | $2y$10$TG6oZ3ow5UZhLlw7MDME5um7j/7Cw1o6BhY8RhHMnrr2ObU3loEMq |        1 |
| 12 | TheCyberGeek         | thecybergeek@hackthebox.eu | $2y$10$wATidKUukcOeJRaBpYtOyekSpwkKghaNYr5pjsomZUKAd0wbzw4QK |        1 |
| 13 | test                 | test@test.com              | $2y$10$cndhRs8OIARzjO7B5TsG9.BxW3DkM1Qq1NzhfvMyMeOR52RG7TlzG |        1 |
| 14 | little.apple@ukr.net | little.apple@ukr.net       | $2y$10$qFwQtIWzvMFIkw4SooWSD.NhAetYzCYkIuH4zfiknD4vRBHS8ipxC |        0 |
+----+----------------------+----------------------------+--------------------------------------------------------------+----------+
