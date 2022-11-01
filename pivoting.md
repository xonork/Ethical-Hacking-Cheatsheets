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
### Windows
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
