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

Samantha Wood
HR Manager
samantha.wood93@wifinetic.htb


info@wifinetic.htb
+44 7583 433 434
wifinetic.htb
10 Downing St, London
SW1A 2AA, United
Kingdom
@wifinetic


management@wifinetic.htb


Oliver Walker
Wireless Network Administrator
olivia.walker17@wifinetic.htb


```
┌──(parallels㉿kali)-[~/hacking/htb/Wifinetic/etc]
└─$ cat config/rpcd 
config rpcd
        option socket /var/run/ubus/ubus.sock
        option timeout 30

config login
        option username 'root'
        option password '$p$root'
        list read '*'
        list write '*'
```

┌──(parallels㉿kali)-[~/hacking/htb/Wifinetic/etc]
└─$ cat config/wireless 

config wifi-device 'radio0'
        option type 'mac80211'
        option path 'virtual/mac80211_hwsim/hwsim0'
        option cell_density '0'
        option channel 'auto'
        option band '2g'
        option txpower '20'

config wifi-device 'radio1'
        option type 'mac80211'
        option path 'virtual/mac80211_hwsim/hwsim1'
        option channel '36'
        option band '5g'
        option htmode 'HE80'
        option cell_density '0'

config wifi-iface 'wifinet0'
        option device 'radio0'
        option mode 'ap'
        option ssid 'OpenWrt'
        option encryption 'psk'
        option key 'VeRyUniUqWiFIPasswrd1!'
        option wps_pushbutton '1'

config wifi-iface 'wifinet1'
        option device 'radio1'
        option mode 'sta'
        option network 'wwan'
        option ssid 'OpenWrt'
        option encryption 'psk'
        option key 'VeRyUniUqWiFIPasswrd1!'

we can connect as netadmin


/usr/bin/systemctl restart hostapd.service
/usr/bin/systemctl restart wpa_supplicant.service
/sbin/wpa_supplicant -u -s -c /etc/wpa_supplicant.conf -i wlan1
/usr/sbin/hostapd_cli -i wlan0 wps_ap_pin set 12345670 0



WhatIsRealAnDWhAtIsNot51121!






























