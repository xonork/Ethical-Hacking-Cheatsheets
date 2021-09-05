# Information Gathering

## **Passive Information Gathering:**

- Google Hacking: *site*, *filetype*, *inurl*, *intitle*
- [Google Hacking (GHDB)](https://www.exploit-db.com/google-hacking-database)
-  Email Harvesting
	```sql
	theHarvester -d cisco.com -b google
	```
- Netcraft
- whois
- recon-ng

### Security Headers

- [Security Headers](https://securityheaders.com/)
###  **Github:**

- Gitrob
- Gitleacks
- recon-ng \[Most of the modules related with GitHub require an access token\]

## **Active Information Gathering:**
- **DNS Enum:**
	```bash
	host -t [QUERY TYPE] evilcorp.com
	```
	
	```bash
	host -l <domain-name> <dns-server-ip>
	```

	```bash
	dnsrecon -d evilcorp.com -t axfr
	```
	
	```bash
	dnsenum evilcorp.com
	```
	
