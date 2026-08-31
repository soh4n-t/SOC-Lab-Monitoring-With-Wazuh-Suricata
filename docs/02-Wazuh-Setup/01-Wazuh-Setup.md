## Wazuh Setup
### Installing Wazuh in Ubuntu Server
` wget https://packages.wazuh.com/4.14/wazuh-install.sh `
` sudo bash ./wazuh-install.sh -a `

After downloading you will get a username and password, store it somewhere.

It is needed for accessing wazuh dashboard in the host system.


### Check Wazuh Services
  - sudo systemctl status wazuh-manager.service
  - sudo systemctl status wazuh-indexer.service
  - sudo systemctl status wazuh-dashboard.service

Once Wazuh services are active; in the host system, open browser and enter https://ubuntu-ip.
