Absolutely — here is a **clean, minimal, well-formatted README** that looks good when copied into GitHub.
No clutter, no repetition, clear headings, and professional formatting.

You can **copy–paste this directly**.

---

# Server Monitoring Agent

## Overview

This repository is used on individual servers to send system metrics and logs to a centralized monitoring server. It runs **Node Exporter** for exposing system metrics and **Promtail** for collecting and forwarding logs. Both services are managed using **Docker Compose**, with no manual binary installation required.

---

## Directory Structure

```
NewGrafanaServer/
├── docker-compose.yml
└── promtail.yml
```

---

## Prerequisites

* Linux server (Ubuntu recommended)
* Docker
* Docker Compose plugin
* Network access to the monitoring server

  * Port `9100` (Node Exporter)
  * Port `3100` (Loki)

---

## Setup Instructions

### 1. Install Docker & Docker Compose

```bash
sudo apt update
sudo apt install -y docker.io docker-compose-plugin
sudo systemctl enable docker
sudo systemctl start docker
```

---

### 2. Download the Repository

```bash
git clone <server-repo-url>
cd NewGrafanaServer
```

Ensure both `docker-compose.yml` and `promtail.yml` are present in this directory.

---

### 3. Configure Promtail

Edit `promtail.yml` and set the Loki endpoint to the monitoring server:

```bash
nano promtail.yml
```

```yaml
clients:
  - url: http://<MONITORING_PC_IP>:3100/loki/api/v1/push
```

Replace `<MONITORING_PC_IP>` with the actual IP or hostname.

---

### 4. Start the Services

```bash
docker compose up -d
```

This command automatically downloads and starts:

* **Node Exporter** (exposes metrics on port `9100`)
* **Promtail** (collects logs from `/var/log`)

---

### 5. Verify

```bash
docker compose ps
```

Check Promtail logs:

```bash
docker compose logs -f promtail
```

---

## Verification (Monitoring Server)

* Metrics:

  ```
  http://<SERVER_IP>:9100/metrics
  ```
* Logs (Grafana → Explore → Loki):

  ```logql
  {job="koha"}
  ```

---

## Stop the Agent

```bash
docker compose down
```

---

## Notes

* Deploy this setup **once per server**
* Do not run this repository on the monitoring server
* No manual Node Exporter installation is required

---

