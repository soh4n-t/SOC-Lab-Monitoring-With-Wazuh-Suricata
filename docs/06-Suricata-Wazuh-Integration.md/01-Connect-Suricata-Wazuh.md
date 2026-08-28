### Connect Suricata & Wazuh
In Administrator PowerShell;

Open ossec.conf :- notepad "C:\Program Files (x86)\ossec-agent\ossec.conf"

In the notepad find the <ossec_config> and add the following block inside it:
```
<localfile>
  <location>C:\Program Files\Suricata\log\eve.json</location>
  <log_format>json</log_format>
</localfile>
```
Save the configuration and restart the Wazuh agent :- Restart-Service WazuhSvc

Verify connection with Wazuh manager :- Test-NetConnection 192.168.198.129 -Port 1514

Then check the Wazuh agent log :- Get-Content "C:\Program Files (x86)\ossec-agent\ossec.log" -Tail 50

It will show like :- <img width="833" height="673" alt="image" src="https://github.com/user-attachments/assets/9955fec4-e9e5-407c-94af-e61ae0d590c7" />
