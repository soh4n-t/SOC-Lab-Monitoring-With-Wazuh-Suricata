### Connect Windows to Wazuh
In the Wazuh dashboard, go to ` ☰ -> Agents management -> Summary -> Deploy new agent `

Select Windows -> MSI 32/64 bits

In the server address box, enter the `ubuntu-ip`

Give a suitable agent name like `WIN10-ENDPOINT`

Run the Wazuh generated command in the Windows 10 vm, it will download the Wazuh agent

Then start the agent:

`NET START Wazuh`

Verify the Windows service:

`Get-Service Wazuh`

Check Wazuh dashboard you will see `WIN10-ENDPOINT` as Active
