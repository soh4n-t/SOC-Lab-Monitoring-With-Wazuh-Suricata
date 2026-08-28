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

It will show like :- <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/f32a263b-d558-458e-9a8c-756e273a7b6e" />
