# 📋 TOPIC 2: SERVICE INSTALLATION & MANAGEMENT

## File: `topic2-service-management.yml`

**18 Tasks | Service installation, start/stop/reload, enable/disable, custom services**

---

## 🎯 What This Playbook Does

### **Task 2.1: Install Apache (httpd)**
```bash
package:
  name: httpd
  state: present
```
- Installs Apache HTTP Server
- Works on RedHat, CentOS, Fedora, Debian, Ubuntu

### **Task 2.2: Start Service**
```bash
systemctl start httpd
```
- Starts Apache immediately
- Service runs in background

### **Task 2.3: Enable at Boot**
```bash
systemctl enable httpd
```
- Service auto-starts after system reboot
- One-time configuration

### **Task 2.4-2.5: Check Service Status**
```bash
systemctl is-active httpd
```
- Shows if service is running
- Output: `active` or `inactive`

### **Task 2.6: Reload Service**
```bash
systemctl reload httpd
```
- Graceful reload (no downtime)
- Reloads config without stopping

### **Task 2.7-2.8: List Failed Services**
```bash
systemctl --failed
```
- Shows all failed system services
- Helps identify problems

### **Task 2.9-2.11: Create Custom Service**
```yaml
# Creates /etc/systemd/system/myapp.service
[Unit]
Description=My Custom Application
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/sleep infinity
Restart=on-failure
User=root

[Install]
WantedBy=multi-user.target
```
- Creates new systemd service
- Starts and enables custom app

### **Task 2.12-2.13: View Service Logs**
```bash
journalctl -u httpd -n 20 --no-pager
```
- Shows last 20 lines of httpd logs
- Helps diagnose service issues

---

## 🚀 Quick Start

### **Check Syntax**
```bash
ansible-playbook topic2-service-management.yml --syntax-check
```

### **Run Playbook**
```bash
ansible-playbook topic2-service-management.yml
```

### **Run on Specific Host**
```bash
ansible-playbook topic2-service-management.yml -l node1
```

### **With Sudo Password**
```bash
ansible-playbook topic2-service-management.yml -K
```

---

## 📂 Output Locations

### **On Each Node**
```
/tmp/topic2_service_outputs/topic2_service_report_<hostname>.txt
```

### **On Master**
```
/root/playbooks/outputs/<hostname>_topic2_service.txt
```

---

## 📊 Report Contents

The generated report includes:

```
- httpd Installation status
- httpd Service status (active/inactive)
- httpd Enable status
- Failed services (if any)
- Custom service creation status
- Service logs (last 20 lines)
- Custom service file content
- Execution summary
```

---

## ✅ Expected Output

### **Console Output**
```
TASK [2.1 - Install httpd] ***
changed: [node1] => {"changed": true, ...}

TASK [2.2 - Start httpd service] ***
changed: [node1] => {"changed": true, ...}

TASK [2.3 - Enable httpd at boot] ***
changed: [node1] => {"changed": true, ...}

TASK [2.5 - Display service status] ***
ok: [node1] => {
    "msg": "httpd is: active"
}

[... more tasks ...]

TASK [2.18 - Display final summary] ***
ok: [node1] => {
    "msg": [
        "===========================================",
        "TOPIC 2: SERVICE MANAGEMENT COMPLETED",
        "===========================================",
        "Node: node1",
        "httpd Status: active",
        "httpd Enabled: YES",
        ...
    ]
}
```

---

## 🔧 Troubleshooting

### **httpd Service Not Starting**
- Check port 80 not in use: `netstat -tlnp | grep :80`
- Check for config errors: `httpd -t`
- Review logs: `journalctl -u httpd -n 50`

### **Service Won't Enable**
- Check if service file exists and is valid
- Try: `systemctl daemon-reload`

### **Failed Services Found**
- Check which services failed
- View logs: `journalctl -u <service> -n 50`
- Start manually: `systemctl start <service>`

### **Custom Service Not Starting**
- Check service file syntax
- Verify ExecStart path exists
- Check logs: `journalctl -u myapp -n 50`

---

## 💡 Pro Tips

1. **Always check config before reload**
   ```bash
   httpd -t  # Validates Apache config
   ```

2. **Use graceful reload when possible**
   ```bash
   systemctl reload httpd  # No downtime
   systemctl restart httpd # Downtime
   ```

3. **Monitor service status**
   ```bash
   systemctl status httpd
   ```

4. **View service logs**
   ```bash
   journalctl -u httpd -f  # Follow logs
   ```

---

## 🎯 Common Use Cases

### **Use Case 1: Deploy Apache on New Server**
```bash
# Run this playbook to install and configure
ansible-playbook topic2-service-management.yml
```

### **Use Case 2: Check Service Health**
```bash
# Run and review report
ansible-playbook topic2-service-management.yml
cat /root/playbooks/outputs/node1_topic2_service.txt
```

### **Use Case 3: Create Custom Service**
```bash
# Playbook creates myapp service automatically
# You can modify /etc/systemd/system/myapp.service
```

---

## 📋 Task Summary

| Task | Purpose | Output |
|------|---------|--------|
| 2.1 | Install httpd | Package installed |
| 2.2 | Start service | Service running |
| 2.3 | Enable at boot | Auto-start enabled |
| 2.4-2.5 | Check status | active/inactive |
| 2.6 | Reload service | Config reloaded |
| 2.7-2.8 | Failed services | List of failed services |
| 2.9-2.11 | Custom service | Service created & started |
| 2.12-2.13 | View logs | Service logs |
| 2.14-2.18 | Reporting | Report generated |

---

## ✨ Key Features

✅ Installs Apache HTTP Server
✅ Starts service immediately
✅ Enables service at boot
✅ Checks service status
✅ Graceful reload capability
✅ Lists failed services
✅ Creates custom service
✅ Views service logs
✅ Automatic reporting
✅ Master backup copy

---

## 🔐 Security Considerations

1. **Firewall**: May need to allow port 80
   ```bash
   firewall-cmd --add-service=http --permanent
   firewall-cmd --reload
   ```

2. **SELinux**: May need to adjust contexts
   ```bash
   getenforce  # Check status
   ```

3. **Service User**: Consider running as non-root
   ```bash
   # Modify service file User=apache
   ```

---

## 🚀 Run Now

```bash
ansible-playbook topic2-service-management.yml
```

**Service management automated!** ✅

