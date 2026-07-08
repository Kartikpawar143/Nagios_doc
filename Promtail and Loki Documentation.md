# Loki + Promtail Installation and Monitoring on RHEL 9

## Objective

This document explains how to install **Loki** and **Promtail** on **RHEL 9**, configure log collection, and connect Loki to Grafana for centralized log monitoring.

---

# Architecture

```text
+-----------------------------------------------------+
|                     RHEL 9 Server                   |
|                                                     |
|  /var/log/messages                                 |
|  /var/log/secure                                   |
|  /var/log/httpd/*                                  |
|              │                                      |
|              ▼                                      |
|          Promtail                                   |
|              │                                      |
|              ▼                                      |
|             Loki (Port 3100)                                  |
|              │                                      |
+--------------│--------------------------------------+
               │ HTTP 
               ▼
+-----------------------------------------------------+
|                    Grafana                          |
|                   Port 3000                         |
+-----------------------------------------------------+
```

---

# Prerequisites

- RHEL 9.x
- Root or sudo privileges
- Internet connectivity
- x86_64 (amd64) architecture

Verify:

```bash
cat /etc/redhat-release
uname -m
```

Expected Output:

```text
Red Hat Enterprise Linux release 9.x
x86_64
```

---

# Step 1 - Download Loki and Promtail

```bash
cd /tmp

wget https://github.com/grafana/loki/releases/download/v3.2.0/loki-linux-amd64.zip

wget https://github.com/grafana/loki/releases/download/v3.2.0/promtail-linux-amd64.zip
```

---

# Step 2 - Install unzip

```bash
sudo dnf install unzip -y
```

---

# Step 3 - Extract the Packages

```bash
unzip loki-linux-amd64.zip
unzip promtail-linux-amd64.zip
```

---

# Step 4 - Install the Binaries

```bash
sudo mv loki-linux-amd64 /usr/local/bin/loki
sudo mv promtail-linux-amd64 /usr/local/bin/promtail
```

Grant execute permission:

```bash
sudo chmod +x /usr/local/bin/loki
sudo chmod +x /usr/local/bin/promtail
```

Restore SELinux context:

```bash
sudo restorecon -Rv /usr/local/bin
```

Verify:

```bash
loki --version
promtail --version
```

---

# Step 5 - Create Service Users

```bash
sudo useradd --system --no-create-home --shell /sbin/nologin loki

sudo useradd --system --no-create-home --shell /sbin/nologin promtail
```

---

# Step 6 - Create Required Directories

```bash
sudo mkdir -p /etc/loki
sudo mkdir -p /etc/promtail

sudo mkdir -p /var/lib/loki
sudo mkdir -p /var/lib/promtail
```

---

# Step 7 - Download Configuration Files

Loki

```bash
sudo wget -O /etc/loki/loki-local-config.yaml \
https://raw.githubusercontent.com/grafana/loki/v3.2.0/cmd/loki/loki-local-config.yaml
```

Promtail

```bash
sudo wget -O /etc/promtail/promtail-local-config.yaml \
https://raw.githubusercontent.com/grafana/loki/v3.2.0/clients/cmd/promtail/promtail-local-config.yaml
```

---

# Step 8 - Configure Promtail

Open the configuration file:

```bash
sudo nano /etc/promtail/promtail-local-config.yaml
```

Modify the client section:

```yaml
clients:
  - url: http://localhost:3100/loki/api/v1/push
```

---

# Step 9 - Set Ownership

```bash
sudo chown -R loki:loki /etc/loki
sudo chown -R loki:loki /var/lib/loki

sudo chown -R promtail:promtail /etc/promtail
sudo chown -R promtail:promtail /var/lib/promtail
```

---

# Step 10 - Create Loki Service

Create:

```text
/etc/systemd/system/loki.service
```

```ini
[Unit]
Description=Loki
After=network.target

[Service]
User=loki
Group=loki
Type=simple

ExecStart=/usr/local/bin/loki -config.file=/etc/loki/loki-local-config.yaml

Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```

---

# Step 11 - Create Promtail Service

Create:

```text
/etc/systemd/system/promtail.service
```

```ini
[Unit]
Description=Promtail
After=network.target

[Service]
User=promtail
Group=promtail
Type=simple

ExecStart=/usr/local/bin/promtail -config.file=/etc/promtail/promtail-local-config.yaml

Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```

---

# Step 12 - Reload systemd

```bash
sudo systemctl daemon-reload
```

---

# Step 13 - Enable Services

```bash
sudo systemctl enable loki
sudo systemctl enable promtail
```

---

# Step 14 - Start Services

```bash
sudo systemctl start loki
sudo systemctl start promtail
```

---

# Step 15 - Verify Status

```bash
systemctl status loki

systemctl status promtail
```

Expected:

```text
Active: active (running)
```

---

# Step 16 - Verify Loki

```bash
curl http://localhost:3100/ready
```

Expected:

```text
ready
```

---

# Step 17 - Generate Test Logs

```bash
logger "Promtail test log"
```

Monitor Promtail:

```bash
journalctl -u promtail -f
```

---

# Step 18 - Install Grafana

```bash
sudo dnf install -y https://dl.grafana.com/enterprise/release/grafana-enterprise-11.6.1-1.x86_64.rpm
```

Enable Grafana:

```bash
sudo systemctl enable grafana-server
sudo systemctl start grafana-server
```

---

# Step 19 - Configure Firewall

```bash
sudo firewall-cmd --permanent --add-port=3000/tcp
sudo firewall-cmd --reload
```

---

# Step 20 - Access Grafana

Open:

```
http://<SERVER-IP>:3000
```

Default Credentials

```
Username : admin

Password : admin
```

---

# Step 21 - Add Loki Data Source

Navigate to:

```
Connections
      ↓
Data Sources
      ↓
Add Data Source
      ↓
Loki
```

Set:

```
URL

http://localhost:3100
```

Click:

```
Save & Test
```

Expected:

```
Data source is working
```

---

# Step 22 - Explore Logs

Go to:

```
Explore
```

Select:

```
Loki
```

Run queries:

All Logs

```logql
{}
```

Search for Errors

```logql
{} |= "error"
```

Search Test Log

```logql
{} |= "Promtail test log"
```

---

# Useful Commands

Check Loki

```bash
systemctl status loki
```

Check Promtail

```bash
systemctl status promtail
```

Restart Services

```bash
systemctl restart loki
systemctl restart promtail
```

View Logs

```bash
journalctl -u loki -f
journalctl -u promtail -f
```

Check Readiness

```bash
curl http://localhost:3100/ready
```

---

# Troubleshooting

## Permission Denied

```bash
sudo restorecon -Rv /usr/local/bin
```

## Check SELinux

```bash
getenforce
```

## Verify Versions

```bash
loki --version

promtail --version
```

## Check Logs

```bash
journalctl -xeu loki

journalctl -xeu promtail
```

---

# Conclusion

- Loki stores logs efficiently.
- Promtail collects and forwards logs to Loki.
- Grafana connects to Loki to provide log visualization and searching using LogQL.
- This setup provides a centralized logging solution for Linux servers.
