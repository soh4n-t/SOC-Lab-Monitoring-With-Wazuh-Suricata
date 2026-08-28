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

It will show like :- Agent is now online, continuing....

Start Suricata and alert monitor, then on Kali run :- sudo dhclient -r && sudo dhclient

After it appears in the alert monitor, check whether Wazuh manager received it (in Ubuntu server) :- sudo tail -f /var/ossec/logs/ossec.log

Then check Suricata events stored (in Ubuntu server) :- sudo tail -n 50 /var/ossec/logs/alerts/alerts.json

For verification, run the following cmds:
- sudo grep -i "suricata" /var/ossec/logs/alerts/alerts.json | tail -5
- sudo grep -i "Possible Kali Linux hostname" /var/ossec/logs/alerts/alerts.json | tail -5

