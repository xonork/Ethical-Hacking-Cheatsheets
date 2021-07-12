# Password Cracking

```sql
hash-identifier [hash]
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


## **Bruteforcing:**

```sql
    hydra 10.0.0.1 http-post-form “/admin.php:target=auth&mode=login&user=^USER^&password=^PASS^:invalid” -P /usr/share/wordlists/rockyou.txt -l admin
```

```sql
hydra -l admin -P /usr/share/wordlists/rockyou.txt -o results.txt IPADDR PROTOCOL
```

-   Hydra SMTP Brute force
	```sql
	hydra -P /usr/share/wordlistsnmap.lst 192.168.X.XXX smtp –V
	```




