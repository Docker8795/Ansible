# CHAPTER 10 - TROUBLESHOOTING: COMMON ISSUES - EXECUTION GUIDE

## 📍 File Location

```
/mnt/user-data/outputs/CHAPTER-10-EXECUTABLE.yml
```

---

## 🚀 QUICK START

```bash
# Check Syntax
ansible-playbook -i inventory.ini CHAPTER-10-EXECUTABLE.yml --syntax-check

# Preview
ansible-playbook -i inventory.ini CHAPTER-10-EXECUTABLE.yml --check -v

# Execute
ansible-playbook -i inventory.ini CHAPTER-10-EXECUTABLE.yml -v
```

---

## 📋 ISSUE CATEGORIES

### System Performance
- High CPU usage
- High memory usage
- Disk full
- High load average

### Network Connectivity
- No internet connection
- DNS not working
- Slow network
- SSH issues

### Services & Daemons
- Service won't start
- Service crashes
- Port conflicts
- Permission denied

### Storage Issues
- Filesystem corruption
- Inode exhaustion
- Quota exceeded
- LVM problems

### Security
- Permission denied
- SELinux blocking
- Firewall issues
- SSH key problems

---

## 🔧 ESSENTIAL DIAGNOSTIC COMMANDS

### Performance
```bash
top                          # Process monitoring
vmstat 1 5                    # Memory/CPU stats
iostat -x 1 5                # I/O statistics
iotop -o                      # I/O by process
ps aux --sort=-%cpu | head   # CPU hogs
```

### Network
```bash
ip addr show                  # IP configuration
ip route show                 # Routing table
ping 8.8.8.8                 # Connectivity test
traceroute google.com        # Path trace
nslookup google.com          # DNS lookup
ss -tulpn                    # Listening ports
```

### Storage
```bash
df -h                        # Disk space
du -sh /*                    # Directory sizes
df -i                        # Inode usage
lsof /path                   # Files in use
fsck -n /dev/sda1           # Check filesystem
```

### Logging
```bash
journalctl -u service -n 50  # Service logs
journalctl -p err            # Errors only
tail -f /var/log/syslog     # Follow logs
grep pattern /var/log/*     # Search logs
```

---

## 🆘 QUICK FIXES

### High CPU
```bash
# Identify
top
ps aux --sort=-%cpu | head

# Fix
kill -9 PID
systemctl restart service
```

### High Memory
```bash
# Check
free -h
ps aux --sort=-%mem | head

# Fix
kill -9 PID
systemctl restart service
```

### Disk Full
```bash
# Find space
du -sh /* | sort -rh
find / -type f -size +100M

# Free space
rm -rf /var/log/*.old
rm -rf /var/cache/*
```

### No Connectivity
```bash
# Diagnose
ip addr show
ip route show
ping 8.8.8.8

# Fix
ip link set eth0 up
ip addr add 192.168.1.100/24 dev eth0
ip route add default via 192.168.1.1
```

### Service Won't Start
```bash
# Check
systemctl status service
journalctl -u service -n 50

# Fix
systemctl daemon-reload
systemctl start service
```

---

## 📊 TROUBLESHOOTING WORKFLOW

1. **Identify Problem**
   - Observe symptoms
   - Check error messages
   - Review recent changes

2. **Gather Information**
   - Check system logs
   - Monitor performance
   - Test connectivity

3. **Develop Hypothesis**
   - What could cause this?
   - What changed recently?
   - What's most likely?

4. **Test Hypothesis**
   - Make isolated changes
   - Test one thing at a time
   - Document observations

5. **Implement Solution**
   - Apply fix carefully
   - Test before production
   - Monitor results

6. **Verify & Document**
   - Confirm resolution
   - Check side effects
   - Document solution

---

## 📈 EXPECTED OUTPUT

```
✓ CHAPTER 10 EXECUTION COMPLETE

CONCEPTS COVERED:
✓ Troubleshooting methodology
✓ System performance issues
✓ Network connectivity problems
✓ Service and daemon issues
✓ Storage and filesystem problems
✓ Security and permission issues
✓ Diagnostic tools
✓ Log analysis techniques
✓ Quick recovery procedures
✓ Prevention strategies

Status: ✓ SUCCESS
```

---

**Ready to Execute Chapter 10!** 🚀

```bash
ansible-playbook -i inventory.ini CHAPTER-10-EXECUTABLE.yml -v
```

