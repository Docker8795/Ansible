# 📋 TOPIC 3: SERVICE TROUBLESHOOTING & DIAGNOSTICS

## File: `topic3-diagnostics.yml`

**32 Tasks | 27 diagnostic checks for comprehensive service troubleshooting**

---

## 🎯 What This Playbook Does

### **Network Diagnostics**

#### **Task 3.1-3.2: Port 80 Listening Check**
```bash
netstat -tlnp | grep :80
```
- Checks if Apache is listening on port 80
- Status: LISTENING or NOT LISTENING
- Diagnoses: Accessibility issues

#### **Task 3.18-3.19: Firewall Status**
```bash
systemctl status firewalld
```
- Checks if firewall is active
- Status: ACTIVE or INACTIVE
- Diagnoses: Connection blocked issues

#### **Task 3.16-3.17: SELinux Status**
```bash
getenforce
```
- Checks SELinux enforcement level
- Status: Enforcing/Permissive/Disabled
- Diagnoses: Permission denied errors

#### **Task 3.20-3.21: All Open Ports**
```bash
netstat -tlnp
```
- Lists all listening ports and services
- Shows what's running
- Diagnoses: Port conflicts

---

### **Process & Service Diagnostics**

#### **Task 3.3-3.4: Process Count**
```bash
ps aux | grep -c '[h]ttpd'
```
- Counts httpd processes
- Healthy: 5+ processes
- Diagnoses: Service not running properly

#### **Task 3.5-3.6: Detailed Service Status**
```bash
systemctl status httpd
```
- Full service details
- Shows PID, memory, restart count
- Diagnoses: Service issues

#### **Task 3.9-3.11: Enable Status**
```bash
systemctl is-enabled httpd
systemctl is-enabled myapp
```
- Checks if services auto-start
- Status: enabled/disabled
- Diagnoses: Missing auto-start

#### **Task 3.7-3.8: Service File Verification**
```bash
stat /etc/systemd/system/myapp.service
```
- Checks file exists and permissions
- Verifies ownership
- Diagnoses: Permission issues

---

### **Configuration Diagnostics**

#### **Task 3.22-3.23: Apache Config Syntax**
```bash
httpd -t
```
- Validates Apache configuration
- Status: OK or ERROR
- Diagnoses: Config errors

#### **Task 3.24-3.25: Apache Version**
```bash
httpd -v
```
- Shows Apache version
- Diagnoses: Compatibility issues

---

### **Resource Diagnostics**

#### **Task 3.12-3.13: Memory Usage**
```bash
free -m
```
- Shows RAM usage in MB
- Diagnoses: Resource exhaustion

#### **Task 3.14-3.15: Disk Space Usage**
```bash
df -h
```
- Shows disk usage percentage
- Diagnoses: Disk full issues

---

### **Log & Error Diagnostics**

#### **Task 3.26-3.27: System Errors**
```bash
journalctl -p err -n 50 --no-pager
```
- Last 50 system errors from journal
- Diagnoses: System-wide issues

---

## 🚀 Quick Start

### **Check Syntax**
```bash
ansible-playbook topic3-diagnostics.yml --syntax-check
```

### **Run Playbook**
```bash
ansible-playbook topic3-diagnostics.yml
```

### **Run on Specific Host**
```bash
ansible-playbook topic3-diagnostics.yml -l node1
```

### **With Sudo Password**
```bash
ansible-playbook topic3-diagnostics.yml -K
```

---

## 📂 Output Locations

### **On Each Node**
```
/tmp/topic3_diagnostics_outputs/topic3_diagnostics_report_<hostname>.txt
```

### **On Master**
```
/root/playbooks/outputs/<hostname>_topic3_diagnostics.txt
```

---

## 📊 Report Contents

The generated report includes:

```
NETWORK DIAGNOSTICS:
- Port 80 listening status
- Firewall status
- SELinux status
- All open ports & services

PROCESS DIAGNOSTICS:
- httpd process count
- Detailed service status
- Service enable status
- Custom service file verification

CONFIGURATION DIAGNOSTICS:
- Apache config syntax validation
- Apache version

RESOURCE DIAGNOSTICS:
- Memory usage (RAM)
- Disk space usage

LOG & ERROR DIAGNOSTICS:
- System error logs (50 lines)

TROUBLESHOOTING CHECKLIST:
- All 27 checks performed ✓
```

---

## ✅ Expected Output

### **Console Output**
```
TASK [3.2 - Display port 80 listening status] ***
ok: [node1] => {
    "msg": "Port 80 status: LISTENING"
}

TASK [3.4 - Display httpd process count] ***
ok: [node1] => {
    "msg": "Number of httpd processes: 5"
}

[... more diagnostic checks ...]

TASK [3.32 - Display final summary] ***
ok: [node1] => {
    "msg": [
        "==============================================",
        "TOPIC 3: DIAGNOSTICS COMPLETED",
        "==============================================",
        "Node: node1",
        "Port 80: LISTENING",
        "Config Valid: YES",
        "Firewall: ACTIVE",
        "SELinux: Enforcing",
        "Processes: 5",
        "Total Checks: 27",
        ...
    ]
}
```

---

## 🔧 Troubleshooting Guide

### **Problem: Port 80 Not Listening**

**Check in report:**
- Task 3.1: Port status
- Task 3.3: Process count (should be > 0)
- Task 3.22: Config syntax

**Solutions:**
```bash
# Check if service is running
systemctl is-active httpd

# Check for config errors
httpd -t

# Check firewall
firewall-cmd --list-all
```

---

### **Problem: Service Not Starting**

**Check in report:**
- Task 3.5: Detailed status
- Task 3.22: Config validity
- Task 3.26: System errors

**Solutions:**
```bash
# View service logs
journalctl -u httpd -n 50

# Test config
httpd -t

# Check permissions
ls -la /etc/systemd/system/myapp.service
```

---

### **Problem: High Resource Usage**

**Check in report:**
- Task 3.12: Memory usage
- Task 3.14: Disk usage
- Task 3.3: Process count

**Solutions:**
```bash
# Check memory
free -m

# Check disk
df -h

# Check processes
ps aux | grep httpd
```

---

### **Problem: Firewall Blocking**

**Check in report:**
- Task 3.18: Firewall status
- Task 3.20: Open ports

**Solutions:**
```bash
# Check firewall status
systemctl status firewalld

# Allow HTTP
firewall-cmd --add-service=http --permanent
firewall-cmd --reload
```

---

### **Problem: SELinux Issues**

**Check in report:**
- Task 3.16: SELinux status

**Solutions:**
```bash
# Check SELinux
getenforce

# View denials
journalctl | grep -i "avc denied"

# Set to permissive (if needed)
setenforce 0
```

---

## 💡 Pro Tips

1. **Review the full report for complete picture**
   ```bash
   cat /root/playbooks/outputs/node1_topic3_diagnostics.txt
   ```

2. **Compare multiple nodes**
   ```bash
   diff /root/playbooks/outputs/node1_topic3_diagnostics.txt \
        /root/playbooks/outputs/node2_topic3_diagnostics.txt
   ```

3. **Search for specific issues**
   ```bash
   grep -i "error\|warning\|failed" /root/playbooks/outputs/node1_topic3_diagnostics.txt
   ```

4. **Run regularly for monitoring**
   ```bash
   # Add to crontab for daily checks
   0 2 * * * ansible-playbook /path/to/topic3-diagnostics.yml
   ```

---

## 📋 All Diagnostic Checks (27 Total)

| Check | Purpose | Status |
|-------|---------|--------|
| Port 80 | Service accessibility | LISTEN/NOT LISTEN |
| Process count | Service health | Count |
| Service status | Service running | active/inactive |
| File permissions | Custom service | File checks |
| Enable status | Auto-start | enabled/disabled |
| Memory | Resource usage | MB/% |
| Disk | Storage usage | GB/% |
| SELinux | Security | Enforcing/Permissive |
| Firewall | Network security | Active/Inactive |
| Open ports | Service ports | Port list |
| Config syntax | Apache config | OK/ERROR |
| Apache version | Software version | Version |
| System errors | System health | Error count |
| And 14 more... | Complete diagnostics | Various |

---

## ✨ Key Features

✅ Network connectivity checks
✅ Process monitoring
✅ Service status verification
✅ Configuration validation
✅ Security status (SELinux, Firewall)
✅ Resource monitoring (Memory, Disk)
✅ Log analysis
✅ File permission checks
✅ Comprehensive reporting
✅ Master backup copy

---

## 🎯 Use Cases

### **Use Case 1: Troubleshoot Service Issues**
```bash
ansible-playbook topic3-diagnostics.yml
cat /root/playbooks/outputs/node1_topic3_diagnostics.txt
# Review all diagnostics for root cause
```

### **Use Case 2: Monitor System Health**
```bash
# Run daily via cron
0 2 * * * ansible-playbook /path/to/topic3-diagnostics.yml
```

### **Use Case 3: Compare Node Health**
```bash
# Run on all nodes
ansible-playbook topic3-diagnostics.yml

# Compare results
diff /root/playbooks/outputs/node*.txt
```

---

## 🚀 Run Now

```bash
ansible-playbook topic3-diagnostics.yml
```

**Complete service diagnostics automated!** ✅

