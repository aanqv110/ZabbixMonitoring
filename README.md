# 🖥️ Zabbix Infrastructure Monitoring Lab

## 📌 Project Overview

This project demonstrates the **design, deployment, and validation of a centralized monitoring solution using Zabbix 6.x**.  
The lab simulates a **small enterprise infrastructure** built entirely on **VMware Workstation**, focusing on:

- Windows & Linux monitoring
- Agent-based monitoring
- Trigger and alert configuration
- Real-time problem detection
- Infrastructure visibility

---

## 🧱 Lab Architecture Overview

### Monitoring Stack

- **Zabbix Server:** Ubuntu Server 24.04 (64-bit)
- **Zabbix Web UI:** Apache + PHP
- **Database:** MySQL
- **Monitoring Method:** Zabbix Agent (Active & Passive checks)

---

## 🖧 Virtual Infrastructure (VMware Workstation)

All machines were created and managed using **VMware Workstation**.

### Virtual Machines

| Hostname | OS | Role |
|--------|----|-----|
| ZBX01 | Ubuntu Server 24.04 | Zabbix Server |
| DC01 | Windows Server | Domain Controller |
| APP01 | Windows Server | Application Server |
| APP02 | Windows Server | Application Server |
| WIN10 | Windows 10 | Admin / User Workstation |

### Network

- **VMware VMnet (Internal Network)**
- Static and DHCP IP addressing
- TCP communication between agents and server

---

## 🔧 VM Creation Steps (VMware Workstation)

1. Created new VMs using **Custom (Advanced)** configuration
2. Assigned:
   - 2 vCPUs (minimum)
   - 4–8 GB RAM depending on role
   - NAT / Host-only networking (VMnet)
3. Installed operating systems:
   - Ubuntu Server 24.04 (Zabbix)
   - Windows Server (DC01, APP01, APP02)
   - Windows 10 (WIN10)
4. Installed VMware Tools on all VMs

---

## 📦 Zabbix Server Installation (Ubuntu)

### System Preparation

```bash
sudo apt update
sudo apt install -y apache2 mysql-server php php-mysql php-gd php-xml php-bcmath php-mbstring

wget https://repo.zabbix.com/zabbix/6.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_6.0-6+ubuntu24.04_all.deb
sudo dpkg -i zabbix-release_6.0-6+ubuntu24.04_all.deb
sudo apt update

## Install Zabbix Components

sudo apt install -y zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-agent

---

🗄️ Database Configuration
Create Database and User
CREATE DATABASE zabbix CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;
CREATE USER 'zabbix'@'localhost' IDENTIFIED BY 'StrongPassword';
GRANT ALL PRIVILEGES ON zabbix.* TO 'zabbix'@'localhost';
FLUSH PRIVILEGES;

Import Zabbix Schema
zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql -u zabbix -p zabbix

🌐 Zabbix Web Setup

Access the frontend:

http://<Zabbix_Server_IP>/zabbix

Web Installer Steps

Verify prerequisites

Configure database connection

Set server name and time zone

Complete installation

🧩 Zabbix Agent Installation (Windows)

Installed Zabbix Agent 6.0 (64-bit) on:

DC01

APP01

APP02

WIN10

Agent Configuration

Zabbix Server IP: 192.168.40.138

Agent Port: 10050

Hostname: Must match Zabbix UI host entry

Verification Commands
sc query "Zabbix Agent"
netstat -ano | find "10050"

🖥️ Host Configuration in Zabbix

Path:

Configuration → Hosts → Create host

Host Settings

Host name

Agent interface (IP + port 10050)

Assign group: Windows servers

Applied Templates

Windows by Zabbix agent

Windows CPU by Zabbix agent

Windows physical disks by Zabbix agent

🚨 Triggers & Alerts
Example Trigger: High CPU Usage
{WIN10:system.cpu.util.last()}>80

Validation

Trigger fires when CPU exceeds threshold

Event visible under Monitoring → Problems

Auto-resolves when CPU usage normalizes

📊 Monitoring Validation

✅ Zabbix Agent connectivity (green ZBX)

✅ Metrics collected (CPU, memory, disk, uptime)

✅ Alerts generated and resolved

✅ Web UI accessible from Windows 10 VM

🧠 Skills Demonstrated

Zabbix deployment and troubleshooting

Windows & Linux monitoring

VMware networking

Agent-based monitoring

Trigger logic and alerting


🚀 Future Enhancements

Email / webhook notifications

Custom dashboards

Zabbix Proxy implementation

Windows service monitoring (AD, IIS)

HTTPS for Zabbix Web UI

📎 Purpose

This lab serves as a portfolio project, showcasing hands-on experience with:

Infrastructure monitoring

Virtualization

Alerting and observability


