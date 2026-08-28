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

If certificate verify fails; run :- python "C:\Users\Name\AppData\Local\Python\pythoncore-3.14-64\Scripts\suricata-update" --suricata "C:\Program Files\Suricata\suricata.exe" --no-check-certificate

If it shows failed to copy files, create a temp folder :- New-Item -ItemType Directory -Force C:\SuricataRules

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
