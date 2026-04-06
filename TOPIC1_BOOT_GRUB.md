# 📋 TOPIC 1: BOOT & GRUB CONFIGURATION

## File: `topic1-boot-grub.yml`

**16 Tasks | Boot target, GRUB setup, boot errors, uptime**

---

## 🎯 What This Playbook Does

### **Task 1.1-1.2: Get Current Boot Target**
```bash
systemctl get-default
```
- Shows what boot target is currently set
- Output: `multi-user.target` (CLI) or `graphical.target` (GUI)

### **Task 1.3: Set Boot Target**
```bash
systemctl set-default multi-user.target
```
- Changes boot target to multi-user (CLI mode)
- One-time change

### **Task 1.4-1.5: Check GRUB File**
```bash
stat /etc/default/grub
```
- Verifies GRUB config file exists
- Shows file status

### **Task 1.6: Backup GRUB Config**
```bash
cp /etc/default/grub /etc/default/grub.backup
```
- Creates backup before any changes
- Safe for modifications

### **Task 1.7: Set GRUB Timeout**
```bash
# Edit /etc/default/grub
GRUB_TIMEOUT=5
```
- Sets boot menu timeout to 5 seconds
- Only runs if GRUB file exists

### **Task 1.8: Regenerate GRUB Config**
```bash
grub2-mkconfig -o /boot/grub2/grub.cfg
```
- Applies changes to GRUB boot menu
- RedHat/CentOS only

### **Task 1.9-1.10: Check Boot Errors**
```bash
dmesg --level=err,warn
```
- Shows boot errors and warnings
- Helps diagnose boot issues

### **Task 1.11-1.12: Check Uptime**
```bash
uptime
```
- Shows how long system has been running
- Useful for monitoring restarts

---

## 🚀 Quick Start

### **Check Syntax**
```bash
ansible-playbook topic1-boot-grub.yml --syntax-check
```

### **Run Playbook**
```bash
ansible-playbook topic1-boot-grub.yml
```

### **Run on Specific Host**
```bash
ansible-playbook topic1-boot-grub.yml -l node1
```

### **With Sudo Password**
```bash
ansible-playbook topic1-boot-grub.yml -K
```

---

## 📂 Output Locations

### **On Each Node**
```
/tmp/topic1_boot_outputs/topic1_boot_report_<hostname>.txt
```

### **On Master**
```
/root/playbooks/outputs/<hostname>_topic1_boot.txt
```

---

## 📊 Report Contents

The generated report includes:

```
- Current boot target (graphical/multi-user)
- Boot target change status
- GRUB file status (exists/not exists)
- GRUB backup status
- GRUB timeout configuration
- Boot errors and warnings
- System uptime
- Execution summary
```

---

## ✅ Expected Output

### **Console Output**
```
TASK [1.1 - Show current default boot target] ***
ok: [node1]

TASK [1.2 - Display current boot target] ***
ok: [node1] => {
    "msg": "Current default target is: multi-user.target"
}

[... more tasks ...]

TASK [1.17 - Display final summary] ***
ok: [node1] => {
    "msg": [
        "=============================================",
        "TOPIC 1: BOOT CONFIGURATION COMPLETED",
        "=============================================",
        "Node: node1",
        "Boot Target: multi-user.target",
        "GRUB Configured: YES",
        "Boot Errors Found: NO",
        ...
    ]
}
```

---

## 🔧 Troubleshooting

### **Boot Target Not Changed**
- Check if you have sudo access
- May need to reboot for changes to take effect

### **GRUB File Not Found**
- Check if running on RedHat/CentOS system
- GRUB path may be different on other distros

### **Boot Errors Found**
- Check dmesg output in report
- Investigate drivers, modules, or hardware issues

### **Port 80 Not Accessible After Boot**
- Check firewall settings
- Verify SELinux context
- Check httpd configuration

---

## 💡 Pro Tips

1. **Always backup GRUB before changes**
   - This playbook does it automatically

2. **Understand boot targets**
   ```
   multi-user.target = CLI mode (no GUI)
   graphical.target = Desktop/GUI mode
   rescue.target = Single user (emergency)
   ```

3. **Check boot errors regularly**
   - Helps prevent future issues

4. **Monitor uptime**
   - Know when system was last rebooted

---

## 🎯 Common Use Cases

### **Use Case 1: Disable GUI on Server**
```bash
# Run this playbook to set boot target to multi-user.target
ansible-playbook topic1-boot-grub.yml
```

### **Use Case 2: Check Boot Issues**
```bash
# Run and review boot errors in report
ansible-playbook topic1-boot-grub.yml
cat /root/playbooks/outputs/node1_topic1_boot.txt
grep -i "error\|warning" /root/playbooks/outputs/node1_topic1_boot.txt
```

### **Use Case 3: Backup GRUB Before Changes**
```bash
# Playbook automatically creates backup
# Located at: /etc/default/grub.backup
```

---

## 📋 Task Summary

| Task | Purpose | Output |
|------|---------|--------|
| 1.1-1.2 | Get boot target | Current target (multi-user/graphical) |
| 1.3 | Set boot target | Change status |
| 1.4-1.5 | Check GRUB | File exists (yes/no) |
| 1.6 | Backup GRUB | Backup created |
| 1.7 | Set timeout | GRUB_TIMEOUT=5 |
| 1.8 | Regenerate GRUB | New config generated |
| 1.9-1.10 | Boot errors | dmesg output |
| 1.11-1.12 | Uptime | System running time |
| 1.13-1.17 | Reporting | Report generated |

---

## ✨ Key Features

✅ Gets current boot target
✅ Sets boot target to multi-user
✅ Backs up GRUB config
✅ Sets GRUB timeout
✅ Regenerates GRUB menu
✅ Checks boot errors
✅ Shows system uptime
✅ Automatic reporting
✅ Master backup copy

---

## 🚀 Run Now

```bash
ansible-playbook topic1-boot-grub.yml
```

**Boot configuration management automated!** ✅

