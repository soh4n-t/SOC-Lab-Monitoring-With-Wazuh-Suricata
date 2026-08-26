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

### Create a Sysmon Configuration File
In Administrator PowerShell, run:
- notepad C:\Sysmon\sysmonconfig.xml

Paste the following and save it:
```
<Sysmon schemaversion="4.90">
  <HashAlgorithms>SHA256</HashAlgorithms>

  <EventFiltering>

    <FileCreate onmatch="include">
      <Image condition="end with">.exe</Image>
    </FileCreate>

    <ProcessCreate onmatch="include">
      <Image condition="end with">.exe</Image>
    </ProcessCreate>  

    <ProcessTerminate onmatch="include">
      <Image condition="end with">.exe</Image>
    </ProcessTerminate>

    <NetworkConnect onmatch="include">
      <Image condition="end with">.exe</Image>
    </NetworkConnect>

    <CreateRemoteThread onmatch="include">
      <SourceImage condition="end with">.exe</SourceImage>
    </CreateRemoteThread>

    <DnsQuery onmatch="include">
      <Image condition="end with">.exe</Image>
    </DnsQuery>

  </EventFiltering>
</Sysmon>
```
Then update the configuration :- C:\Sysmon\Sysmon64.exe -c C:\Sysmon\sysmonconfig.xml

