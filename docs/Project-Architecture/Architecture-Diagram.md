```
                         KALI LINUX
                      Security Testing
                              │
                              │
                    Network / Test Traffic
                              │
                              ▼
┌───────────────────────────────────────────────────────────────┐
│                         WINDOWS 10 VM                         │
│                       Monitored Endpoint                      │
│                                                               │
│    ┌──────────────┐                ┌───────────────┐          │
│    │    Sysmon    │                │   Suricata    │          │
│    │              │                │  Network IDS  │          │
│    │   Endpoint   │                │               │          │
│    │  Telemetry   │                │   Detection   │          │
│    └──────┬───────┘                └───────┬───────┘          │
│           │                                │                  │
│           │                                ▼                  │
│           │                          ┌───────────┐            │
│           │                          │ eve.json  │            │
│           │                          └─────┬─────┘            │
│           │                                │                  │
│           │                 ┌──────────────┴──────────┐       │
│           │                 │                         │       │
│           │                 ▼                         ▼       │
│           │        Python Alert Monitor          Wazuh Agent  │
│           │                 │                         │       │
│           │                 ▼                         │       │
│           │           soc_alerts.db                   │       │
│           │                                           │       │
└───────────┼───────────────────────────────────────────┼───────┘
            │                                           │
            │ Sysmon Events                             │
            │                                           │ Suricata Events
            │                                           │
            └──────────────────────┬────────────────────┘
                                   ▼
                        ┌─────────────────────┐
                        │    UBUNTU SERVER    │
                        │                     │
                        │    Wazuh Manager    │
                        │          │          │
                        │          ▼          │
                        │   Wazuh Dashboard   │
                        └──────────┬──────────┘
                                   │
                                   │ Dashboard accessed through
                                   │ host system's browser
                                   ▼
                        ┌─────────────────────┐
                        │     HOST  SYSTEM    │
                        │                     │
                        │     Web Browser     │
                        │          │          │
                        │          ▼          │
                        │   Wazuh Dashboard   │
                        └─────────────────────┘
```
