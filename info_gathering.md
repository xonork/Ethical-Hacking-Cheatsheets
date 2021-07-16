# Information Gathering

## **Passive Information Gathering:**

- Google Hacking: *site*, *filetype*, *inurl*, *intitle*
- [Google Hacking (GHDB)]([https://www.exploit-db.com/google-hacking-database)
-  Email Harvesting
	```sql
	theHarvester -d cisco.com -b google
	```
- Netcraft
- whois
- recon-ng

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
	
