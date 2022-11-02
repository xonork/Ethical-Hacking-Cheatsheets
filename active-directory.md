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
