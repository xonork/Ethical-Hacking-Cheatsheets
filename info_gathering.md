# Information Gathering

## **Passive Information Gathering:**

- Google Hacking: *site*, *filetype*, *inurl*, *intitle*
- [Google Hacking (GHDB)](https://www.exploit-db.com/google-hacking-database)
- intelx.io
- pastebin
-  Email Harvesting
	```sql
	theHarvester -d cisco.com -b google
	```
- Netcraft
- whois
- recon-ng
- [Pastebin](https://pastebin.ga/)
- [Socia-Searcher](https://www.social-searcher.com)
- Twofi
- linkedin2username
### Server Security

- [Security Headers](https://securityheaders.com/)
- [SSL Server Test](https://www.ssllabs.com/ssltest/)
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
- Trying Zone Transfer
	```bash
	dnsrecon -d evilcorp.com -t axfr
	```
- Bruteforcing SubDomains
	```bash
	dnsrecon -d evilcorp.com -D [path to wordlist] -t brt
	```
```bash
dnsenum evilcorp.com
```
	
