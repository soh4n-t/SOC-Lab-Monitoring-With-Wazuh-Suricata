### Python Alert Monitor
Run Suricata, then in another Administrator PowerShell;

Create a test rule :- notepad "C:\SuricataRules\test.rules"

Paste this & save it :- alert icmp any any -> any any (msg:"TEST ICMP ALERT"; sid:1000001; rev:1;)  

This test rule is a temporary pipeline validation rule. We disable it later.

Make a folder :- mkdir C:\SOC_Project & Enter the folder :- cd C:\SOC_Project

Create a python file :- notepad alert_monitor.py & paste the following code in it:
```
import json
import time
import os

LOG_FILE = r"C:\Program Files\Suricata\log\eve.json"

print("=" * 60)
print("             SURICATA ALERT MONITOR")
print("=" * 60)
print(f"Monitoring: {LOG_FILE}")
print("Waiting for new alerts...")
print("=" * 60)

# Open the file and start from the current end
with open(LOG_FILE, "r", encoding="utf-8", errors="ignore") as f:

    f.seek(0, os.SEEK_END)

    while True:
        line = f.readline()

        if not line:
            time.sleep(0.2)
            continue

        try:
            event = json.loads(line)

            if event.get("event_type") != "alert":
                continue

            alert = event.get("alert", {})

            print()
            print("=" * 60)
            print("🚨 ALERT DETECTED")
            print("=" * 60)

            print("Timestamp    :", event.get("timestamp"))
            print("Source IP    :", event.get("src_ip"))
            print("Source Port  :", event.get("src_port"))
            print("Destination  :", event.get("dest_ip"))
            print("Dest Port    :", event.get("dest_port"))
            print("Protocol     :", event.get("proto"))
            print("Signature    :", alert.get("signature"))
            print("Signature ID :", alert.get("signature_id"))
            print("Severity     :", alert.get("severity"))
            print("Action       :", alert.get("action"))

            print("=" * 60)

        except json.JSONDecodeError:
            continue
        except Exception as e:
            print("Monitor error:", e)
```
Then run it :- python C:\SOC_Project\alert_monitor.py

Test it with an icmp test from the Kali VM, the alert monitor will display an alert

It means Suricata is generating new alerts and the alert monitor is receiving them.

