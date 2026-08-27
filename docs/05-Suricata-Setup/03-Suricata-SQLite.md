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
