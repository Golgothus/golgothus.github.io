# Selecting the Best Course of Action

## Network Related Symptoms

### Netflow Traffic

![netflow traffic](../../images/2020-04-30%2017_34_21-Clipboard.png)

### Beaconing

Beaconing is a periodical outbound connection between a compromised computer and an external controller.

Typically determined by two characteristics:

- Periodicity
- Destination

To pick out beacnoning in traffic, analyze by:

- Internal Source address
- Destination address
- Time

### Irregular p2p

Things to look for:

- Unprivileged accounts connecting to other hosts
- Privileged accounts connecting from regular hosts (jump boxes)
- Repeated failed remote logins

> Does this user have any legitimate reson to be connecting from this host to the other resources?

### Rogue devices on the network

Best way to prohibit rogue devices is to know what systems shoudl **normally** be active. Users can try and connect to an access point through wired / wireless means, it's best to use a NAC (Network Access Control) to assist in manging access.

If you cannot use a NAC, it's best to have logs from all AP's reach provide their logs to a centralized location.

### Scan sweeps

Some attackers will use NMAP or another scanning mechanism to map out your network. This will generate a large number of ARP (address resolution protocol) queries

![ARP Queries](../../images/2020-04-30%2019_23_25-Clipboard.png)

## Host-related Symptoms

### Running Processes

> Very first tasks should be to examine running processes

it is always preferable to capture memory first, and then look at running processes. We reverse the order here for pedagogical reasons.

> Easy way to see established network connections for processes is by using **netstat**

- Windows
  - netstat -ano
- Mac OS
  - netstat -v
- Linux
  - netstat -nap

Tools for capturing volatile memory:

- [Access Data's FTK Imager](https://accessdata.com/product-download)
- [Hal Pomeranz's Linux Memory Grabber](https://github.com/halpomeranz/lmg)

Tools for memory anlaysis:

- [Volatility framework](https://www.volatilityfoundation.org/)

### File System

File systems - set of processes and data structures that an OS uses to manage data in persistent storage devices such as a hard drive

> *Artifact* denotes a digital object of interest to a forensic investigation

It is important to implement a software / hardware inventory to make it easier to detect unauthorized software as well as to allo for not only software, but license auditability

### Unauthorized changes

Common technique to maintain access is to replace system libraries (DLLs), with malicious ones

> Windows will automatically log / detect unauthorized changes to sensitive files / folders. This feature is called **Object Access Auditing**, event code 5136

## Application-related Symptoms

- Applications reaching outside of the network where they had not before (unexpected outbound communication)
- Applications using features / plug-ins that were unused and unknown
- Use of macros in applications
- Unexpected output (random UAC prompts)
- Anomolous activity, seeing pages freeze, applications taking a while to load / show, rapidly changing URL addresses, browser restarts
- Service interruption (services that are starting, stopping, restarting)

## Chapter Review

1. ~~A. Blacklisting (running only known-benign software)~~ B. Whitelisting
2. C. Exfiltration
3. B. Adding new user accounts
4. A. Random Access Memory
5. ~~D. Disk Space~~ A. Network Speed
6. D. NAC
7. ~~B. Exfiltration of large design files~~ C. License Verification
8. B. The printer server belong to logistics
9. A. This device might not arouse suspicion due to its normal purpoose on the network
10. D. Beaconing
