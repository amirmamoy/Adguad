### 📚 Dokumentasi Adguad Home Server
Version 1.0 • last update: 2025

## 📋 DAFTAR ISI
- [Overview](#overview)
- [Spesifikasi Server](#spesifikasi-server)
- [Konfigurasi DNS](#konfigurasi-dns)
- [Filter List](#filter-list)
- [Monitoring dan Statistik](#monitoring-dan-statistik)
- [Troubleshooting](#troubleshooting)
- [Backup & Maintenance](#backup--maintenance)

---

## 🏠 Overview

## Informasi Server
```txt
📍 Alamat Server: 192.168.0.103
🌐 Web Admin: http://192.168.0.103:3000
🔧 DNS Port: 53
📊 Status: Active & Optimal
```
## Purpose
AdGuard Home berfungsi sebagai:
+ ✅ DNS Server jaringan lokal
+ ✅ Pemblokir iklan & tracker
+ ✅ Perlindungan malware & phishing
+ ✅ Parental control (opsional)
+ ✅ DNS over HTTPS resolver

## ⚙️ Spesifikasi Server
## Hardware 
```text
OS: Armbian
Architecture: ARM
Storage: > 10GB available
RAM: > 1GB recommended
```
## Software 
```text
AdGuard Home: Latest version
Service: systemd managed
Config Path: /opt/AdGuardHome/AdGuardHome.yaml
Data Path: /opt/AdGuardHome/data/
```

## 🔧 Konfigurasi DNS
## Upstream DNS Servers
```text
tls://dns.google
tls://1dot1dot1dot1.cloudflare-dns.com
tls://dns.quad9.net
```
## Bootstrap DNS
```text
8.8.8.8
1.1.1.1
9.9.9.9
```
## DNS Setting 
```text
dns:
  bind_hosts: [0.0.0.0]
  port: 53
  upstream_dns: [tls://dns.google, ...]
  bootstrap_dns: [8.8.8.8, ...]
  blocking_mode: "null_ip"  # 0.0.0.0
  edns_client_subnet: true
  dnssec: true
  ipv6: true
  cache_size: 4194304
  rate_limit: 20
```
## 🛡️ Filter List
## Active Blocklist
```text
✓ https://adguardteam.github.io/AdGuardSDNSFilter/Filters/filter.txt
✓ https://small.oisd.nl/
✓ https://raw.githubusercontent.com/hagezi/dns-blocklists/main/adguard/pro.txt
✓ https://easylist.to/easylist/easyprivacy.txt
```
### Update Schedule 
+ Interval: 24 jam
+ Auto-update: Enabled
+ Last update: (check web interface)
## Custom filtering rules
```text
# Tambahan rules khusus jika diperlukan
||doubleclick.net^
||googleadservices.com^
||googlesyndication.com^
```
## 📊 Monitoring dan Statistik
## Performance Metrics
```text
📈 Total Queries: 958
🚫 Blocked Queries: 452
🎯 Block Rate: 47.2%
⚡ Avg Response Time: <50ms
```
## Access Monitoring 
1. Dasboard: http//192.168.0.103:3000
2. Query log: Real-time monitoring
3. Statistics: Historical data (7 hari retention)
4. Filtering Log: Blocked domains detail
## Critical Metrics to Monitor
+ ✅ DNS response time < 100ms
+ ✅ Block rate 40-50%
+ ✅ Memory usage < 80%
+ ✅ Uptime 99%
## 🚨 Troubleshooting
## Common Issues & Solutions
## ❌ Internet Tidak Connect
```bash
# Cek service status
sudo systemctl status AdGuardHome

# Test DNS resolution
nslookup google.com 192.168.0.103

# Restart service
sudo systemctl restart AdGuardHome
```
## ❌ Client Tidak Bisa Access
```bash
# Cek firewall
sudo ufw status

# Test port 53
telnet 192.168.0.103 53

# Cek client DNS settings
```
## ❌ Web Interface Tidak Bisa Di Akses 
```bash
# Cek port 3000
sudo netstat -tulpn | grep :3000

# Restart service
sudo systemctl restart AdGuardHome
```
## Diagnostic Commands
```bash
# Service status
sudo systemctl status AdGuardHome

# Log monitoring
sudo journalctl -u AdGuardHome -f

# Resource usage
htop
df -h

# Network check
ip addr show
ping 8.8.8.8
```
## 📑 Backup Dan Maintenance 
## Backup Configuration
```bash
#!/bin/bash
# backup-adguard.sh
DATE=$(date +%Y%m%d-%H%M%S)
sudo tar -czf /backup/adguard-backup-$DATE.tar.gz /opt/AdGuardHome/
sudo cp /opt/AdGuardHome/AdGuardHome.yaml /backup/AdGuardHome.yaml.$DATE
echo "Backup completed: adguard-backup-$DATE.tar.gz"
```
## Auto Backup Schedule 
```bash
# Tambah di crontab
0 2 * * * /path/to/backup-adguard.sh
```
## Update Procedure 
```bash
# Manual update
sudo /opt/AdGuardHome/AdGuardHome -s update
sudo systemctl restart AdGuardHome

# Auto update (via script)
curl -s -S -L https://raw.githubusercontent.com/AdguardTeam/AdGuardHome/master/scripts/install.sh | sh -s -- -c
```
## Restore Procedure 
```bash
# Stop service
sudo systemctl stop AdGuardHome

# Extract backup
sudo tar -xzf adguard-backup-YYYYMMDD.tar.gz -C /

# Restart service
sudo systemctl start AdGuardHome
```
## 🔐 Security Setting 
## Web Admin Security 
+ ✅ Password protected
+ ✅ Local network access only
+ ✅ Session timeout enabled
## Network Security
```bash
sudo ufw allow 53/tcp
sudo ufw allow 53/udp
sudo ufw allow 3000/tcp
```
### Privacy Settings
+ ✅ Query log retention: 7 hari
+ ✅ Anonymize client IP: Enabled
+ ✅ No personal data collection
## 📱 Client Configuration
### Manual DNS Setup
```bash
DNS Server: 192.168.0.103
```
## Router Setup
```bash
Primary DNS: 192.168.0.103
Secondary DNS: 1.1.1.1 (fallback)
```
## Supported Clients

+ ✅ Windows/Mac/Linux
+ ✅ Android/iOS
+ ✅ Smart TVs
+ ✅ IoT Devices

## Logs File Location 
```bash
/opt/AdGuardHome/data/querylog.json
/var/log/syslog
journalctl -u AdGuardHome
```

