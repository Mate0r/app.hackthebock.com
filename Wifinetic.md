# Introduction

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.11.221 wifinetic.htb
```

# Enumeration

we do a nmap with some options :\
\
-T4 is for have more threads to accelerate the scan\
-p- is to scan all the ports\
-sV is to scan the version\
\
we run the scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- wifinetic.htb
```

we got these results
```bash
PORT   STATE SERVICE    VERSION
21/tcp open  ftp        vsftpd 3.0.3
22/tcp open  ssh        OpenSSH 8.2p1 Ubuntu 4ubuntu0.9 (Ubuntu Linux; protocol 2.0)
53/tcp open  tcpwrapped
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```
