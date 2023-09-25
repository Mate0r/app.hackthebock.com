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

http://zipping.htb/upload.php \
http://zipping.htb/shop\
http://zipping.htb/uploads\
http://zipping.htb/shop/functions.php\
http://zipping.htb/shop/home.php\
http://zipping.htb/index.php\
http://zipping.htb/shop/placeorder.php\
http://zipping.htb/shop/product.php\
http://zipping.htb/shop/products.php\
http://zipping.htb/shop/cart.php\







