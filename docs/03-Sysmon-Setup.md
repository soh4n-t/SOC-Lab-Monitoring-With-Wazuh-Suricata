## Sysmon Setup
### Installing Sysmon in Windows 10
Download Sysmon officially - https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon
### For Creating a Sysmon Folder
Open Administrator PowerShell and run:
- New-Item -ItemType Directory -Path C:\Sysmon   (Creating directory path)
- Copy-Item "$env:<username>\Downloads\Sysmon64.exe" "C:\Sysmon\Sysmon64.exe"   (Copying sysmon to new path)
- Test-Path C:\Sysmon\Sysmon64.exe   (Verifying path)
- cd C:\Sysmon
- .\Sysmon64.exe -accepteula -i   (Installing Sysmon)
- Get-Service Sysmon64   (Verify Sysmon)
- Get-WinEvent -ListLog "Microsoft-Windows-Sysmon/Operational"   (Verify event log)
- notepad.exe   (Generate sysmon event)  
- close notepad
- Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" -MaxEvents 10 | Select-Object TimeCreated, Id, ProviderName, Message   (Should generate process creation telemetry)


