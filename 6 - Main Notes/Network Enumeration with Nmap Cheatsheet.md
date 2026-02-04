
2025-09-20 14:43

Status:

Tags:  [[nmap]], [[tools]], [[options]],[[cheatsheet]]


# Network Enumeration with Nmap Cheatsheet
## Scanning Options

```bash
nmap 10.10.10.0/24
````

Target network range.

```bash
nmap -sn <target>
```

Disables port scanning.

```bash
nmap -Pn <target>
```

Disables ICMP Echo Requests.

```bash
nmap -n <target>
```

Disables DNS Resolution.

```bash
nmap -PE <target>
```

Ping scan using ICMP Echo Requests.

```bash
nmap --packet-trace <target>
```

Shows all packets sent and received.

```bash
nmap --reason <target>
```

Displays the reason for a specific result.

```bash
nmap --disable-arp-ping <target>
```

Disables ARP Ping Requests.

```bash
nmap --top-ports=<num> <target>
```

Scans the top `<num>` most frequent ports.

```bash
nmap -p- <target>
```

Scan all ports.

```bash
nmap -p22-110 <target>
```

Scan all ports between 22 and 110.

```bash
nmap -p22,25 <target>
```

Scan only ports 22 and 25.

```bash
nmap -F <target>
```

Scans top 100 ports.

```bash
nmap -sS <target>
```

TCP SYN scan (stealth scan).

```bash
nmap -sA <target>
```

TCP ACK scan.

```bash
nmap -sU <target>
```

UDP scan.

```bash
nmap -sV <target>
```

Service version detection.

```bash
nmap -sC <target>
```

Run default scripts.

```bash
nmap --script <script> <target>
```

Run a specific script.

```bash
nmap -O <target>
```

OS Detection.

```bash
nmap -A <target>
```

Aggressive scan (OS, Services, Traceroute).

```bash
nmap -D RND:5 <target>
```

Use 5 random decoys.

```bash
nmap -e eth0 <target>
```

Use specific network interface.

```bash
nmap -S 10.10.10.200 <target>
```

Spoof source IP.

```bash
nmap -g 53 <target>
```

Spoof source port (e.g., 53 for DNS).

```bash
nmap --dns-server 8.8.8.8 <target>
```

Use a custom DNS server.

---

## Output Options

```bash
nmap -oA results <target>
```

Save in all formats (`.nmap`, `.gnmap`, `.xml`).

```bash
nmap -oN results.txt <target>
```

Save in normal format.

```bash
nmap -oG results.gnmap <target>
```

Save in grepable format.

```bash
nmap -oX results.xml <target>
```

Save in XML format.

---

## Performance Options

```bash
nmap --max-retries 2 <target>
```

Set max retries.

```bash
nmap --stats-every=5s <target>
```

Show status updates every 5 seconds.

```bash
nmap -v <target>
```

Verbose mode.

```bash
nmap -vv <target>
```

Very verbose mode.

```bash
nmap --initial-rtt-timeout 50ms <target>
```

Set initial RTT timeout.

```bash
nmap --max-rtt-timeout 100ms <target>
```

Set max RTT timeout.

```bash
nmap --min-rate 300 <target>
```

Send packets at a minimum rate of 300 per second.

```bash
nmap -T4 <target>
```

Timing template (0–5, higher = faster).


# References
- https://academy.hackthebox.com/module/details/19