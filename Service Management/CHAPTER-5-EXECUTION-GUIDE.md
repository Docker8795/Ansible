# CHAPTER 5 - SERVICE MANAGEMENT - EXECUTION GUIDE

## 📍 File Location

```
/mnt/user-data/outputs/CHAPTER-5-EXECUTABLE.yml
```

---

## 🚀 QUICK START (4 STEPS)

### Step 1: Check Syntax

```bash
ansible-playbook -i inventory.ini CHAPTER-5-EXECUTABLE.yml --syntax-check
```

### Step 2: Dry-Run (Preview Changes)

```bash
ansible-playbook -i inventory.ini CHAPTER-5-EXECUTABLE.yml --check -v
```

### Step 3: Execute the Playbook

```bash
ansible-playbook -i inventory.ini CHAPTER-5-EXECUTABLE.yml -v
```

### Step 4: Verify Results

```bash
ssh root@server1 'cat /tmp/chapter5_service_report.txt'
```

---

## 📋 ALL 40 TASKS AT A GLANCE

| Task | Name | Action |
|------|------|--------|
| 5.1 | Display Chapter 5 Header | Show task header |
| 5.2 | Gather systemd information | Explain systemd |
| 5.3 | List all units | Get all units |
| 5.4 | Display units count | Show count |
| 5.5 | Get current target | Show target |
| 5.6 | Display target info | Explain targets |
| 5.7 | Gather service facts | Get service info |
| 5.8 | Display service stats | Show statistics |
| 5.9 | Ensure critical services running | Manage services |
| 5.10 | Display management results | Show results |
| 5.11 | Verify service status | Check status |
| 5.12 | Display verification | Show output |
| 5.13 | Show dependencies | List dependencies |
| 5.14 | Display dependencies | Show details |
| 5.15 | Show failing services | List failed |
| 5.16 | Display failed services | Show results |
| 5.17 | Display journal | Get logs |
| 5.18 | Display journal output | Show logs |
| 5.19 | Demonstrate restart | Explain restart |
| 5.20 | Show socket services | List sockets |
| 5.21 | Display sockets | Show details |
| 5.22 | Show timer units | List timers |
| 5.23 | Display timers | Show details |
| 5.24 | Create custom service | Template service |
| 5.25 | Display service creation | Show results |
| 5.26 | Reload systemd | daemon-reload |
| 5.27 | Display reload | Show status |
| 5.28 | Show isolation | Security info |
| 5.29 | Display limits | Resource limits |
| 5.30 | Boot analysis | Performance |
| 5.31 | Display analysis | Show output |
| 5.32 | Common issues | Troubleshooting |
| 5.33 | Best practices | Guidelines |
| 5.34 | Create report | Generate report |
| 5.35 | Display report | Show location |
| 5.36 | Enable/disable info | Commands |
| 5.37 | Journalctl tips | Logging tips |
| 5.38 | Socket activation | Explain sockets |
| 5.39 | Target info | Runlevels |
| 5.40 | Execution summary | Final summary |

---

## 🎯 SERVICES MANAGED

### System Services (3)
- **sshd** (SSH Server) - Essential system service
- **firewalld** (Firewall Daemon) - Dynamic firewall
- **chronyd** (NTP Daemon) - Time synchronization

### Service States
- **State:** Started (running)
- **Enabled:** Yes (starts on boot)
- **Category:** System

---

## 📚 KEY SYSTEMD CONCEPTS

### Unit Types

```
.service  - Services (daemons, applications)
.socket   - Socket activation
.timer    - Scheduled tasks (like cron)
.target   - Grouping of units (like runlevels)
.mount    - Filesystem mounts
.slice    - Resource control groups
```

### Targets (Runlevels)

```
graphical.target    - GUI desktop (runlevel 5)
multi-user.target   - Command line (runlevel 3)
rescue.target       - Maintenance/single user (runlevel 1)
emergency.target    - Emergency shell
poweroff.target     - Shutdown (runlevel 0)
reboot.target       - Reboot (runlevel 6)
```

### Service Types

```
Type=simple         - Starts and stays running
Type=forking        - Forks a child process
Type=oneshot        - Runs once then exits
Type=notify         - Signals when ready
Type=dbus           - D-Bus activation
```

---

## 💻 ESSENTIAL SYSTEMD COMMANDS

### Service Management

```bash
# Start/stop/restart
systemctl start service-name
systemctl stop service-name
systemctl restart service-name
systemctl reload service-name         # Reload config only
systemctl reload-or-restart service   # Smart restart

# Enable/disable (boot startup)
systemctl enable service-name         # Start on boot
systemctl disable service-name        # Don't start on boot
systemctl is-enabled service-name     # Check if enabled

# Check status
systemctl status service-name
systemctl is-active service-name
```

### Service Information

```bash
# List services
systemctl list-units --type=service
systemctl list-units --failed
systemctl list-units --all

# Service details
systemctl show service-name
systemctl list-dependencies service-name
systemctl cat service-name            # Show service file
```

### Logging

```bash
# View logs
journalctl                            # All logs
journalctl -u service-name            # Specific service
journalctl -n 50                      # Last 50 lines
journalctl -f                         # Follow (tail)
journalctl -p err                     # Errors only
journalctl --since "1 hour ago"       # Time range
```

### Analysis

```bash
# Boot performance
systemd-analyze                       # Total boot time
systemd-analyze blame                 # Services by time
systemd-analyze critical-chain        # Slowest chain
systemd-analyze security service-name # Security check
systemd-analyze plot > boot.svg       # Boot graph
```

---

## 🔐 SERVICE SECURITY

### Isolation Options

```bash
PrivateTmp=yes              # Isolated /tmp per service
PrivateDevices=yes          # Limited device access
ProtectSystem=strict        # Read-only filesystems
ProtectHome=yes             # Hide /home and /root
NoNewPrivileges=yes         # No privilege escalation
```

### Capability Limiting

```bash
CapabilityBoundingSet=~CAP_SYS_ADMIN  # Remove admin cap
SystemCallFilter=@system-service      # Restrict syscalls
```

### Running as Non-Root

```bash
User=appuser                # Run as specific user
Group=appgroup              # Run as specific group
DynamicUser=yes             # Create temp user
```

---

## 📊 CUSTOM SERVICE FILE EXAMPLE

```ini
[Unit]
Description=My Application Service
After=network.target
Requires=network-online.target
Documentation=https://example.com/docs

[Service]
Type=simple
User=appuser
Group=appgroup
WorkingDirectory=/opt/app
ExecStart=/usr/local/bin/myapp
ExecReload=/bin/kill -HUP $MAINPID
Restart=always
RestartSec=10

# Security
PrivateTmp=yes
ProtectSystem=strict
NoNewPrivileges=yes

# Resource limits
CPUQuota=50%
MemoryLimit=512M
TasksMax=100

[Install]
WantedBy=multi-user.target
```

---

## ⏱️ EXECUTION TIME

| Section | Time |
|---------|------|
| Tasks 5.1-5.8 (Foundation) | ~2 minutes |
| Tasks 5.9-5.18 (Services) | ~3 minutes |
| Tasks 5.19-5.31 (Features) | ~3 minutes |
| Tasks 5.32-5.40 (Examples) | ~2 minutes |
| **TOTAL** | **~10-15 minutes** |

---

## 📈 EXPECTED OUTPUT

### Task 5.8 - Service Statistics
```
==========================================
SERVICE STATISTICS
==========================================
Total services: 387

Running services: 45

Enabled services: 52

Sample services:
- sshd: running
- firewalld: running
- chronyd: running
==========================================
```

### Task 5.12 - Service Verification
```
==========================================
SERVICE VERIFICATION
==========================================
SSH Server:
● sshd.service - OpenSSH server daemon
   Loaded: loaded (/usr/lib/systemd/system/sshd.service)
   Active: active (running) since Tue 2024-04-06 14:00:00 UTC

Firewall Daemon:
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service)
   Active: active (running) since Tue 2024-04-06 14:00:05 UTC

NTP Daemon:
● chronyd.service - NTP daemon
   Loaded: loaded (/usr/lib/systemd/system/chronyd.service)
   Active: active (running) since Tue 2024-04-06 14:00:10 UTC
==========================================
```

### Task 5.40 - Final Summary
```
==========================================
✓ CHAPTER 5 EXECUTION COMPLETE
==========================================

EXECUTION STATISTICS:
- Total Tasks Executed: 40
- Host: server1
- OS: RedHat 8.5

SERVICES MANAGED:
✓ sshd: System service (running)
✓ firewalld: Firewall daemon (running)
✓ chronyd: NTP daemon (running)

CONCEPTS COVERED:
✓ systemd basics
✓ Service management
✓ Dependencies
✓ Socket services
✓ Timers
✓ Custom services
✓ Security hardening
✓ Resource limiting
✓ Boot optimization
✓ Troubleshooting

Status: ✓ SUCCESS
==========================================
```

---

## 🔍 VERIFICATION COMMANDS

### Check Service Status

```bash
# Verify services are running
systemctl is-active sshd
systemctl is-active firewalld
systemctl is-active chronyd

# Check if enabled
systemctl is-enabled sshd
systemctl is-enabled firewalld

# Full status
systemctl status sshd
systemctl status firewalld
systemctl status chronyd
```

### View Service Logs

```bash
# Recent logs
journalctl -u sshd -n 20
journalctl -u firewalld -n 20
journalctl -u chronyd -n 20

# Follow logs
journalctl -u sshd -f
journalctl -u firewalld -f

# Errors only
journalctl -u sshd -p err
```

### Check Dependencies

```bash
# Service dependencies
systemctl list-dependencies sshd
systemctl list-dependencies firewalld

# Reverse dependencies
systemctl list-dependencies --reverse sshd
```

### Boot Analysis

```bash
# Boot time
systemd-analyze

# Slowest units
systemd-analyze blame | head -10

# Critical path
systemd-analyze critical-chain

# Security
systemd-analyze security sshd
```

### View Custom Service

```bash
# Show created service
systemctl cat myapp.service

# Show all service files
ls /etc/systemd/system/

# View service content
cat /etc/systemd/system/myapp.service
```

---

## ⚠️ IMPORTANT NOTES

### Before Running

✅ **Update inventory.ini** with your server IPs  
✅ **Test connectivity** with `ansible all -m ping`  
✅ **Check syntax** with `--syntax-check`  
✅ **Run dry-run** with `--check`  
✅ **Understand systemd** basics  

### What Gets Modified

⚠️ Service configuration reviewed  
⚠️ Custom service template created  
⚠️ systemd daemon reloaded  
⚠️ Reports generated  
⚠️ **No services actually stopped/restarted** (template only)

### Safe to Run

✅ Non-destructive (no service interruption)  
✅ Informational (mostly gathering data)  
✅ Template only (myapp service not actually created)  
✅ Report generated for reference  

### Important Notes

⚠️ **Template only:** Custom service file created but not enabled
⚠️ **No restarts:** Services are not restarted (demonstration)
⚠️ **Logging enabled:** Journal contains service history
⚠️ **Security focus:** Best practices demonstrated

---

## 🆘 TROUBLESHOOTING

### Service Won't Start

```bash
# Check logs
journalctl -u service-name -n 50

# Check status
systemctl status service-name

# Verify ExecStart
systemctl cat service-name | grep ExecStart

# Run manually
/path/to/executable
```

### Service Constantly Restarts

```bash
# Check exit code
journalctl -u service-name -p err

# Disable restart temporarily
systemctl stop service-name
# Edit /etc/systemd/system/service-name.service
# Set Restart=no
# systemctl daemon-reload

# Fix underlying issue
# Then set Restart=always again
```

### Service Not Starting on Boot

```bash
# Enable service
systemctl enable service-name

# Check WantedBy
systemctl cat service-name | grep WantedBy

# Check dependencies
systemctl list-dependencies service-name

# View target
systemctl get-default
```

### High Boot Time

```bash
# Analyze boot
systemd-analyze blame | head

# Optimize:
# 1. Check dependencies
# 2. Use socket activation
# 3. Set Type=notify if possible
# 4. Remove unnecessary services
```

---

## 📚 RELATED COMMANDS

```bash
systemctl, systemd-analyze, journalctl
systemd.unit, systemd.service documentation
man systemctl, man systemd.service
```

---

## ✅ VERIFICATION CHECKLIST

After execution:
- [ ] All 40 tasks executed successfully
- [ ] No errors in output
- [ ] Services running and enabled
- [ ] Custom service template created
- [ ] Report generated at /tmp/chapter5_service_report.txt
- [ ] Boot analysis completed
- [ ] Journal logs accessible
- [ ] Dependencies understood

---

## 🎯 NEXT STEPS

1. ✅ Run this playbook successfully
2. ✅ Verify services are running
3. ✅ Review service logs
4. ✅ Check boot performance
5. ✅ Create custom services as needed
6. ✅ Implement security hardening
7. ✅ Monitor with journalctl
8. ✅ Proceed to Chapter 6: Network Configuration

---

**Ready to Execute Chapter 5!** 🚀

```bash
ansible-playbook -i inventory.ini CHAPTER-5-EXECUTABLE.yml -v
```

