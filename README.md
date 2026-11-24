# 💻 IT Admin, Network & Security Diagnostic Scripts

A collection of beginner-friendly PowerShell and Bash scripts for IT help desk diagnostics, network connectivity checks, open port auditing, and basic security log inspection.

---

## 1. Windows Network & DNS Diagnostic Script (`network-check.ps1`)
**Purpose:** Gathers IP configuration, tests external reachability, and verifies DNS resolution.

```powershell
# Windows Network & DNS Quick Check
Write-Host "=== 1. Active IP Configuration ===" -ForegroundColor Cyan
Get-NetIPAddress -AddressFamily IPv4 | Where-Object {$_.InterfaceAlias -notlike "*Loopback*"} | Select-Object InterfaceAlias, IPAddress, PrefixLength

# Windows Basic System Health Check
Write-Host "=== System Uptime ===" -ForegroundColor Cyan
(Get-Date) - (Get-CimInstance Win32_OperatingSystem).LastBootUpTime

Write-Host "`n=== Drive C: Storage Status ===" -ForegroundColor Cyan
Get-PSDrive C | Select-Object Name, @{Name="Used(GB)";Expression={[math]::Round($_.Used/1GB,2)}}, @{Name="Free(GB)";Expression={[math]::Round($_.Free/1GB,2)}}


Write-Host "`n=== 2. Testing External Connectivity (8.8.8.8) ===" -ForegroundColor Cyan
Test-Connection -ComputerName 8.8.8.8 -Count 2

Write-Host "`n=== 3. Testing DNS Resolution ===" -ForegroundColor Cyan
Resolve-DnsName -Name "google.com" -Type A

# Windows Open Ports & Firewall Quick Audit
Write-Host "=== Active Listening TCP Ports ===" -ForegroundColor Cyan
Get-NetTCPConnection -State Listen | Select-Object LocalAddress, LocalPort, OwningProcess | Sort-Object LocalPort

Write-Host "`n=== Windows Firewall Profile Status ===" -ForegroundColor Cyan
Get-NetFirewallProfile | Select-Object Name, Enabled

#!/bin/bash
# Quick Linux Server & Basic Security Check

echo "=== Disk Usage ==="
df -h /

echo -e "\n=== Memory Usage ==="
free -h

echo -e "\n=== Website HTTP Status Check ==="
curl -Is [https://example.com](https://example.com) | head -n 1

echo -e "\n=== Recent Failed SSH Login Attempts ==="
grep "Failed password" /var/log/auth.log 2>/dev/null | tail -n 10

