                         KALI LINUX
                    Security Testing
                           │
                           │
                  Network / Test Traffic
                           │
                           ▼
┌───────────────────────────────────────────────────────────────┐
│                       WINDOWS 10 VM                           │
│                     Monitored Endpoint                        │
│                                                               │
│    ┌──────────────┐                    ┌───────────────┐     │
│    │    Sysmon    │                    │   Suricata    │     │
│    │              │                    │   Network IDS │     │
│    │ Endpoint     │                    │               │     │
│    │ Telemetry    │                    │ Detection     │     │
│    └──────┬───────┘                    └───────┬───────┘     │
│           │                                    │             │
│           │                                    ▼             │
│           │                              ┌──────────┐        │
│           │                              │ eve.json │        │
│           │                              └────┬─────┘        │
│           │                                   │              │
│           │                    ┌──────────────┴──────────┐   │
│           │                    │                         │   │
│           │                    ▼                         ▼   │
│           │             Python Alert Monitor       Wazuh Agent│
│           │                    │                         │   │
│           │                    ▼                         │   │
│           │             soc_alerts.db                   │   │
│           │                                             │   │
└───────────┼─────────────────────────────────────────────┼───┘
            │                                             │
            │ Sysmon Events                               │
            │                                             │ Suricata Events
            │                                             │
            └──────────────────────┬──────────────────────┘
                                   ▼
                        ┌─────────────────────┐
                        │    UBUNTU SERVER    │
                        │                     │
                        │   Wazuh Manager     │
                        │          │          │
                        │          ▼          │
                        │   Wazuh Dashboard  │
                        └──────────┬──────────┘
                                   │
                                   │ Accessed through
                                   │ browser
                                   ▼
                        ┌─────────────────────┐
                        │    WINDOWS HOST     │
                        │                     │
                        │     Web Browser     │
                        └─────────────────────┘
