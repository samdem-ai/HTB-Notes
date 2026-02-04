
2025-10-09 12:19

Status: 

Tags: [[tools]], [[options]],[[cheatsheet]],[[footprinting]]


# Infrastructure & Host Enumeration — Cheat Sheet

A compact, copy-paste friendly markdown cheat sheet for common enumeration commands.

---

## Infrastructure-based Enumeration

| Command                                                                                                                      | Description                                  |
| ---------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------- |
| curl -s "[https://crt.sh/?q=&lt;target-domain&gt;&amp;output=json](https://crt.sh/?q=&lt;target-domain&gt;&amp;output=json)" | jq .                                         |
| for i in $(cat ip-addresses.txt); do shodan host $i; done                                                                    | Scan each IP address in a list using Shodan. |

**Copy-and-paste (Infrastructure)**

```bash
curl -s "https://crt.sh/?q=<target-domain>&output=json" | jq .
for i in $(cat ip-addresses.txt); do shodan host $i; done
```

---

## Host-based Enumeration

### FTP

|Command|Description|
|---|---|
|ftp <FQDN/IP>|Interact with the FTP service on the target.|
|nc -nv <FQDN/IP> 21|Interact with the FTP service on the target.|
|telnet <FQDN/IP> 21|Interact with the FTP service on the target.|
|openssl s_client -connect <FQDN/IP>:21 -starttls ftp|Interact with the FTP service using an encrypted connection.|
|wget -m --no-passive ftp://anonymous:anonymous@<target>|Download all available files from an anonymous FTP server.|

**Copy-and-paste (FTP)**

```bash
ftp <FQDN/IP>
nc -nv <FQDN/IP> 21
telnet <FQDN/IP> 21
openssl s_client -connect <FQDN/IP>:21 -starttls ftp
wget -m --no-passive ftp://anonymous:anonymous@<target>
```

---

### SMB

|Command|Description|
|---|---|
|smbclient -N -L //<FQDN/IP>|Null session authentication on SMB (list shares).|
|smbclient //<FQDN/IP>/<share>|Connect to a specific SMB share.|
|rpcclient -U "" <FQDN/IP>|Interact with the target using RPC.|
|samrdump.py <FQDN/IP>|Username enumeration using Impacket scripts.|
|smbmap -H <FQDN/IP>|Enumerate SMB shares.|
|crackmapexec smb <FQDN/IP> --shares -u '' -p ''|Enumerate SMB shares using null session authentication.|
|enum4linux-ng.py <FQDN/IP> -A|SMB enumeration using enum4linux-ng.|

**Copy-and-paste (SMB)**

```bash
smbclient -N -L //<FQDN/IP>
smbclient //<FQDN/IP>/<share>
rpcclient -U "" <FQDN/IP>
samrdump.py <FQDN/IP>
smbmap -H <FQDN/IP>
crackmapexec smb <FQDN/IP> --shares -u '' -p ''
enum4linux-ng.py <FQDN/IP> -A
```

---

### NFS

|Command|Description|
|---|---|
|showmount -e <FQDN/IP>|Show available NFS shares.|
|mount -t nfs <FQDN/IP>:/<share> ./target-NFS/ -o nolock|Mount the specific NFS share to ./target-NFS.|
|umount ./target-NFS|Unmount the specific NFS share.|

**Copy-and-paste (NFS)**

```bash
showmount -e <FQDN/IP>
mount -t nfs <FQDN/IP>:/<share> ./target-NFS/ -o nolock
umount ./target-NFS
```

---

### DNS

|Command|Description|
|---|---|
|dig ns <domain.tld> @<nameserver>|NS request to the specific nameserver.|
|dig any <domain.tld> @<nameserver>|ANY request to the specific nameserver.|
|dig axfr <domain.tld> @<nameserver>|AXFR (zone transfer) request to the nameserver.|
|dnsenum --dnsserver <nameserver> --enum -p 0 -s 0 -o found_subdomains.txt -f ~/subdomains.list <domain.tld>|Subdomain brute forcing with a wordlist.|

**Copy-and-paste (DNS)**

```bash
dig ns <domain.tld> @<nameserver>
dig any <domain.tld> @<nameserver>
dig axfr <domain.tld> @<nameserver>
dnsenum --dnsserver <nameserver> --enum -p 0 -s 0 -o found_subdomains.txt -f ~/subdomains.list <domain.tld>
```

---

### SMTP

|Command|Description|
|---|---|
|telnet <FQDN/IP> 25|Connect to SMTP service (send EHLO/VRFY/EXPN, etc.).|

**Copy-and-paste (SMTP)**

```bash
telnet <FQDN/IP> 25
```

---

### IMAP / POP3

|Command|Description|
|---|---|
|curl -k 'imaps://<FQDN/IP>' --user <user>:<password>|Log in to the IMAPS service using cURL.|
|openssl s_client -connect <FQDN/IP>:imaps|Connect to the IMAPS service.|
|openssl s_client -connect <FQDN/IP>:pop3s|Connect to the POP3s service.|

**Copy-and-paste (IMAP/POP3)**

```bash
curl -k 'imaps://<FQDN/IP>' --user <user>:<password>
openssl s_client -connect <FQDN/IP>:imaps
openssl s_client -connect <FQDN/IP>:pop3s
```

---

### SNMP

|Command|Description|
|---|---|
|snmpwalk -v2c -c <community string> <FQDN/IP>|Query OIDs using snmpwalk.|
|onesixtyone -c community-strings.list <FQDN/IP>|Bruteforce SNMP community strings.|
|braa <community string>@<FQDN/IP>:.1.*|Bruteforce SNMP OIDs with braa.|

**Copy-and-paste (SNMP)**

```bash
snmpwalk -v2c -c <community string> <FQDN/IP>
onesixtyone -c community-strings.list <FQDN/IP>
braa <community string>@<FQDN/IP>:.1.*
```

---

### MySQL

|Command|Description|
|---|---|
|mysql -u <user> -p<password> -h <FQDN/IP>|Login to the MySQL server.|

**Copy-and-paste (MySQL)**

```bash
mysql -u <user> -p<password> -h <FQDN/IP>
```

---

### MSSQL

|Command|Description|
|---|---|
|mssqlclient.py <user>@<FQDN/IP> -windows-auth|Log in to the MSSQL server using Windows authentication.|

**Copy-and-paste (MSSQL)**

```bash
mssqlclient.py <user>@<FQDN/IP> -windows-auth
```

---

### IPMI

|Command|Description|
|---|---|
|msf6 auxiliary(scanner/ipmi/ipmi_version)|IPMI version detection (Metasploit).|
|msf6 auxiliary(scanner/ipmi/ipmi_dumphashes)|Dump IPMI hashes (Metasploit).|

**Copy-and-paste (IPMI)**

```bash
msf6 auxiliary(scanner/ipmi/ipmi_version)
msf6 auxiliary(scanner/ipmi/ipmi_dumphashes)
```

---

### Linux Remote Management

|Command|Description|
|---|---|
|ssh-audit.py <FQDN/IP>|Remote security audit against the target SSH service.|
|ssh <user>@<FQDN/IP>|Log in to the SSH server using the SSH client.|
|ssh -i private.key <user>@<FQDN/IP>|Log in using a private key.|
|ssh <user>@<FQDN/IP> -o PreferredAuthentications=password|Enforce password-based authentication.|

**Copy-and-paste (Linux SSH)**

```bash
ssh-audit.py <FQDN/IP>
ssh <user>@<FQDN/IP>
ssh -i private.key <user>@<FQDN/IP>
ssh <user>@<FQDN/IP> -o PreferredAuthentications=password
```

---

### Windows Remote Management

|Command|Description|
|---|---|
|rdp-sec-check.pl <FQDN/IP>|Check the security settings of the RDP service.|
|xfreerdp /u:<user> /p:"<password>" /v:<FQDN/IP>|Log in to the RDP server from Linux.|
|evil-winrm -i <FQDN/IP> -u <user> -p <password>|Log in to the WinRM server.|
|wmiexec.py <user>:"<password>"@<FQDN/IP> "<system command>"|Execute a command using WMI.|

**Copy-and-paste (Windows RDP/WinRM/WMI)**

```bash
rdp-sec-check.pl <FQDN/IP>
xfreerdp /u:<user> /p:"<password>" /v:<FQDN/IP>
evil-winrm -i <FQDN/IP> -u <user> -p <password>
wmiexec.py <user>:"<password>"@<FQDN/IP> "<system command>"
```

---

### Oracle TNS

|Command|Description|
|---|---|
|./odat.py all -s <FQDN/IP>|Perform a variety of Oracle DB scans (odat).|
|sqlplus <user>/<pass>@<FQDN/IP>/<db>|Log in to the Oracle database using sqlplus.|
|./odat.py utlfile -s <FQDN/IP> -d <db> -U <user> -P <pass> --sysdba --putFile C:\insert\path file.txt ./file.txt|Use odat to write a file to the remote database server (example).|

**Copy-and-paste (Oracle)**

```bash
./odat.py all -s <FQDN/IP>
sqlplus <user>/<pass>@<FQDN/IP>/<db>
./odat.py utlfile -s <FQDN/IP> -d <db> -U <user> -P <pass> --sysdba --putFile C:\\insert\\path file.txt ./file.txt
```

---

_Tip:_ Replace placeholders like `<FQDN/IP>`, `<domain.tld>`, `<user>`, and `<password>` before running commands. Use a safe test environment or an authorised engagement scope when running enumeration commands.


# References
