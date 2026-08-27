### Storing Suricata Alerts in SQLite
Replace alerts_monitor.py with the following code:
```
import json
import time
import os
import sqlite3

LOG_FILE = r"C:\Program Files\Suricata\log\eve.json"
DB_FILE = r"C:\SOC_Project\soc_alerts.db"


# --------------------------------------------------
# SQLite Database Setup
# --------------------------------------------------

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
print("             SURICATA ALERT MONITOR")
print("=" * 60)
print(f"Monitoring: {LOG_FILE}")
print(f"Database  : {DB_FILE}")
print("Waiting for new alerts...")
print("=" * 60)


# --------------------------------------------------
# Monitor eve.json
# --------------------------------------------------

with open(LOG_FILE, "r", encoding="utf-8", errors="ignore") as f:

    # Start from the current end of the file
    f.seek(0, os.SEEK_END)

    while True:

        line = f.readline()

        if not line:
            time.sleep(0.2)
            continue

        try:

            event = json.loads(line)

            # Ignore everything except Suricata alerts
            if event.get("event_type") != "alert":
                continue

            alert = event.get("alert", {})

            # Extract alert information
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

            # --------------------------------------------------
            # Display Alert
            # --------------------------------------------------

            print()
            print("=" * 60)
            print("🚨 ALERT DETECTED")
            print("=" * 60)

            print("Timestamp    :", timestamp)
            print("Source IP    :", source_ip)
            print("Source Port  :", source_port)
            print("Destination  :", destination_ip)
            print("Dest Port    :", destination_port)
            print("Protocol     :", protocol)
            print("Signature    :", signature)
            print("Signature ID :", signature_id)
            print("Severity     :", severity)
            print("Action       :", action)

            print("=" * 60)

            # --------------------------------------------------
            # Store Alert in SQLite
            # --------------------------------------------------

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

            print("✅ Alert stored in SQLite")

        except json.JSONDecodeError:
            continue

        except Exception as e:
            print("Monitor error:", e)
```            

Then running alert_monitor.py will create the soc_alerts.db database to store alerts

### Verifying Database Storage
In PowerShell, run these commands seperately:
```
python
import sqlite3
conn = sqlite3.connect(r"C:\SOC_Project\soc_alerts.db")   :- connect to our database
cursor.execute("SELECT COUNT(*) FROM alerts").fetchone()  :- see how many alerts we have
cursor.execute("SELECT * FROM alerts").fetchall()         :- display the stored alerts
```

### SOC Investigation Queries
```
Identify how many times each alert occured :- cursor.execute("SELECT signature, COUNT(*) FROM alerts GROUP BY signature").fetchall()
