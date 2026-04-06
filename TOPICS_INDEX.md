# 📚 TOPIC-BASED ANSIBLE PLAYBOOKS - INDEX

## Overview

3 separate YAML playbooks, each focused on one topic:
- **Each topic is independent** - Run any topic standalone
- **Each topic has output capture** - Reports generated automatically
- **Each topic has detailed MD guide** - Step-by-step documentation

---

## 📋 Topic 1: Boot & GRUB Configuration

### **File:** `topic1-boot-grub.yml` (5.9 KB)

**16 Tasks | Boot target, GRUB setup, boot errors, uptime**

#### What it does:
- Gets current boot target (graphical/multi-user)
- Sets boot target to multi-user (CLI mode)
- Backs up GRUB configuration
- Sets GRUB timeout to 5 seconds
- Regenerates GRUB config
- Checks boot errors and warnings
- Shows system uptime

#### Output:
- **On Node:** `/tmp/topic1_boot_outputs/topic1_boot_report_<hostname>.txt`
- **On Master:** `/root/playbooks/outputs/<hostname>_topic1_boot.txt`

#### Run:
```bash
ansible-playbook topic1-boot-grub.yml
```

#### Documentation:
Read: `TOPIC1_BOOT_GRUB.md`

---

## 📋 Topic 2: Service Installation & Management

### **File:** `topic2-service-management.yml` (6.7 KB)

**18 Tasks | Service installation, start/stop/reload, enable/disable, custom services**

#### What it does:
- Installs Apache HTTP Server (httpd)
- Starts httpd service immediately
- Enables httpd at boot
- Checks service status
- Reloads service gracefully
- Lists failed services
- Creates custom systemd service
- Views service logs

#### Output:
- **On Node:** `/tmp/topic2_service_outputs/topic2_service_report_<hostname>.txt`
- **On Master:** `/root/playbooks/outputs/<hostname>_topic2_service.txt`

#### Run:
```bash
ansible-playbook topic2-service-management.yml
```

#### Documentation:
Read: `TOPIC2_SERVICE_MANAGEMENT.md`

---

## 📋 Topic 3: Service Troubleshooting & Diagnostics

### **File:** `topic3-diagnostics.yml` (11 KB)

**32 Tasks | 27 comprehensive diagnostic checks**

#### What it does:

**Network Diagnostics:**
- Port 80 listening check
- Firewall status
- SELinux status
- All open ports listing

**Process Diagnostics:**
- httpd process count
- Detailed service status
- Service enable status
- Custom service file verification

**Configuration Diagnostics:**
- Apache config syntax validation
- Apache version

**Resource Diagnostics:**
- Memory usage monitoring
- Disk space monitoring

**Log & Error Diagnostics:**
- System error logs (last 50 lines)

#### Output:
- **On Node:** `/tmp/topic3_diagnostics_outputs/topic3_diagnostics_report_<hostname>.txt`
- **On Master:** `/root/playbooks/outputs/<hostname>_topic3_diagnostics.txt`

#### Run:
```bash
ansible-playbook topic3-diagnostics.yml
```

#### Documentation:
Read: `TOPIC3_DIAGNOSTICS.md`

---

## 🚀 Quick Start

### **Run Individual Topics:**

#### Topic 1: Boot & GRUB
```bash
# Check syntax
ansible-playbook topic1-boot-grub.yml --syntax-check

# Run playbook
ansible-playbook topic1-boot-grub.yml

# View report
cat /root/playbooks/outputs/node1_topic1_boot.txt
```

#### Topic 2: Service Management
```bash
# Check syntax
ansible-playbook topic2-service-management.yml --syntax-check

# Run playbook
ansible-playbook topic2-service-management.yml

# View report
cat /root/playbooks/outputs/node1_topic2_service.txt
```

#### Topic 3: Diagnostics
```bash
# Check syntax
ansible-playbook topic3-diagnostics.yml --syntax-check

# Run playbook
ansible-playbook topic3-diagnostics.yml

# View report
cat /root/playbooks/outputs/node1_topic3_diagnostics.txt
```

---

## 🎯 Which Topic to Run?

### **Topic 1: Boot & GRUB** - Use when:
- You need to change boot target (GUI to CLI)
- You want to configure GRUB boot options
- You need to check boot errors
- You want to monitor system uptime

### **Topic 2: Service Management** - Use when:
- You need to install Apache
- You need to start/stop/reload services
- You need to enable services at boot
- You want to create custom services
- You need to check service logs

### **Topic 3: Diagnostics** - Use when:
- Service is not running and you need to troubleshoot
- You need comprehensive system checks
- You want to verify network connectivity
- You need to check resource usage
- You want to validate configuration
- You need to check security (SELinux, Firewall)

---

## 📊 Task Summary

| Topic | File | Tasks | Purpose |
|-------|------|-------|---------|
| **1** | topic1-boot-grub.yml | 16 | Boot configuration |
| **2** | topic2-service-management.yml | 18 | Service management |
| **3** | topic3-diagnostics.yml | 32 | Troubleshooting & diagnostics |
| | | **66 total** | Complete system management |

---

## 🚀 Running All Topics

### **Run all topics in sequence:**
```bash
# Run each topic one by one
ansible-playbook topic1-boot-grub.yml
ansible-playbook topic2-service-management.yml
ansible-playbook topic3-diagnostics.yml

# View all reports
cat /root/playbooks/outputs/node1_topic*.txt
```

### **Run on multiple hosts:**
```bash
# Run on all hosts in inventory
ansible-playbook topic1-boot-grub.yml
ansible-playbook topic2-service-management.yml
ansible-playbook topic3-diagnostics.yml

# View all reports for all hosts
ls /root/playbooks/outputs/
```

### **Run specific topics on specific hosts:**
```bash
# Run Topic 1 on node1 only
ansible-playbook topic1-boot-grub.yml -l node1

# Run Topic 2 on node2 only
ansible-playbook topic2-service-management.yml -l node2

# Run Topic 3 on all
ansible-playbook topic3-diagnostics.yml
```

---

## 📂 Output Organization

### **On Each Node:**
```
/tmp/topic1_boot_outputs/
├── topic1_boot_report_node1.txt
└── topic1_boot_report_node2.txt

/tmp/topic2_service_outputs/
├── topic2_service_report_node1.txt
└── topic2_service_report_node2.txt

/tmp/topic3_diagnostics_outputs/
├── topic3_diagnostics_report_node1.txt
└── topic3_diagnostics_report_node2.txt
```

### **On Master Node:**
```
/root/playbooks/outputs/
├── node1_topic1_boot.txt
├── node1_topic2_service.txt
├── node1_topic3_diagnostics.txt
├── node2_topic1_boot.txt
├── node2_topic2_service.txt
└── node2_topic3_diagnostics.txt
```

---

## ✅ Pre-Execution Checklist

Before running any playbook:
- [ ] Playbook file exists
- [ ] Inventory configured
- [ ] SSH access verified
- [ ] Sudo configured
- [ ] Output directory exists: `/root/playbooks/outputs/`

```bash
# Create output directory
mkdir -p /root/playbooks/outputs/
chmod 755 /root/playbooks/outputs/

# Verify inventory
ansible all -m ping

# Verify sudo
ansible all -m command -a "whoami"
```

---

## 💡 Common Commands

```bash
# Syntax check
ansible-playbook topic1-boot-grub.yml --syntax-check

# Run with verbose output
ansible-playbook topic2-service-management.yml -v

# Run on specific host
ansible-playbook topic3-diagnostics.yml -l node1

# Run with sudo password prompt
ansible-playbook topic1-boot-grub.yml -K

# Save output to log
ansible-playbook topic2-service-management.yml | tee execution.log

# View reports
cat /root/playbooks/outputs/*.txt

# Compare node reports
diff /root/playbooks/outputs/node1_topic1_boot.txt \
     /root/playbooks/outputs/node2_topic1_boot.txt

# Search for errors in report
grep -i "error" /root/playbooks/outputs/node1_topic3_diagnostics.txt
```

---

## 📚 Documentation Files

| Document | Content |
|----------|---------|
| **TOPIC1_BOOT_GRUB.md** | Complete guide for Topic 1 |
| **TOPIC2_SERVICE_MANAGEMENT.md** | Complete guide for Topic 2 |
| **TOPIC3_DIAGNOSTICS.md** | Complete guide for Topic 3 |

---

## 🎯 Use Cases

### **Scenario 1: Deploy Apache on New Server**
```bash
# Step 1: Configure boot
ansible-playbook topic1-boot-grub.yml

# Step 2: Install and manage Apache
ansible-playbook topic2-service-management.yml

# Step 3: Verify with diagnostics
ansible-playbook topic3-diagnostics.yml

# Step 4: Review reports
cat /root/playbooks/outputs/node1_topic*.txt
```

### **Scenario 2: Troubleshoot Service Issues**
```bash
# Run diagnostics
ansible-playbook topic3-diagnostics.yml

# Review detailed report
cat /root/playbooks/outputs/node1_topic3_diagnostics.txt | grep -i "error\|warning\|failed"

# Fix issues based on findings
# Re-run diagnostics to verify
ansible-playbook topic3-diagnostics.yml
```

### **Scenario 3: Monitor Regular Health**
```bash
# Run all topics daily
0 2 * * * ansible-playbook /path/to/topic1-boot-grub.yml
0 2 * * * ansible-playbook /path/to/topic2-service-management.yml
0 2 * * * ansible-playbook /path/to/topic3-diagnostics.yml
```

---

## ✨ Key Features

✅ **Modular** - Each topic is independent
✅ **Executable** - Direct execution, no modifications needed
✅ **Documented** - Each topic has detailed MD guide
✅ **Auto-reporting** - Reports generated automatically
✅ **Master Backup** - Reports copied to master
✅ **Comprehensive** - 66 total tasks covering all areas
✅ **Flexible** - Run any topic, any combination, any time

---

## 🎉 Ready to Use!

**Download and run any topic:**

```bash
# Topic 1
ansible-playbook topic1-boot-grub.yml

# Topic 2
ansible-playbook topic2-service-management.yml

# Topic 3
ansible-playbook topic3-diagnostics.yml
```

**All reports generated automatically!** ✅

---

## 📞 Support

- For Topic 1 questions: See `TOPIC1_BOOT_GRUB.md`
- For Topic 2 questions: See `TOPIC2_SERVICE_MANAGEMENT.md`
- For Topic 3 questions: See `TOPIC3_DIAGNOSTICS.md`

