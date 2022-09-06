# Information Gathering

## **Passive Information Gathering:**
- https://hacklejandria.com/osint-tools/: List of interesting OSINT tools.
- Twofi: It makes a custom password list from scanning someone social media account.
### Search Engines

- Google Dorking: *site*, *filetype*, *inurl*, *intitle*
- Yandex: For image reversing
- Bai du
- DuckDuckGo
- Bing
- https://search.carrot2.org/#/search/web
- [Google Hacking (GHDB)](https://www.exploit-db.com/google-hacking-database)
- https://github.com/BullsEye0/dorks-eye
- DarkWeb

### Sock Puppet
- genware.es
- 
### Identify Geographical Locations
- Google Image Search
- Yandex
- https://somerandomstuff1.wordpress.com/2019/02/08/geoguessr-the-top-tips-tricks-and-techniques/

### Physical Location OSINT
- Google Maps

### EXIF Data
- Jeffrey's Image Metadata Viewer
- exiftool

### Reverse Image Searching
- Yandex
- TinEye
- Google
- PinEyes

### Email OSINT
- hunter.io
- https://phonebook.cz/
- https://www.voilanorbert.com/
- Clearbit extension
- https://tools.emailhippo.com/
- https://email-checker.net/
-  Email Harvesting
	```sql
	theHarvester -d cisco.com -b google
	```
### Password OSINT and Leaks
- [bugmenot](http://bugmenot.com)
- intelx.io
- [Pastebin](https://pastebin.ga/)
- breach-parse
- https://pwndb2am4tzkvold.onion *It doesn't work, functional link needede*
- Gitrob
- Gitleacks

### Username OSINT
- https://namechk.com/
- https://whatsmyname.app/
- https://namecheckup.com/

### People OSINT
- Google Dorking
- Twitter
- https://fastbackgroundcheck.com/
- https://webmii.com/
- https://peekyou.com/
- https://thatsthem.com/challenge?r=%2F

### Phone Number OSINT
- Google Dorking
- https://www.truecaller.com
- https://www.infobel.com/

### Social Media OSINT
- [Socia-Searcher](https://www.social-searcher.com)


#### Twitter
- https://help.twitter.com/en/using-twitter/twitter-advanced-search
- - **https://tweetdeck.twitter.com/** (Very good app)
- https://socialbearing.com/
- https://www.twitonomy.com/
- https://mentionmapp.com/
- https://sleepingtime.org/
- https://sleepingtime.org/
- https://spoonbill.io/twitter/data/[user]
- https://tinfoleak.com/

#### Reddit
- https://www.online-tech-tips.com/computer-tips/advanced-search-on-reddit-like-a-pro/

### Webapp OSINT
- Google Dorking
- Google -> Cached Websites 
- Wappalizer
- https://builtwith.com/ (More completed wappalizer)
- https://centralops.net/co/ (WHOIS record, nmap scan..)
- https://dnslytics.com/ (search all the subdomains that has an IP)
- https://spyonweb.com/ (UA recopiled from VirusTotal)
- https://whois.domaintools.com/
- https://www.virustotal.com/gui/home/upload (URL -> Details -Look if there is UA-)
- https://www.reddit.com/domain
- https://visualping.io/
- https://backlinkwatch.com/
- https://heaviside.digital/
- https://viewdns.com/

### Subdomains
- Google Dorking *(site:tesla.com -www)
- https://pentest-tools.com/information-gathering/find-subdomains-of-domain
- https://spyse.com/
- https://crt.sh/ (% is the wildcard; %.tesla.com)
- Netcraft
- Sublist3r

### Network OSINT
- [Shodan](https://www.shodan.io/)

### Server Security

- [Security Headers](https://securityheaders.com/)
- [SSL Server Test](https://www.ssllabs.com/ssltest/)


## **Active Information Gathering:**
- httprobe
### Linkedin
- linkedin2username


### **DNS Enum:**
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
	
