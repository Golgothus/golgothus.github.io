# Preparing the Incident Response Toolkit

> The digital forensic analyst answers the questions ***what, where, when, and how,*** but not ***who or why***

The National Institute of Justice identifies the following three principles that should guide every investigation:

- Actions taken to secure and collect digital evidence should not affect the integrity of that evidence.
- Persons conducting an examination of digital evidence should be trained for that purpose.
- Activity relating to the seizure, examination, storage, or transfer of digital evidence should be documented, preserved, and available for review.

Four phases of analysis:

- Seizure
  - Process of controlling the crime scene and the state of potential evidentiary items
- Acquisition
  - Preservation of evidence in a legally admissable manner
- Analysis
  - The analysis should take place in an environment where evidence should remain untainted
- Reporting
  - Goal to produce a report that is complete, accurate, and unbiased

Photos to take during the initial investigation:

- Computer Desktop showing running programs (if the device is unlocked)
- Peripherals connected to the device (for example, thumb drives and external drives)
- Immediate surroundings of the device (for exmaple, physical desktop)
- Proximate surroundings of the device (for example, the room or cubicle)

___

## Phases of an Investigation

- Seizure
- Acquisition
- Analysis
- Reporting

### Phases of an Investigation: Seizure

Purpose it to ensure that neither the perpetrators nor the investigators make any changes to the evidence

### Phases of an Investigation: Acquisition

Best Practices

1. Prepare the destination media.
   1. Make sure the device that the information is being written to is properly cleaned and free of any content that may taint the evidence
2. Prevent changes to the original.
   1. Use of write-blockers will help in maintaining the integrity of the contents.
3. Hash the original evidence. Before you copy anything, you should take a cryptographic hash of the original evidence. Most products support MD5 and SHA-1 hashes.
4. Copy the evidence.
5. Verify the acquisition. Make sure the hash of the copy and original contents of the evidence match.
6. Safeguard the original evidence. Because you now have a perfect copy of the evidence, you store the original in a safe place and ensure nobody gains access to it.

### Phases of an Investigation: Analysis

Key components:

- Timeframe: what happened when
- Data Hiding: things that have been intentionally concealed
- Application and file: which applications accessed which files
- Ownership and posession: which user accounts accessed which applications and files

Timeline: Simply an ordred list of actions taken on the system, actions can be categorized as read, write, modify, and delete operations on an item of interest

Columnize Timeline in a spreadsheet as follows:

| Date and Time | Time Zone | Source (registry or syslog) | Item Name (Reg Key or filename) | Item Location (full path) | Description |
|---|---|---|---|---|---|

### Forensic Investigation Suite

Acquisition Utilities

Best recommended to slow down and use a checklist and ensure you make no mistakes as doing so would possible invalidate all the work that follows

dd: \
Linux utility for copying a drive, bit-by-bit \
> dd if=/dev/hda of=case123.img bs=4k conv=noerror,sync

To create a sha1sum of a file, use:
> sha1sum \<filename\>

If you are looking to use a GUI on top of CLI for a forensic kit, you can use Autopsy on top of the open source / free analysis tool, Sleuth Kit

#### Useful Windows Directories

- Autorun locations, this is where programs tell Windows they should be launched during the boot process. Malware oftentimes uses this for persistance
  - HKLM\Software\Microsoft\Windows\CurrentVersion\Run
- Most Recently Used Lists, often referred to as MRUs, this is where you find the most recently launched apps, documents, and changed reg keys
  - HKEY_Current_User\Software\Microsoft\Office\12.0\Common\Open Find
- Wireless Networks, each network connection atempt is recorded in the registry
  - HKLM\Software\Microsoft\Windows NT\CurrentVersion\NetworkList\Profile

#### Useful Linux Directories

- /etc
  - Contains primary system configuration, subdirectory for most installed apps
- /var/log
  - Plaintext log file location
- /home/$USER
  - Variable name that you should replace with the given name of a user

#### Typical toolbag for forensic analysts

- Jump Bag
  - Prepackaged set of tools taht is always ready to go on no notice, first line of help
  - Helpful to include a checklist to make sure you are ready for each new run
- Write Blockers and Drive Adapters
  - Have an adapter and cable for each type of storage device interface
    - scsi, ata, sata, usb
- Cables
  - Ethernet, serial, power cables, small ethernet hub, antistatic wrist wrap
- Wiped removable media
- Camera
  - Ruler for scale
- Crime Scene Tape
- Tamper Proof Seals
- Documentation and forms
  - Chain of Custody Forms
  - Incident Response Plan
  - Incident Log (notes of current / past investigations)
  - Call / Escalation List

## Chapter Review

1. C. Remove Power from currently running systems
2. D. Chain of custody
3. B. Timeline analysis
4. ~~B. High Level formatting only erases the file data, leaving the system structure intact~~
   1. C. The destination media should be free from anything that may contaminate the evidence. Completely removing the data requires overwriting each block of the destination storage medium, which may always be performed if you're using high-level formatting tools
5. A. Timestomping
6. ~~A. 2048~~
   1. B. The bs argument indicates the number of **bytes** transferring the process. So multiply 8 by 2048 to get 16384
7. D. SDC
8. C. To copy the entire contents of the hard drive to an image file
9. A. Hash all storage media then make a copy of the hard drive
10. B. dd
11. C. Password Cracker
12. A. Faraday bag and a tamper-evident seal
