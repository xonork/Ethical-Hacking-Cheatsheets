# 📕 Infosec Notes

In this documenht I will post my infosec tools and notes. I will cover the different areas of cibersecurity, including password cracking, obfuscation and exploit development (BOFs for noobies like me).

⚠️ The document is not finished, I have to add a lot more of content. Also, the CyberSecurity area is always evolving, which means that is always generating new knowledge, for this reason this document will never be finished. ⚠️

This document is **NOT** for malicious porpouses.

## 🔍 [Information Gathering](info_gathering.md)
In this section you will find different OSINT tools. This tools are divided in two groups:
- **Passive Information Gathering**: The tools presented in this section collect information without establishing direct communication between yourself and the target
- **Active Information Gathering**: This tools establish communication between you and the target (p.e. querying their DNS) to collect as much information as they can.

## 📡 [Scanning & Enumeration](scan_enum.md)
Here we have different tools and methods that will help us to enumerate the network (*nmap*) or a specific service such as FTP.

## 🚩 [Post Exploitation](post.md)
After we have enumerate, exploit a service and gain control of the target machine, we will need to upload files to the compromised machine for persistance or to make privilege escalation. So, here you will find different techniques to Upload/Download files to/from a target machine.
Also, in this section you will find different tools to detect possible privilege escalation vectors such as *Juicy Potato* in Windows or a vulnerable *cron job* in Linux.

## 🐚 [Shells & Reverse Shells](shells.md)
Here you will find a list of useful reverse shells done with different coding languages. The big part of them are made as *one line*.
Also is explained how to upgrade a shell in Linux (where we will find with this problem a hundreds times).

## 🪲 [Exploit Development](exploit_dev.md)
Very noobie introduction to Windows and Linux Buffer Overflow.

## #️⃣ [Password Cracking](passwd_crack.md)
Password cracking tools. 
## [Wireless Penetration Testing](wireless.md)
(*⚠️ Under construction*)

## [Miscellaneous](misc.md)
