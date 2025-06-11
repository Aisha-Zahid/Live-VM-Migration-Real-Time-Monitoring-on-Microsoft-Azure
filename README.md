# 🚀 Live VM Migration & Real-Time Monitoring on Microsoft Azure

This repository contains a complete cloud infrastructure project where I implemented **live VM disk migration** and **real-time system monitoring** using Microsoft Azure, Prometheus, Grafana, and Node Exporter.

> 📝 Project completed 3–4 weeks ago | 📤 Documented now for public sharing

---

## 📁 Project Overview

| Feature             | Description                                   |
| ------------------- | --------------------------------------------- |
| 🧠 Goal             | Monitor VM health and simulate live migration |
| ☁️ Cloud Platform   | Microsoft Azure                               |
| 📊 Monitoring Stack | Prometheus + Grafana + Node Exporter          |
| 💻 VMs Used         | VM1 (Monitor), VM2 (Target), VM3 (Migrated)   |
| ⚙️ Automation Tools | Azure CLI, Bash, YAML                         |

---

## 🧱 Architecture

```
VM1 (Prometheus + Grafana)  <-- monitors --  VM2 (Node Exporter)
                                   ↑
                          [Disk attached from VM3]
```

---

## 🛠️ Steps Performed

### ✅ 1. Provision VMs on Azure

```bash
az vm create --resource-group MyResourceGroup --name VM1 --image Ubuntu2204 ...
az vm create --name VM2 ...
az vm create --name VM3 ...
```

### ✅ 2. Setup Prometheus & Grafana on VM1

* Installed via `apt`
* Configured scrape targets
* Created Grafana dashboards (CPU, RAM, Disk, Uptime)

### ✅ 3. Simulate VM Disk Migration (VM3 ➜ VM2)

```bash
az vm deallocate --name VM3
az snapshot create --resource-group MyResourceGroup --name VM3_Snapshot --source <VM3_DISK_ID>
az disk create --resource-group MyResourceGroup --name MigratedDisk --source VM3_Snapshot
az vm disk attach --resource-group MyResourceGroup --vm-name VM2 --name MigratedDisk
```

### ✅ 4. Configure Node Exporter on VMs

```bash
wget https://github.com/prometheus/node_exporter/releases/download/v*/node_exporter-*.linux-amd64.tar.gz
./node_exporter &
```

### ✅ 5. Create Alert Rules in Prometheus

```yaml
groups:
- name: example_alert
  rules:
  - alert: HighCPUUsage
    expr: rate(node_cpu_seconds_total{mode="user"}[1m]) > 0.7
    for: 1m
    labels:
      severity: critical
    annotations:
      summary: "High CPU usage detected"
      description: "CPU usage > 70% for over 1 min."
```

### ✅ 6. Stress Testing

```bash
stress-ng --cpu 2 --timeout 60s
```

---

## 📸 Screenshots (I’ve detailed every step, command, and dashboard in my full Medium article with screenshots.)

| Component         | Description                         |
| ----------------- | ----------------------------------- |
| Azure Portal      | 3 VMs provisioned                   |
| Prometheus UI     | Alerts visible at `/alerts`         |
| Grafana Dashboard | CPU, RAM, Disk panels for VM1 & VM2 |
| CLI               | Disk migration commands             |

---

## 📊 Grafana Panel Metrics

* **CPU Usage:**

```promql
rate(node_cpu_seconds_total{mode="user"}[5m])
```

* **Memory %:**

```promql
(node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes) / node_memory_MemTotal_bytes * 100
```

* **Disk I/O:**

```promql
rate(node_disk_bytes_read_total[5m])
```

* **Uptime:**

```promql
node_time_seconds - node_boot_time_seconds
```

---

## 🎓 Key Learnings

* Performed disk-based VM migration using Azure CLI
* Configured Prometheus to collect metrics across nodes
* Built real-time Grafana dashboards
* Triggered alerts and visualized usage during stress

---

## 📖 Blog & LinkedIn Post

* 📘 [Medium Blog](https://medium.com/@ayeshazahid036/live-vm-migration-and-real-time-monitoring-on-microsoft-azure-using-prometheus-grafana-2a0db7299dbc)
* 💼 [LinkedIn Post](https://www.linkedin.com/feed/update/urn:li:activity:7338601014121390082/)

---

## 🛡️ Disclaimer

This was a test/study project built on Azure Student Subscription. Please ensure proper shutdown of all services to avoid billing.

---

## 🤝 Let's Connect

Open to feedback, collaboration, or discussion around DevOps, observability, or cloud-native projects!
