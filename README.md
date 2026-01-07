Server Monitoring Agent (Node Exporter + Promtail)
Overview

This repository is used on application and database servers to send system metrics and logs to a centralized monitoring server. It runs Node Exporter to expose system-level metrics for Prometheus and Promtail to collect logs from the server and push them to Loki. Both services are managed entirely using Docker Compose, with no manual installation of binaries required.

Folder Structure

Ensure the directory layout looks exactly like this:

NewGrafanaServer/
├── docker-compose.yml
└── promtail.yml


docker-compose.yml starts Node Exporter and Promtail

promtail.yml defines which logs are collected and where they are sent

Prerequisites

Linux server (Ubuntu recommended)

Docker installed

Docker Compose plugin installed

Network connectivity to the monitoring server

Port 3100 (Loki)

Port 9100 (Node Exporter)

Installation Steps
1. Install Docker and Docker Compose

Run the following commands on the server:

sudo apt update
sudo apt install -y docker.io docker-compose-plugin
sudo systemctl enable docker
sudo systemctl start docker

2. Download the Agent Repository

Clone the repository or copy the files manually:

git clone <server-repo-url>
cd NewGrafanaServer


If downloading manually, ensure both docker-compose.yml and promtail.yml are inside the NewGrafanaServer directory.

3. Configure Promtail

Edit promtail.yml and confirm the Loki endpoint points to the monitoring server:

nano promtail.yml


Ensure it contains:

clients:
  - url: http://<MONITORING_PC_IP>:3100/loki/api/v1/push


Replace <MONITORING_PC_IP> with the actual IP or hostname of the monitoring server.

4. Start Node Exporter and Promtail

From inside the NewGrafanaServer directory, start both services:

docker compose up -d


This command will:

Automatically download the Node Exporter Docker image

Automatically download the Promtail Docker image

Start both services in the background

No manual installation of Node Exporter is required.

5. Verify the Services

Check that both containers are running:

docker compose ps


Check Promtail logs to ensure logs are being sent:

docker compose logs -f promtail

Verification from Monitoring Server

Node Exporter metrics should be reachable at:

http://<SERVER_IP>:9100/metrics


Logs should be visible in Grafana under Explore → Loki, for example:

{job="koha"}

Stopping the Agent

To stop both Node Exporter and Promtail:

docker compose down
