# Chapter 15 - Tool Sets

## Types of Tools

### Preventative Tools
- Firewalls
  - Palo Alto
  - Fortinet
  - PFSense
  - Checkpoint
  - Cisco
  - NGFW (Next-Generation Firewalls) oftentimes includes features that allow them to share signatures with a cloud aggregator (OpenDXL) to block new attacks that are detected by other systems in the cloud network. These are often based on attack signatures and can also pull policies / signatures from well-known threat intel feeds.
- IDS and IPS
  - Sourcefire - IPS for web servers, part of CISCO's suite of tools in the Adaptive Security Appliance line of products
  - Snort
  - ZEEK (IDS) - makes hella logs, maybe think of MongoDB being used in conjunction with the ELK stack, not really meant to be used as an IPS
  - Suricata - IPS
- HIPS (Host-Intrusion Based Systems)
- Antimalware / AV
- Proxies
- WAF (Web Application Firewalls)
  - SecureSphere
  - ModSecurity
  - NAXSI
  - Nginx Anti XSS and SQLi
### Collective Tools
- SIEM (Security Information and Event Management)
  - Security Intelligence Platform (Splunk)
  - ELK (Elastic LogStash Kibana) Stack
  - Unified Security Management Platform
  - ArcSight
  - Kiwi Syslog
  - OSSIM
  - QRadar
- Network Scanning
  - NMAP
  - Wireshark
  - TCPDump
  - Aircrack-ng
- Command-line Utilities
  - Ping
  - Netstat
  - Traceroute
  - ifconfic
  - ip a
  - nslookup
  - OpenSSL
  - Sysinternals (Toolset by Mark Russinovich over at MSFT) Toolset can be found [here](https://docs.microsoft.com/en-us/sysinternals/). Some of the most helpful tools here are Procmon / Process Explorer, Autoruns and Sysmon
### Analytical Tools
- Vulnerability Scanning
  - OpenVAS
  - Nessus
  - Qualys
  - Nexpose (Used in conjunction with Metasploit as it's developed by Rapid7)
  - Nikto (primarily used for webservers)
### Monitoring Tools
- MRTG
- Nagios
- Orion
- Cacti
- Netflow Analyzer
![Monitoring Tools List](/images/2020-09-20-15-28-46.png)
- Interception Proxy
  - Burp Suite
  - OWASP Zap (Zed Attack Proxy)
  - Vega
### Exploitative Tools
- Exploitation Frameworks
  - Metasploit
  - Nexpose - Useful at determining and prioritizing exploitable vulnerabilites on the network
- Fuzzers
  - Untidy (PortSwigger) (Discovering vulnerabilities in web clients and servers)
  - Peach Fuzzer (OWASP) (Fuzzing binaries, embedded systems and IoT devices)
  - Microsoft SDL fuzzers (Basic File and REGEX fuzzing capabilities)
### Forensic Tools
- EnCase - Digital Forensic Suite (Paid)
- FTK (Forensic Toolkit) (free or paid)
- Helix (Based on open source Knoppix live boot utility) (Free or Paid)
- Cellebrite Universal Forensic Extraction Device (Paid)
- Password Cracking
  - John the Ripper
  - Cain and Abel (Windows based)
  - hashcat
- Hashing
  - MD5sum
    - 128-bit hash
  - shasum
    - You can add the switch -a to modify the algorithm strength, by default it is 160-bit hash
  - You can perform the same actions above on a Windows system in Powershell by doing:
  ```powershell
  get-filehash -path <filename> -algorithm [md5, sha1, sha256]
  ```
  - By default the hash type will be a sha256
- Imaging
  - dd - bit-for-bit copy of a hard drive
    - Primary purpose is to copy or convert files
    - Breakdown of the command:
      - dd = convert and copy a file
      - if=FILE, read from FILE instead of stdin
      - of=FILE, write to FILE instead of stdout
      - bs, read and writes up to BYTES bytes at a time
      - conv=CONVS, convert the file as per the comma separated symobol list
    - Example of the command is as follows. This specific line will create a copy of the hard drive "hda" to a file called case123.img using the options to set the block size to 4096 and fill the rest with null symbols should it encounter an error:
```bash
dd if=/dev/hda of=case123.img bs=4k conv=noerror,sync
```

## Chapter Review

1. ~~B. Imperva's SecureSphere~~
   1. D. NGFW (Next-Generation Firewall), can connect to threat intel feeds to quickly identify new attacks and share those with others with similar firewalls. They also have the ability to run applications in a sandbox to determine whether they are benign or malicious before allowing them into the network.
2. B. SIEM
3. C. OpenVAS
4. D. Exploitation Framework
5. C. Burp Suite
6. A. Untidy
7. B. Sourcefire
   1. Sourcefire is an IPS for web servers, can be configured to block traffic to your server containing the problematic password values
8. ~~D. Netstat~~
   1. C. nmap - as it's the quickest most effective method for scanning your network and searching for systems which have port 1337 open / listening.
   ```bash
   nmap  -p 1337 192.168.10.0/24
   ```
9.  A. tcpdump
10. D. dd, E. shasum