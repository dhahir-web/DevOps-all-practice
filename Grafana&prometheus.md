# 📘 Prometheus & Grafana Installation on Amazon Linux 2023

---

# 🚀 Install Prometheus

## Step 1: Update the System

```bash
sudo dnf update -y
```

---

## Step 2: Download Prometheus

```bash
cd /opt/

sudo wget https://github.com/prometheus/prometheus/releases/download/v3.5.2/prometheus-3.5.2.linux-amd64.tar.gz
```

---

## Step 3: Extract the Package

```bash
sudo tar -xvf prometheus-3.5.2.linux-amd64.tar.gz
```

---

## Step 4: Move into the Prometheus Directory

```bash
cd prometheus-3.5.2.linux-amd64
```

---

## Step 5: Move Binary Files

```bash
sudo mv prometheus /usr/local/bin/
sudo mv promtool /usr/local/bin/
```

---

## Step 6: Create Required Directories

```bash
sudo mkdir /etc/prometheus
sudo mkdir /var/lib/prometheus
```

---

## Step 7: Move Configuration File

```bash
sudo mv prometheus.yml /etc/prometheus/
```

---

## Step 8: Create Prometheus User

```bash
sudo useradd --no-create-home --shell /bin/false prometheus
```

---

## Step 9: Set Permissions

```bash
sudo chown -R prometheus:prometheus /etc/prometheus
sudo chown -R prometheus:prometheus /var/lib/prometheus
```

---

## Step 10: Verify Files

```bash
ls
cd
```

---

# ⚙️ Configure Prometheus Service

## Create Systemd Service File

```bash
sudo vi /etc/systemd/system/prometheus.service
```

Add the following configuration:

```ini
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
ExecStart=/usr/local/bin/prometheus \
  --config.file=/etc/prometheus/prometheus.yml \
  --storage.tsdb.path=/var/lib/prometheus/

Restart=always

[Install]
WantedBy=multi-user.target
```

---

# 🧪 Verify Prometheus Installation

## Check Version

```bash
prometheus --version
```

---

## Start and Enable Prometheus

```bash
sudo systemctl daemon-reload

sudo systemctl start prometheus

sudo systemctl enable prometheus
```

---

## Check Service Status

```bash
sudo systemctl status prometheus
```

---

## Check Logs

```bash
sudo journalctl -u prometheus -n 50 --no-pager
```

---

# 📊 Install Grafana

Official Download Page:

- https://grafana.com/grafana/download

---

## Step 1: Update System

```bash
sudo dnf update -y
```

---

## Step 2: Install Grafana Enterprise

```bash
sudo yum install -y https://dl.grafana.com/grafana-enterprise/release/13.0.1/grafana-enterprise_13.0.1_24542347077_linux_amd64.rpm
```

---

## Step 3: Start and Enable Grafana

```bash
sudo systemctl daemon-reload

sudo systemctl start grafana-server

sudo systemctl enable grafana-server
```

---

## Step 4: Check Grafana Status

```bash
sudo systemctl status grafana-server
```

---

# 🌐 Access URLs

## Prometheus

### Default Port
```bash
9090
```

### Access URL
```bash
http://<SERVER-IP>:9090
```

---

## Grafana

### Default Port
```bash
3000
```

### Access URL
```bash
http://<SERVER-IP>:3000
```

---

# 🔐 Grafana Default Login

| Username | Password |
|----------|-----------|
| admin | admin |

> ⚠️ Change the password after first login.

---

# ☁️ AWS Security Group Rules

Allow the following inbound ports:

| Service | Port | Protocol |
|----------|------|-----------|
| SSH | 22 | TCP |
| Prometheus | 9090 | TCP |
| Grafana | 3000 | TCP |

---
## Node exporter Installation
## https://grafana.com/grafana/dashboards/1860-node-exporter-full/

cd /opt/
sudo wget https://github.com/prometheus/node_exporter/releases/download/v1.11.1/node_exporter-1.11.1.linux-amd64.tar.gz

sudo tar -xvf node_exporter-1.11.1.linux-amd64.tar.gz

sudo mv node_exporter-1.11.1.linux-amd64/node_exporter /usr/local/bin/

sudo useradd --no-create-home --shell /bin/false node_exporter

sudo vim /etc/systemd/system/node_exporter.service

```
[Unit]
Description=Node Exporter
After=network.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
```

sudo systemctl daemon-reload
sudo systemctl enable node_exporter
sudo systemctl start node_exporter
sudo systemctl status node_exporter

---
## Add required Node's Name and Ip in prometheus.yml end of the file
  - /etc/prometheus/prometheus.yml
```
  - job_name: "Webserver"

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
      - targets: ["172.31.46.214:9100"]
```
