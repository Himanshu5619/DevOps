Here's a detailed `README.md` file that includes **all the setup steps** for your **Monitoring with Prometheus and Grafana** project. You can use this in your GitHub repo or local folder.

---

### 📄 `README.md`

```markdown
# 🚀 Monitoring with Prometheus and Grafana

This project sets up a full monitoring stack using **Node Exporter**, **Prometheus**, and **Grafana**.  
Node Exporter collects Linux system metrics, Prometheus scrapes them, and Grafana visualizes them.

---

## 🧰 Prerequisites

- Docker installed and running
- Docker Compose installed
- Linux/WSL environment (for Node Exporter)

---

## 📦 1. Install Node Exporter on Linux/WSL

### ➤ Download and Run Node Exporter

```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.9.1/node_exporter-1.9.1.linux-amd64.tar.gz
tar xvfz node_exporter-1.9.1.linux-amd64.tar.gz
cd node_exporter-1.9.1.linux-amd64
./node_exporter
```

- Node Exporter runs on: [http://localhost:9100/metrics](http://localhost:9100/metrics)

### ➤ Verify

```bash
curl http://localhost:9100/metrics
curl http://localhost:9100/metrics | grep "node_"
```

### ➤ Find WSL IP (if Prometheus is outside WSL)

```bash
hostname -I
```

---

## 🐳 2. Start Prometheus & Grafana with Docker Compose

### ➤ Create Project Directory

```bash
mkdir monitoring-stack
cd monitoring-stack
```

### ➤ Create `docker-compose.yml`

```yaml
services:
  prometheus:
    image: prom/prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml

  grafana:
    image: grafana/grafana
    ports:
      - "3000:3000"
```

### ➤ Create `prometheus.yml`

> Replace `<WSL_IP>` with your actual IP address from `hostname -I`.

```yaml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'node-exporter'
    static_configs:
      - targets: ['<WSL_IP>:9100']
```

### ➤ Start the Stack

```bash
docker-compose up -d
```

If you get a permissions error, try:

```bash
sudo docker-compose up -d
```

---

## 📊 3. Set Up Grafana Dashboard

### ➤ Access Grafana

- URL: [http://localhost:3000](http://localhost:3000)
- Default Login: `admin` / `admin`

### ➤ Add Prometheus as Data Source

- Navigate: **Configuration → Data Sources → Add Prometheus**
- URL: `http://prometheus:9090`
- Click **Save & Test**

### ➤ Import Node Exporter Dashboard

- Navigate: **Create (+) → Import**
- Use Dashboard ID: `1860`
- Select Prometheus as data source
- Click **Import**

---

## 🛠️ Troubleshooting

### ❗ Docker Permission Denied

```text
permission denied while trying to connect to the Docker daemon socket
```

**Fix:**
```bash
sudo usermod -aG docker $USER
newgrp docker
```

### ❗ Prometheus Target Down?

- Make sure Node Exporter is running
- Replace `localhost` with WSL IP
- Restart stack:
```bash
docker-compose restart
```

---

## ✅ Summary

| Component      | URL                        | Status            |
|----------------|----------------------------|--------------------|
| Node Exporter  | http://localhost:9100      | Running            |
| Prometheus     | http://localhost:9090      | Scraping metrics   |
| Grafana        | http://localhost:3000      | Visualizing data   |
| Dashboard      | Node Exporter Full (ID:1860)| Imported          |

---

## 📂 Folder Structure

```
monitoring-stack/
├── docker-compose.yml
└── prometheus.yml
```

---

## 📌 Credits

- [Prometheus](https://prometheus.io/)
- [Grafana](https://grafana.com/)
- [Node Exporter](https://github.com/prometheus/node_exporter)
- Dashboard: [Node Exporter Full - ID 1860](https://grafana.com/grafana/dashboards/1860)

---
```

Let me know if you'd like a version with images, GIFs, or a pre-filled `.zip` you can download!
