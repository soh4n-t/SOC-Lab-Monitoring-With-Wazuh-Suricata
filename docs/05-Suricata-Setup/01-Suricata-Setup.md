## Suricata Setup
### Installing Suricata for Windows 10 VM
Download and install from the official page :- https://suricata.io/download

We need Npcap along with Suricata as it uses Npcap for packet capture.

### Installing Npcap
Download from :- https://npcap.com/#download

Make sure to install Npcap in WinPcap API-compatible-Mode

### Suricata update
Install Python :- https://www.python.org/downloads/release/pymanager-263/

In Administrator PowerShell;

Install suricata-update :- pip install suricata-update

Verify :- python "C:\Users\Name\AppData\Local\Python\pythoncore-3.14-64\Scripts\suricata-update" --version

Update :- python "C:\Users\Name\AppData\Local\Python\pythoncore-3.14-64\Scripts\suricata-update"

Run :- python "C:\Users\Name\AppData\Local\Python\pythoncore-3.14-64\Scripts\suricata-update" --suricata "C:\Program Files\Suricata\suricata.exe"

If certificate verify failed; run :- python "C:\Users\Name\AppData\Local\Python\pythoncore-3.14-64\Scripts\suricata-update" --suricata "C:\Program Files\Suricata\suricata.exe" --no-check-certificate

If it show failed to copy files, create a temp folder :- New-Item -ItemType Directory -Force C:\SuricataRules

Then download the rules directly :- Invoke-WebRequest -Uri "https://rules.emergingthreats.net/open/suricata-8.0.6/emerging.rules.tar.gz" -OutFile "C:\SuricataRules\emerging.rules.tar.gz"

Extract rules :- tar -xzf "C:\SuricataRules\emerging.rules.tar.gz" -C "C:\SuricataRules"

Copy rules :- Copy-Item "C:\SuricataRules\rules\*" "C:\Program Files\Suricata\rules\" -Force

### Running Suricata
In Administrator PowerShell, run;

Test run Suricata :- & "C:\Program Files\Suricata\suricata.exe" -T -c "C:\Program Files\Suricata\suricata.yaml"

If there is an error like ET EXPLOIT 7-Zip 7z File PPMd Properties Parsing Integer Underflow

Make a backup :- Copy-Item "C:\Program Files\Suricata\rules\emerging-exploit.rules" "C:\Program Files\Suricata\rules\emerging-exploit.rules.bak"

Then open the rule :- notepad "C:\Program Files\Suricata\rules\emerging-exploit.rules"

Go to line(Ctrl G) 4073 & put # at the beginning of that line then save it.

Test Suricata again.

Find Windows network adaptor :-  Get-NetAdapter | Select-Object Name, InterfaceGuid, Status

Run Suricata :-  & "C:\Program Files\Suricata\suricata.exe" -c "C:\Program Files\Suricata\suricata.yaml" -i "\Device\NPF_{InterfaceGuid}"

Keep it running, then  in another Administrator PowerShell;

Create a test rule :- notepad "C:\SuricataRules\test.rules"

Paste this & save it :- alert icmp any any -> any any (msg:"TEST ICMP ALERT"; sid:1000001; rev:1;)  

Make a folder :- mkdir C:\SOC_Project & Enter the folder :- cd C:\SOC_Project

Create a python file :- notepad alert_monitor.py & paste the following code in it:
```
import json
import time
import sqlite3

LOG_FILE = r"C:\Program Files\Suricata\log\eve.json"
DB_FILE = r"C:\SOC_Project\soc_alerts.db"

# Create database
conn = sqlite3.connect(DB_FILE)
cursor = conn.cursor()

cursor.execute("""
CREATE TABLE IF NOT EXISTS alerts (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    timestamp TEXT,
    source_ip TEXT,
    source_port INTEGER,
    destination_ip TEXT,
    destination_port INTEGER,
    protocol TEXT,
    signature TEXT,
    signature_id INTEGER,
    severity INTEGER,
    action TEXT
)
""")

conn.commit()

print("=" * 60)
print("           SOC ALERT MONITOR")
print("=" * 60)
print("Suricata log :", LOG_FILE)
print("Database     :", DB_FILE)
print("Status       : Monitoring...")
print()

with open(LOG_FILE, "r", encoding="utf-8", errors="ignore") as file:

    # Start at the end of the existing log
    file.seek(0, 2)

    while True:

        line = file.readline()

        if not line:
            time.sleep(0.5)
            continue

        try:
            event = json.loads(line)

            if event.get("event_type") != "alert":
                continue

            alert = event.get("alert", {})

            timestamp = event.get("timestamp")
            source_ip = event.get("src_ip")
            source_port = event.get("src_port")
            destination_ip = event.get("dest_ip")
            destination_port = event.get("dest_port")
            protocol = event.get("proto")
            signature = alert.get("signature")
            signature_id = alert.get("signature_id")
            severity = alert.get("severity")
            action = alert.get("action")

            # Store alert
            cursor.execute("""
            INSERT INTO alerts (
                timestamp,
                source_ip,
                source_port,
                destination_ip,
                destination_port,
                protocol,
                signature,
                signature_id,
                severity,
                action
            )
            VALUES (?, ?, ?, ?, ?, ?, ?, ?, ?, ?)
            """, (
                timestamp,
                source_ip,
                source_port,
                destination_ip,
                destination_port,
                protocol,
                signature,
                signature_id,
                severity,
                action
            ))

            conn.commit()

            # Display alert
            print("=" * 60)
            print("🚨 SOC ALERT")
            print("=" * 60)
            print("Time        :", timestamp)
            print("Source IP   :", source_ip)
            print("Source Port :", source_port)
            print("Destination :", destination_ip)
            print("Dest Port   :", destination_port)
            print("Protocol    :", protocol)
            print("Signature   :", signature)
            print("Signature ID:", signature_id)
            print("Severity    :", severity)
            print("Action      :", action)
            print("Database    : SAVED")
            print()

        except json.JSONDecodeError:
            continue
```
Then run it :- python C:\SOC_Project\alert_monitor.py

Test it with an icmp test from the Kali VM, the alert monitor will display an alert
