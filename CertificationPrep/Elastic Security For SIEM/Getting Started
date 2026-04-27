# Elastic Stack (ELK Stack) — Overview

## Stack Components

The Elastic Stack consists of **3 layers**:

1. **Data Ingestion** — Beats, Agent, Logstash
2. **Storage, Search & Analytics** — Elasticsearch
3. **Visualization & Management** — Kibana

---

## Elasticsearch
- The **heart** of the Elastic Stack
- Responsible for **storing, searching, and analyzing** data
- Fully **scalable** — limited only by available hardware resources
- Handles common **data transformations**
- Compatible with **all data types**

---

## Logstash
- The **original** data ingestion method
- **Server-side** — no agent installation required on endpoints
- Accepts data from any source that can be directed to it
- Handles **data normalization and parsing**
- **200+ plugins** for broad compatibility with data sources

---

## Beats
- **Lightweight agents** installed directly on devices
- Pull and ship relevant data to Elasticsearch (or optionally to Logstash for additional parsing)

### Beat Types

| Beat | Purpose |
|---|---|
| **Filebeat** | Monitors specified log files and ships them |
| **Metricbeat** | Ships system metrics (RAM, CPU usage, etc.) |
| **Packetbeat** | Real-time network packet analyzer |
| **Winlogbeat** | Ships Windows Event Logs |
| **Auditbeat** | Ships Linux audit logs (via Audit Daemon) |
| **Heartbeat** | Monitors service status on servers |

### Most Powerful Beats

**Auditbeat**
- Groups related messages into a **single event**
- Ships with default **audit rules**
- Automatically **maps usernames to Process IDs**
- Efficient **File Integrity Monitoring (FIM)**

**Winlogbeat**
- Parses and ships any **Windows Event Logs**
- Monitors **Active Directory** interactions
- Supports **third-party Windows event logs**
- Native integration with **Sysmon**

---

## Elastic Agent
- The **newest** ingestion method
- Functions like **4 Beats combined** in a single agent
- Can ship to **Elasticsearch directly** or via **Logstash**
- **200+ integrations** available, each adding unique capabilities

---

## Kibana
- The **web-based UI** — the "window into the Elastic Stack"
- Used to:
  - Run **queries**
  - Create **visualizations**
  - **Manage** the stack

---

## Data Flow Summary

```
Data Source (logs, user info, etc.)
        ↓
Ingestion Layer (Beats / Agent / Logstash)
        ↓
Elasticsearch (storage & search)
        ↓
Kibana (query & visualize)
```