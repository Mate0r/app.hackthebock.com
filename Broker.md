# #1 Introduction
<img width="693" alt="image" src="https://github.com/Mate0r/app.hackthebock.com/assets/94843357/a1f1d832-68ca-40d1-b5c7-bdb7be15e255">

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx broker.htb
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- broker.htb
```

we got these open ports
```bash
PORT      STATE SERVICE    VERSION
22/tcp    open  ssh        OpenSSH 8.9p1 Ubuntu 3ubuntu0.4 (Ubuntu Linux; protocol 2.0)
80/tcp    open  http       nginx 1.18.0 (Ubuntu)
1883/tcp  open  mqtt
5672/tcp  open  amqp?
8161/tcp  open  http       Jetty 9.4.39.v20210325
45225/tcp open  tcpwrapped
61613/tcp open  stomp      Apache ActiveMQ
61614/tcp open  http       Jetty 9.4.39.v20210325
61616/tcp open  apachemq   ActiveMQ OpenWire transport
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# #3 user.txt


Powered by Jetty:// 9.4.39.v20210325

https://github.com/antonwierenga/activemq-cli

<img width="651" alt="image" src="https://github.com/Mate0r/app.hackthebock.com/assets/94843357/743d88e7-56cc-4f1d-84c5-d3b8c67880a6">
