## Wazuh Setup
### Installing Wazuh in Ubuntu Server
```
- wget https://packages.wazuh.com/4.14/wazuh-install.sh
- sudo bash ./wazuh-install.sh -a
 
After downloading you will get a username and password, store it somewhere.
It is needed for accessing wazuh dashboard in the host system.
```
### Check Wazuh Services
```
- sudo systemctl status wazuh-manager.service
- sudo systemctl status wazuh-indexer.service
- sudo systemctl status wazuh-dashboard.service
Once Wazuh services are active; in the host system, open browser and enter https://ubuntu-ip.
```
### Connect Windows to Wazuh
```
In the Wazuh dashboard, go to Agents management -> Summary -> Deploy new agent
Select Windows -> MSI 32/64 bits
In the server address box, enter the ubuntu-ip
Give a suitable agent name like WIN10-ENDPOINT
Run the Wazuh generated command in the Windows 10 vm, it will download the Wazuh agent
Then start the agent:
- NET START Wazuh
Verify the Windows service:
- Get-Service Wazuh
Check Wazuh dashboard you will see WIN10-ENDPOINT as Active
```
