### Enabling Wazuh archives in Wazuh dashboard (Optional)
Archives are enabled to see events that may not generate a Wazuh alert, allowing raw event investigation in the SOC lab.
On ubuntu-server run :- `sudo cp /var/ossec/etc/ossec.conf /var/ossec/etc/ossec.conf.backup`

Open configuration :- `sudo nano /var/ossec/etc/ossec.conf`
```
<global>  
    <jsonout_output>yes</jsonout_output>   
    <alerts_log>yes</alerts_log>
    <logall>no</logall>   
    <logall_json>no</logall_json>    
    ...
    
</global>
```
In the configuration, change those two "no" shown here in the code block to "yes"

Validate configuration :- `sudo /var/ossec/bin/wazuh-analysisd -t`

Restart Wazuh manager :- `sudo systemctl restart wazuh-manager`

Verify status :- `sudo systemctl status wazuh-manager`

Verify the archives :- `sudo grep -A5 -B2 "archives:" /etc/filebeat/filebeat.yml`

If archives: enabled is 'false', run :- `sudo nano /etc/filebeat/filebeat.yml`

And find:
```
filebeat.modules:
    module: wazuh    
    alerts:    
      enabled: true   
    archives:    
      enabled: false
```  
Then change;
```
archives:    
  enabled : true
```
Test filebeat configuration :- `sudo filebeat test config`

Test connection to Wazuh indexer :- `sudo filebeat test output`

Restart filebeat :-` sudo systemctl restart filebeat`

Check status :- `sudo systemctl status filebeat `

Verify archives are enabled :- `grep -A5 -B2 "archives:" /etc/filebeat/filebeat.yml`


### Create archives index in Wazuh
In the Wazuh dashboard, go to  `☰ -> Dashboard Management -> Index Patterns -> Create Index Pattern -> Enter wazuh-archives-*`

Then for the timefield select timestamp and create it.

Go to `☰ -> Explore -> Discover ; there you can select :- wazuh-archives-*`
