## Connecting Sysmon to Wazuh
Open Administrator PowerShell:

- Run :- notepad "C:\Program Files (x86)\ossec-agent\ossec.conf"
- Find the existing :- <ossec_config>
- Add the following code block in it (dont delete the existing configuration, just add this inside <ossec_config>)
```
<localfile>
  <location>Microsoft-Windows-Sysmon/Operational</location>
  <log_format>eventchannel</log_format>
</localfile>

<localfile>
  <location>Microsoft-Windows-Powershell/Operational</location>
  <log_format>eventchannel</log_format>
</localfile>
```
- Restart Wazuh agent :- Restart-Service Wazuh
- Verify :- Get-Service WazuhSvc
- Sysmon generate detailed Windows telemetry. The Wazuh Agent collects these Sysmon events and forwards them to the Wazuh Manager, where Wazuh's rules can process them.
### Generate a test event
- Run notepad.exe and close it
- Run :- powershell.exe -Command "Get-Process"
- Verify locally :- Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" -MaxEvents 5 | Select-Object TimeCreated, Id, ProviderName
- In the Wazuh dashboard, go to ☰ -> Explore -> Discover -> Select wazuh-archives-*
- Search :- agent.name:"WIN10-ENDPOINT" AND data.win.system.channel:"Microsoft-Windows-Sysmon/Operational"
- You will see those events
- Using > , you can see details  
