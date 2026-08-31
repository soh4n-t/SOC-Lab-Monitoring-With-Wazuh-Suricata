### Create a Sysmon Configuration File
In Administrator PowerShell, run:

`notepad C:\Sysmon\sysmonconfig.xml`

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

    <DnsQuery onmatch="include">
      <Image condition="end with">.exe</Image>
    </DnsQuery>

    <RegistryEvent onmatch="include">
      <Image condition="end with">.exe</Image>
    </RegistryEvent>

    <CreateRemoteThread onmatch="include">
      <SourceImage condition="end with">.exe</SourceImage>
    </CreateRemoteThread>

  </EventFiltering>
</Sysmon>
```
Then update the configuration :- `C:\Sysmon\Sysmon64.exe -c C:\Sysmon\sysmonconfig.xml`

