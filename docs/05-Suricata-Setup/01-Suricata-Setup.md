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

Update :- python "C:\Users\Name\AppData\Local\Python\pythoncore-3.14-64\Scripts\suricata-update" --version

Run :- python "C:\Users\Name\AppData\Local\Python\pythoncore-3.14-64\Scripts\suricata-update" --suricata "C:\Program Files\Suricata\suricata.exe"

If certificate verify failed; run :- python "C:\Users\Name\AppData\Local\Python\pythoncore-3.14-64\Scripts\suricata-update" --suricata "C:\Program Files\Suricata\suricata.exe" --no-check-certificate

If it show failed to copy files, create a temp folder :- New-Item -ItemType Directory -Force C:\SuricataRules

Then download the rules directly :- Invoke-WebRequest -Uri "https://rules.emergingthreats.net/open/suricata-8.0.6/emerging.rules.tar.gz" -OutFile "C:\SuricataRules\emerging.rules.tar.gz"

Extract rules :- tar -xzf "C:\SuricataRules\emerging.rules.tar.gz" -C "C:\SuricataRules"

Copy rules :- Copy-Item "C:\SuricataRules\rules\*" "C:\Program Files\Suricata\rules\" -Force

Test Suricata :- & "C:\Program Files\Suricata\suricata.exe" -T -c "C:\Program Files\Suricata\suricata.yaml"

If there is error in emerging-icmp_info.rules; run :- Test-Path "C:\Program Files\Suricata\rules\emerging-icmp_info.rules"

Open suricata.yaml :- notepad "C:\Program Files\Suricata\suricata.yaml"

Search for  emerging-icmp_info.rules and change it to :- '  # - emerging-icmp_info.rules  '
### Running Suricata
In Administrator PowerShell, run;

Find Windows network adaptor :-  Get-NetAdapter | Select-Object Name, InterfaceGuid, Status

Run Suricata :-  & "C:\Program Files\Suricata\suricata.exe" -c "C:\Program Files\Suricata\suricata.yaml" -i "\Device\NPF_{InterfaceGuid}"

