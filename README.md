
DevOps Project 05 — EC2 Monitoring Stack (Node Exporter + Prometheus + Grafana)

This project demonstrates how to monitor an AWS EC2 Ubuntu instance using Node Exporter, Prometheus, and Grafana.
It collects real-time Linux system metrics, stores them as time-series data, and visualizes everything through Grafana dashboards.


---

🔧 Architecture Overview

DevOps Engineer → AWS EC2 Ubuntu → Node Exporter → Prometheus → Grafana → End User


---

⭐ Features

Monitor CPU, RAM, Disk, Network, Uptime using Node Exporter

Prometheus scrapes metrics at regular intervals

Time-series metrics stored and accessible through Prometheus UI

Grafana visualizes metrics using advanced dashboards

All services run on a single EC2 instance

Systemd services ensure Prometheus + Node Exporter run in background



---

🛠 Tools & Technologies

AWS EC2 (Ubuntu)

Node Exporter

Prometheus

Grafana

Systemd services

Linux CLI

---

🚀 Setup Instruction

---

📌 1. Launch EC2 (Ubuntu)

Allow inbound ports:

22 → SSH

9100 → Node Exporter

9090 → Prometheus

3000 → Grafana



---

📌 2. Update Server

sudo apt update -y
sudo apt upgrade -y


---

🟧 Install Node Exporter

📌 3. Download & Extract Node Exporter

cd /tmp
curl -LO https://github.com/prometheus/node_exporter/releases/download/v1.6.1/node_exporter-1.6.1.linux-amd64.tar.gz
tar -xvf node_exporter-1.6.1.linux-amd64.tar.gz
sudo mv node_exporter-1.6.1.linux-amd64 /etc/node_exporter

📌 4. Create Node Exporter Service

sudo nano /etc/systemd/system/node_exporter.service

Paste:

[Unit]
Description=Node Exporter
After=network.target

[Service]
ExecStart=/etc/node_exporter/node_exporter

[Install]
WantedBy=default.target

📌 5. Enable & Start Node Exporter

sudo systemctl daemon-reload
sudo systemctl enable node_exporter
sudo systemctl start node_exporter
sudo systemctl status node_exporter

Node Exporter UI:
http://EC2-IP:9100/metrics


---

🔥 Install Prometheus

📌 6. Download & Extract Prometheus

cd /tmp
curl -LO https://github.com/prometheus/prometheus/releases/download/v2.48.1/prometheus-2.48.1.linux-amd64.tar.gz
tar -xvf prometheus-2.48.1.linux-amd64.tar.gz
sudo mv prometheus-2.48.1.linux-amd64 /etc/prometheus

📌 7. Configure Prometheus Scrape Targets

sudo nano /etc/prometheus/prometheus.yml

Add:

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']

  - job_name: 'node_exporter'
    static_configs:
      - targets: ['localhost:9100']

📌 8. Create Prometheus Service

sudo nano /etc/systemd/system/prometheus.service

Paste:

[Unit]
Description=Prometheus Monitoring
After=network.target

[Service]
ExecStart=/etc/prometheus/prometheus \
  --config.file=/etc/prometheus/prometheus.yml

[Install]
WantedBy=default.target

📌 9. Enable & Start Prometheus

sudo systemctl daemon-reload
sudo systemctl enable prometheus
sudo systemctl start prometheus
sudo systemctl status prometheus

Prometheus UI:
http://EC2-IP:9090


---

🟨 Install Grafana

📌 10. Add Grafana Repository & Install

sudo apt-get install -y apt-transport-https software-properties-common wget
wget -q -O - https://apt.grafana.com/gpg.key | sudo apt-key add -
echo "deb https://apt.grafana.com stable main" | sudo tee /etc/apt/sources.list.d/grafana.list
sudo apt-get update -y
sudo apt-get install grafana -y

📌 11. Start Grafana

sudo systemctl enable grafana-server
sudo systemctl start grafana-server
sudo systemctl status grafana-server

Grafana UI:
http://EC2-IP:3000
Login: admin / admin


---

📈 12. Import Dashboards

From Grafana → Dashboards → Import:

Node Exporter Full — ID: 1860

Prometheus 2.0 Stats — ID: 3662



---

🎉 Project Completed

✔ EC2 metrics monitored
✔ Node Exporter running as service
✔ Prometheus scraping metrics
✔ Grafana visualizing dashboards

Your monitoring pipeline is fully operational.

