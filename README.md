# TCP-SYN-scan-using-Zenmap-Nmap-GUI-
What is the project about:
Learn to discover open ports on devices in your local network to understand
network exposure.
Sources used:
1) Zenmap(NMap GUI)
2) Command prompt
3) NPcap installer

What i did in the project:
Installed Nmap
Got Nmap from the official site and installed it on my machine.

Found my local IP range using command prompt with command #ipconfig#

Ran a TCP SYN scan
Command used: #nmap -p- REDACTED_IP_ADDRESS#
This scanned the whole subnet quickly to find live hosts and open TCP ports.
Recorded hosts and open ports in a file.
Noted each responding IP and the open ports on that IP (for example: 192.168.1.0.0 — 22, 80, 445, 3306, ...).

Researched common services on those ports
For each open port, I checked what service typically runs on it
The common services are : 
135-Msrpc(Microsoft Remote Procedure Call): it does remote management in windows and inter-process communication
137-NetBIOS-NS(Name Service) : Used for local name resolution in older windows networks before DNS became the standard
139-NetBIOS-SSN(Session Service) : Enables File/printer sharing over NetBIOS
445-Microsoft-DS(Directory Services) :SMB over TCP(modern file and printer sharing). Also used for active Directory communication
80-HTTP(Hypertext Transfer Protocol): Standard Web traffic protocol.


Where needed I ran service/version detection: #nmap -sS -sV --script "default,banner,vuln,smb-os-discovery,smb-enum-shares,http-enum,mysql-info" -p 135,137,139,445,3306,5040,5357,7070,7680,33060,49664-49669 REDACTED_IP_ADDRESS#

Identified potential security risks
For each service I noted simple risks 
Potential risks from the Open ports
1)msrpc: can leak info about system services, used by DCOM exploits(blaster worms. Should not be exposed to internet.
2) NetBIOS-NS: Leaks hostname, domain info. Rarely needed;disable externally.
3)NetBIOS-SSn: legacy SMB sharing; vulnerable to password brute force and malware propagation. Disable it unless needed.
4)Microsoft-DS: Target for ransomware, should never be open to the internet.

I prioritized by exposure (public vs internal) and by ease of exploit.
Saved scan results to files


