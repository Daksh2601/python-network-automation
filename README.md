# Project 8 — Python Network Automation

## Overview

Automated network configuration backup system built with Python. Connects to routers via SSH, pulls running configurations, saves them with timestamps, and runs on a daily cron schedule with email alerting for failures.

## How It Works

1. **Mock router simulator** starts two virtual Cisco routers (Router-HQ on port 2201, Router-Branch on port 2202)
2. **Backup script** connects to each router via SSH using Paramiko
3. Sends `show running-config` and saves output to timestamped files
4. Logs results to `backup_log.txt`
5. Sends email alert if any backup fails
6. **Cron job** runs the entire process daily at 2:00 AM

## Project Structure

```
network-automation/
├── mock_routers.py          # SSH mock simulator (Router-HQ + Router-Branch)
├── backup_configs.py        # Main backup script (SSH, pull config, save)
├── start_and_backup.sh      # Wrapper script (start, backup, cleanup)
├── backup_log.txt           # Audit log with pass/fail counts
├── cron_log.txt             # Cron output log
└── backups/
    ├── Router-HQ_2026-04-05_17-18-34.txt
    ├── Router-Branch_2026-04-05_17-18-37.txt
    └── ...
```

## Technologies Used

| Technology | Purpose |
|---|---|
| Python 3.12 | Core scripting language |
| Paramiko | SSH connections to network devices |
| Netmiko | Network device automation library |
| Bash | Wrapper script for cron automation |
| Cron | Daily scheduled execution |
| SMTP (smtplib) | Email alerts on backup failure |
| Ubuntu Server 24.04 | Automation platform |

## Device Inventory

| Device | SSH Port | Username | Config Source |
|---|---|---|---|
| Router-HQ | 2201 | admin | Project 7 HQ router |
| Router-Branch | 2202 | admin | Project 7 Branch router |

## Backup File Format

Each backup file includes metadata headers followed by the full device configuration:

```
! Backup: Router-HQ
! Date: 2026-04-05_17-18-34
! Source: 127.0.0.1:2201
==================================================

Building configuration...
Current configuration : 2847 bytes
!
hostname Router-HQ
...
```

## Cron Schedule

```
# Daily at 2:00 AM
0 2 * * * /bin/bash /home/admin/network-automation/start_and_backup.sh >> /home/admin/network-automation/cron_log.txt 2>&1
```

## Quick Start

```bash
# 1. Clone the repo
git clone https://github.com/Daksh2601/python-network-automation.git
cd python-network-automation

# 2. Install dependencies
pip3 install paramiko netmiko

# 3. Run the full backup
bash start_and_backup.sh

# 4. Check results
ls -la backups/
cat backup_log.txt
```

## Email Alerting

The script includes SMTP email alerting (disabled by default). To enable:

1. Set `EMAIL_ENABLED = True` in `backup_configs.py`
2. Configure your Gmail address and App Password
3. Failed backups will trigger an immediate email notification

## Verification Results

| Test | Result |
|---|---|
| SSH connection to Router-HQ | ✅ Pass |
| SSH connection to Router-Branch | ✅ Pass |
| Config backup with timestamp | ✅ Pass |
| Multiple runs create unique files | ✅ Pass |
| Backup log updated | ✅ Pass |
| Cron job scheduled | ✅ Pass |

## Platform

- Ubuntu Server 24.04.4 LTS (aarch64)
- VirtualBox on Mac M1 (Apple Silicon)
- Python 3.12.3 / Pip 24.0

## Author

**Daksh Patel** — CCNA Certified
- GitHub: [github.com/Daksh2601](https://github.com/Daksh2601)
- LinkedIn: [linkedin.com/in/pateldaksh](https://linkedin.com/in/pateldaksh)
- Portfolio: [daksh2601.github.io/Portfolio](https://daksh2601.github.io/Portfolio)
