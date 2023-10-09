# #1 Introduction

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx analytics.htb
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

bootstrap metabase : 	"v0.46.6"

https://github.com/robotmikhro/CVE-2023-38646

in env variables, id to ssh
metalytics
An4lytics_ds20223#

/etc/nginx/sites-enabled/data.analytical.htb

╔══════════╣ Checking if containerd(ctr) is available
╚ https://book.hacktricks.xyz/linux-unix/privilege-escalation/containerd-ctr-privilege-escalation                                                  
ctr was found in /usr/bin/ctr, you may be able to escalate privileges with it                                                                      
ctr: failed to dial "/run/containerd/containerd.sock": connection error: desc = "transport: error while dialing: dial unix /run/containerd/containerd.sock: connect: permission denied"

╔══════════╣ Checking if runc is available
╚ https://book.hacktricks.xyz/linux-unix/privilege-escalation/runc-privilege-escalation                                                            
runc was found in /usr/bin/runc, you may be able to escalate privileges with it 
