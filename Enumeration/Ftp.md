# FTP - Enumeration
## Methodology

```
1. Check which ftp server used
2. Check anonymous login
3. Check whether Exploit exists compatible with version
4. Brute force ftp login
5. Get/put interesting files
```

> ###### Anonymous Login
```
anonymous:anonymous
metasploit module (anon_login)
nmap script (ftp-anon)
```
> ###### Nmap Scripts
```
ls /usr/share/nmap/scripts/\* | grep ftp 
nmap -sV -Pn -vv -p [PORT] --script=ftp-anon,ftp-bounce,ftp-libopie,ftp-proftpd-backdoor,ftp-vsftpd-backdoor,ftp-vuln-cve2010-4221
```

> ###### Brute Force
```
hydra -t 1 -l admin -P /usr/share/wordlists/rockyou.txt -vV $ip ftp
hydra -s [PORT] -C ./wordlists/ftp-default-userpass.txt -u -f [IP] ftp
/usr/share/metasploit-framework/data/wordlists/unix_users.txt
/usr/share/seclists/Usernames/names.txt

```


5.
/etc/passwd
/etc/sudoers
/etc/shadow
config files for services
id_rsa or authorized_keys (sshngjohn.py)
SAM or SYSTEM files (sam2dump -d SYSTEM SAM)


6.
FTP SERVER #1 :
apt-get update && apt-get install pure-ftpd

#!/bin/bash
groupadd ftpgroup
useradd -g ftpgroup -d /dev/null -s /etc ftpuser
pure-pw useradd offsec -u ftpuser -d /ftphome
pure-pw mkdb
cd /etc/pure-ftpd/auth/
ln -s ../conf/PureDB 60pdb
mkdir -p /ftphome
chown -R ftpuser:ftpgroup /ftphome/

/etc/init.d/pure-ftpd restart

FTP SERVER #2:
python -m pyftpdlib -p 21 -w
