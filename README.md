# 🐧 Linux Server Administration Lab — Ubuntu 24.04 LTS

> **Ubuntu Server 24.04 LTS deployed on VMware with Apache2 web server, UFW firewall, SSH hardening, dual NIC configuration, and GNS3 VM integration — fully documented with real troubleshooting.**

---

## 📋 Project Overview

This project deploys and configures an Ubuntu Server 24.04 LTS instance as a functional enterprise Linux server within a multi-VM home lab environment. The server is integrated into both the VMware virtual network and the GNS3 network topology, operating as a web server accessible from the wider simulated enterprise network.

---

## 🏗️ Lab Architecture

```
GNS3 Enterprise Topology (VMnet2 — 192.168.50.0/24)
          |
     Ubuntu Server 24.04 LTS
     NIC1: VMnet1 — 192.168.10.x   (management/internal)
     NIC2: VMnet2 — 192.168.50.x   (GNS3 integration)
          |
     VMware Workstation Host
```

---

## 🛠️ Technologies & Features

| Category | Implementation |
|---|---|
| OS | Ubuntu Server 24.04 LTS (minimal install, no GUI) |
| Web Server | Apache2 — serving HTTP on port 80 |
| Firewall | UFW (Uncomplicated Firewall) — SSH + HTTP allowed, default deny |
| Remote Access | SSH — key-based and password authentication |
| Networking | Dual NIC — VMnet1 (management) + VMnet2 (GNS3 integration) |
| User Management | Custom user creation, sudo privileges, group management |
| Service Management | systemctl — start, stop, enable, status |
| GNS3 Integration | Third NIC added — server connected into GNS3 topology via Cloud node |

---

## 📡 Network Configuration

| Interface | Network | IP | Purpose |
|---|---|---|---|
| ens33 | VMnet1 | 192.168.10.x | Management — SSH access from host |
| ens36 | VMnet2 | 192.168.50.x | GNS3 topology integration |

---

## 🔒 UFW Firewall Rules

```bash
# Active UFW rules
ufw allow OpenSSH          # SSH on port 22
ufw allow 'Apache Full'    # HTTP (80) + HTTPS (443)
ufw default deny incoming  # Block all other inbound
ufw default allow outgoing # Allow all outbound
ufw enable
```

**Status:** Active — default deny inbound with explicit allow for SSH and Apache only.

---

## 🌐 Apache2 Web Server

```bash
# Installation
apt install apache2 -y
systemctl enable apache2
systemctl start apache2

# Verification
systemctl status apache2
curl http://localhost
```

- Default web root: `/var/www/html/`
- Accessible from GNS3 network devices and Windows Server DC01 via IP
- UFW profile `Apache Full` permits ports 80 and 443

---

## 👤 User Management

```bash
# Create new user with sudo access
adduser vasanth
usermod -aG sudo vasanth

# Verify group membership
groups vasanth

# Switch user
su - vasanth
```

---

## 🔧 Key Service Management Commands

```bash
# Apache2
systemctl start apache2
systemctl stop apache2
systemctl restart apache2
systemctl status apache2
systemctl enable apache2      # Start on boot

# SSH
systemctl status ssh
systemctl restart ssh

# UFW
ufw status verbose
ufw status numbered
```

---

## 🔗 GNS3 Integration

The Ubuntu Server was connected into the enterprise GNS3 topology using a third NIC (VMnet2) and a Cloud node in GNS3:

1. Added VMnet2 virtual adapter (192.168.50.0/24) in VMware
2. Added third NIC to Ubuntu Server VM pointing to VMnet2
3. Added Cloud2 node in GNS3 — bound to VMnet2 adapter
4. Connected Cloud2 to the enterprise topology switch
5. Ubuntu Server now reachable from GNS3 virtual routers and PCs

---

## 🐛 Real Issues Solved

- **GNS3 server binding issue:** `gns3_server.ini` was bound to the wrong host IP — changed binding to correct VMware adapter IP to restore GNS3 connectivity.
- **Dual NIC routing conflicts:** Added second NIC without disrupting existing management access by configuring correct routing table entries for each subnet.

---

## ✅ Skills Demonstrated

- Ubuntu Server installation and initial system configuration
- Apache2 web server deployment and service management
- UFW firewall configuration with principle of least privilege
- SSH remote access setup and management
- Dual NIC networking in VMware
- Linux user and group management
- systemctl service lifecycle management
- Integration of a physical VM into a GNS3 virtual topology

---

## 📁 Project Files

```
linux-server-lab/
├── docs/
│   └── Linux_Server_Documentation.docx
└── README.md
```

---

## 📚 Linux Topics Demonstrated

`Ubuntu Server 24.04` `Apache2` `UFW Firewall` `SSH` `systemctl` `User Management` `Dual NIC` `GNS3 VM Integration` `VMware Networking` `Package Management (apt)`

---

## 👨‍💻 Author

**Vasanth Kumar**
BCA — Vivekananda Institute of Management, Bengaluru, Karnataka
📧 vasanthkumarvk2855@gmail.com
🔗 [LinkedIn](https://linkedin.com/in/vasanth-kumar-vk55) | [GitHub](https://github.com/vasanth-kumar-vk)

---

*This project is part of a hands-on IT infrastructure portfolio built to demonstrate entry-level Linux Server / System Administrator skills.*
