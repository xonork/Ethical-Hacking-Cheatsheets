# Enumeration 

## **General Enumeration:**
- Verbose, syn, all ports, all scripts, no ping

	```sql
	nmap -vv -Pn -A -sC -sS -T 4 -p- 10.0.0.0
	```

- Verbose, SYN Stealth, Version info, and scripts against services.

	```sql
	nmap -v -sS -A -T4 x.x.x.x
	```
- Enum all TCP ports (lot ofo noise, preferably -sS).
	```sql
	nmap -sT -sS -p- --min-rate 5000 -oA nmap/alltcp 10.10.10.76
	```
- UDP and TCP SYN scan
	```sql
	nmap -sU -sS 10.10.10.76
	```

```sql
nmap -sC -v -sV -oA nmap/x x.x.x.x
```
-  Nmap script to scan for vulnerable SMB servers – WARNING: unsafe=1 may cause knockover

	```sql
	nmap –script smb-check-vulns.nse –script-args=unsafe=1 -p445 \[host\]
	```

- Network Sweeping

	```bash
	nmap -sn 10.11.1.1-254 -oG greppable-output.txt
	```

## **FTP Enumeration(21):**
```sql
    nmap –script ftp-anon,ftp-bounce,ftp-libopie,ftp-proftpd-backdoor,ftp-vsftpd-backdoor,ftp-vuln-cve2010-4221,tftp-enum -p 21 10.0.0.1
```


## **SMTP Enumeration (25):**
#### **The protocol supports commands such as VRFY and EXPN**
 ```bash
    nmap –script smtp-commands,smtp-enum-users,smtp-vuln-cve2010-4344,smtp-vuln-cve2011-1720,smtp-vuln-cve2011-1764 -p 25 10.0.0.1
 ```

```bash
    nc -nvv INSERTIPADDRESS 25
```

 ```bash
    telnet INSERTIPADDRESS 25
 ```


## **Finger Enumeration (79):**
[http://pentestmonkey.net/tools/user-enumeration/finger-user-enum](http://pentestmonkey.net/tools/user-enumeration/finger-user-enum)


## **Web Enumeration (80/443):**
- **Burp Suite**
- **nikto**
	```sql
	nikto -h x.x.x.x
	```
- **dirbuster**
- **gobuster**
- **httprobe**
- **Automating the Enumeration Process**
	- https://github.com/Gr1mmie/sumrecon
	- https://pastebin.com/MhE6zXVt
- **Nmap**
	- *banner-plus.nse script*
	
	```bash
	nmap --script /usr/share/nmap/scripts/banner-plus.nse -min-rate=400 --min-parallelism=512 -p1-65535 -n -Pn -PS -oA /opt/peepingtom/report <IP CIDR>
	```
			
- **PeepingTom**
	 - *Banner-plus.nse script*
	```sql
	cat report.gnmap | ./gnamp.pl | grep http | cut -f 1,2 -d "," | tr "," 	":" > http_ips.txt
	```
			
- **Whois**
- **nslookup**
- **dnsrecon**
- **Google Fu**
- **dig**
- **Sublist3r**
- **Bluto**
- **[crt.sh](https://crt.sh/)**
- **[Discover Scripts]( https://github.com/leebaird/discover)**:
	- SubDomains
		1.  Type 1 (Passive), and 1 (Domain)
		2.  Type the domain we want to search fot
		3.  The gathered information will be stored in /root/DOMAIN/
		4.  We can take a look to the result typing:
			- **firefox /root/DOMAIN/index.html**
- **Recon-ng**
- **[Hunter.io](https://hunter.io/)**
- **breach-parse**
- **theHarvester**
- **OWASP Amass**
- **whatweb**
- **[wappalyzer](https://addons.mozilla.org/es/firefox/addon/wappalyzer)**
- **Assetfinder**
- **wpscan**


## **Pop3 (110):**

- **To login**
	```sql
		telnet INSERTIPADDRESS 110
	```

	```sql
		USER \[username\]
	```

	```sql
	   PASS \[password\]
	```

- **To list messages**
	```sql
	   LIST
	```

- **Retrieve message**
	```sql
	   RETR \[message number\]
	```

- **Quits**
	```sql
	   QUIT
	```

## **SMB Enumeration (139/445):**

```sql
enum4linux –a 10.0.0.1
```

- Discover Windows / Samba servers on subnet, finds Windows MAC addresses, netbios name and discover client workgroup / domain
	```sql
	nbtscan x.x.x.x
	```


```sql
py 192.168.XXX.XXX 500 50000 dict.txt
```

```sql
python /usr/share/doc/python-impacket-doc/examples/samrdump.py 192.168.XXX.XXX
```

```bash
nmap IPADDR --script smb-enum-domains.nse,smb-enum-groups.nse,smb-enum-processes.nse,smb-enum-sessions.nse,smb-enum-shares.nse,smb-enum-users.nse,smb-ls.nse,smb-mbenum.nse,smb-os-discovery.nse,smb-print-text.nse,smb-psexec.nse,smb-security-mode.nse,smb-server-stats.nse,smb-system-info.nse,smb-vuln-conficker.nse,smb-vuln-cve2009-3103.nse,smb-vuln-ms06-025.nse,smb-vuln-ms07-029.nse,smb-vuln-ms08-067.nse,smb-vuln-ms10-054.nse,smb-vuln-ms10-061.nse,smb-vuln-regsvc-dos.nse
```

- List open shares
	```sql
	smbclient -L //INSERTIPADDRESS/
	```

```sql
smbclient //INSERTIPADDRESS/ipc$ -U john
```

##  NFS Enumeration (111)
```bash
nmap -sV -p 111 --script=rpcinfo 10.10.10.2
```

```bash
nmap -p 111 --script nfs* 10.10.10.2
```

```bash
rpcinfo –p x.x.x.x   
```

- Mount shared directories
	```bash
	sudo mount -o nolock 10.10.10.2:/home ~/new-folder
	```
### **SNMP Enumeration (161):**
```bash
sudo nmap -sU --open -p 161 10.10.10.1 -oG open-snmp.txt
```

```bash
onesixtyone -c [snmp community strings worlist] -i [Ip's list]
```

```sql
snmpwalk -c [specify the community string] -v1 -t 10 10.0.0.2
```

- Enumerate Windows users
	```bash
	snmpwalk -c public -v1 10.10.10.2 1.3.6.1.4.1.77.1.2.25
	#The las string provided is a SNMP OID
	```
- Enumerate Running WIndows Processes
	```bash
	snmpwalk -c public -v1 10.10.10.2 1.3.6.1.2.1.25.4.2.1.2
	```
- Enumerate Open TCP Ports
	```bash
	snmpwalk -c public -v1 10.10.10.2 1.3.6.1.2.1.6.13.1.3
	```
- Enumerate Installed Software
	```bash
	snmpwalk -c public -v1 10.10.10.2 1.3.6.1.2.1.25.6.3.1.2
- Enumerate OIDs available in a host
	```bash
	snmpwalk -v 2c -c public 10.10.10.2
	```
```sql
snmpcheck -t 192.168.1.X -c public
```
    
```sql
onesixtyone -c names -i hosts
```

```sql
snmpenum -t 192.168.1.X
```


## **Oracle (1521):**

```sql
tnscmd10g version -h INSERTIPADDRESS
```
    
```sql
tnscmd10g status -h INSERTIPADDRESS
```


### **Mysql Enumeration (3306):**

```sql
nmap -sV -Pn -vv  10.0.0.1 -p 3306 --script mysql-audit,mysql-databases,mysql-dump-hashes,mysql-empty-password,mysql-enum,mysql-info,mysql-query,mysql-users,mysql-variables,mysql-vuln-cve2012-2122
```


### **DNS Zone Transfers:**

```sql
nslookup -> set type=any -> ls -d blah.com
```

```sql
dig axfr blah.com @ns1.blah.com
```

```sql
dnsrecon -d TARGET -D /usr/share/wordlists/dnsmap.txt -t std --xml ouput.xml
```


## **Mounting File Share**

```sql
 showmount -e IPADDR
```

-   mounts the share to /mnt/nfs without locking it
	```sql
	mount 192.168.1.1:/vol/share /mnt/nfs  -nolock
	```

-   Mount Windows CIFS / SMB share on Linux at /mnt/cifs if you remove password it will prompt on the CLI (more secure as it wont end up in bash\_history)
	```sql
	mount -t cifs -o username=user,password=pass,
	```

-   Mount a Windows share on Windows from the command line
	```sql
	net use Z: \\win-server\share password  /user:domain\janedoe /savecred /p:no
	```
	
-   Install smb4k on Kali, useful Linux GUI for browsing SMB shares
	```SQL
	apt-get install smb4k –y
	```


## **Fingerprinting:  Basic versioning / finger printing via displayed banner**

 ```SQL
nc -v 192.168.1.1 25
 ```

```sql
telnet 192.168.1.1 25
```


## **Exploit Research**

```sql
searchsploit windows 2003 | grep -i local
```


## **Compiling Exploits**

-   Compile C code, add –m32 after ‘gcc’ for compiling 32 bit code on 64 bit Linux
	```sql
	gcc -o exploit exploit.c
	```
	
-   Compile windows .exe on Linux
	```sql
	i586-mingw32msvc-gcc exploit.c -lws2_32 -o exploit.exe
	```


## **Packet Inspection:**

-   tcpdump for port 80 on interface eth0, outputs to output.pcap
	```sql
	tcpdump tcp port 80 -w output.pcap -i eth0
	```
