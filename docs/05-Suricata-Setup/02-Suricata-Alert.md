### Testing Suricata Rules
First edit our test.rules , disable it by putting # at the start of the rule :- notepad "C:\Program Files\Suricata\rules\test.rules"

Edit if it exists here :- notepad "C:\SuricataRules\test.rules"

Generate a dhcp traffic from kali :- sudo dhclient -r && sudo dhclient 

It will appear in the alert_monitor.py due to built in Suricata rules
