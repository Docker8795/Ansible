# CHAPTER 3 - FILE PERMISSIONS & ACL - EXECUTION GUIDE

## 📍 File Location

```
/mnt/user-data/outputs/CHAPTER-3-EXECUTABLE.yml
```

---

## 🚀 QUICK START (4 STEPS)

### Step 1: Check Syntax

```bash
ansible-playbook -i inventory.ini CHAPTER-3-EXECUTABLE.yml --syntax-check
```

### Step 2: Dry-Run (Preview Changes)

```bash
ansible-playbook -i inventory.ini CHAPTER-3-EXECUTABLE.yml --check -v
```

### Step 3: Execute the Playbook

```bash
ansible-playbook -i inventory.ini CHAPTER-3-EXECUTABLE.yml -v
```

### Step 4: Verify Results

```bash
ssh root@server1 'cat /tmp/chapter3_permissions_report.txt'
```

---

## 📋 ALL 30 TASKS AT A GLANCE

| Task | Name | Action |
|------|------|--------|
| 3.1 | Display Chapter 3 Header | Show task header |
| 3.2 | Create test directory structure | Create files/dirs with perms |
| 3.3 | Display file creation results | Show created items |
| 3.4 | Create content for config files | Add file content |
| 3.5 | Display file permissions | Explain octal notation |
| 3.6 | Verify file permissions | Use stat module |
| 3.7 | Display verified permissions | Show results |
| 3.8 | Install ACL tools | Install acl, attr packages |
| 3.9 | Display ACL installation | Show installed tools |
| 3.10 | Apply ACL rules | Use ansible.posix.acl |
| 3.11 | Display ACL application | Show applied rules |
| 3.12 | Get ACL for all files | Run getfacl |
| 3.13 | Display ACL details | Show getfacl output |
| 3.14 | Create scripts with special perms | Create SUID/SGID/sticky |
| 3.15 | Display special permissions | Explain SUID/SGID/sticky |
| 3.16 | Verify special permissions | Run ls -la |
| 3.17 | Display verification results | Show ls output |
| 3.18 | Show permission symbols | Explain rwx notation |
| 3.19 | Demonstrate permission changes | chmod/chown examples |
| 3.20 | Create recursive permission | Set recursive ownership |
| 3.21 | Set secure directory perms | Create .ssh, .gnupg, etc |
| 3.22 | Display secure permissions | Explain secure configs |
| 3.23 | Create umask demo | Explain umask concept |
| 3.24 | Create permission matrix | Show permission tables |
| 3.25 | Verify all permissions | Find all files |
| 3.26 | Display file listing | Run ls -laR |
| 3.27 | Display detailed listing | Show ls output |
| 3.28 | Create permissions report | Generate report file |
| 3.29 | Display report location | Show report path |
| 3.30 | Show final summary | Execution summary |

---

## 🎯 FILES & DIRECTORIES CREATED

### Test Files Created

```
/opt/app                    (directory, 0755)
├─ config.yml              (file, 0640)
└─ data/                   (directory, 0700)

/var/log/app/              (directory, 0755)

/opt/app/sensitive_script.sh    (script with SGID, 2755)
/tmp/shared_dir/           (sticky bit, 1777)
/opt/project/              (directory, 0755, recursive)
```

### Security Directories Created

```
/root/.ssh                 (0700, root only)
/root/.gnupg               (0700, root only)
/etc/ssl/private           (0700, root only)
```

---

## 🔐 ACL RULES APPLIED

| Path | Entity | Type | Permissions | Description |
|------|--------|------|-------------|-------------|
| /opt/app | sysadmin | user | rx | Read+execute for sysadmin |
| /opt/app/data | backup | group | rx | Read+execute for backup group |
| /opt/app/config.yml | devuser | user | r | Read only for devuser |

---

## 🎓 PERMISSIONS EXPLAINED

### Basic Permissions

```
read (r)    = 4
write (w)   = 2
execute (x) = 1

Example: 0755
- Owner:   7 = rwx (4+2+1)
- Group:   5 = r-x (4+0+1)
- Others:  5 = r-x (4+0+1)
```

### Common Octal Permissions

```
0700 rwx------  (owner only)
0600 rw-------  (owner only, no execute)
0755 rwxr-xr-x  (standard directory)
0644 rw-r--r--  (standard file)
0640 rw-r-----  (sensitive file)
0660 rw-rw----  (group writable)
```

### Special Permissions

**SUID (Set User ID) - 4xxx**
- Only for executables
- File runs as owner's privileges
- Mode: 4755 = rwsr-xr-x
- Example: /usr/bin/passwd

**SGID (Set Group ID) - 2xxx**
- For files: runs as group
- For directories: files inherit group
- Mode: 2755 = rwxr-sr-x
- Example: shared group directories

**Sticky Bit - 1xxx**
- Only for directories
- Only owner can delete
- Mode: 1777 = rwxrwxrwt
- Example: /tmp

---

## 📊 WHAT GETS CREATED

### Files (4)
```bash
/opt/app/config.yml         (rw-r-----, 640)
/opt/app/sensitive_script.sh (rwxr-sr-x, 2755)
/tmp/shared_dir/            (rwxrwxrwt, 1777)
/opt/project/               (rwxr-xr-x, 755)
```

### Directories (4)
```bash
/opt/app                    (rwxr-xr-x, 755)
/opt/app/data               (rwx------, 700)
/var/log/app                (rwxr-xr-x, 755)
/opt/project                (rwxr-xr-x, 755, recursive)
```

### Security Directories (3)
```bash
/root/.ssh                  (rwx------, 700)
/root/.gnupg                (rwx------, 700)
/etc/ssl/private            (rwx------, 700)
```

---

## 🔍 VERIFICATION COMMANDS

### Check Basic Permissions

```bash
# View permissions in ls format
ls -la /opt/app

# View permissions in octal
stat /opt/app/config.yml

# Find files by permission
find /opt -perm 0755

# Show octal permissions
ls -l /opt/app/config.yml | awk '{print $1}'
```

### Check ACL

```bash
# Get ACL for file
getfacl /opt/app

# Get ACL for directory
getfacl -R /opt/app

# Get all extended attributes
getfattr -d /opt/app

# List ACL in detail
setfacl -m g:backup:rx /opt/app && getfacl /opt/app
```

### Check Special Permissions

```bash
# Show special bits
ls -la /opt/app/sensitive_script.sh
ls -la /tmp/shared_dir

# Check SUID files
find / -perm -4000 -type f

# Check SGID files
find / -perm -2000 -type f

# Check sticky bit
find / -perm -1000 -type d
```

### Test Permissions as Different Users

```bash
# Test as devuser
sudo -u devuser cat /opt/app/config.yml
sudo -u devuser ls -la /opt/app/data

# Test as sysadmin
sudo -u sysadmin ls /opt/app
sudo -u sysadmin cat /opt/app/config.yml

# Test as backup user
sudo -u backupuser ls /opt/app/data
```

### View Report

```bash
# View generated report
cat /tmp/chapter3_permissions_report.txt

# Copy report to control node
scp root@server1:/tmp/chapter3_permissions_report.txt ./
```

---

## 🔄 EXECUTION EXAMPLES

### Example 1: Run on all hosts

```bash
ansible-playbook -i inventory.ini CHAPTER-3-EXECUTABLE.yml -v
```

### Example 2: Run on specific host

```bash
ansible-playbook -i inventory.ini CHAPTER-3-EXECUTABLE.yml --limit server1 -v
```

### Example 3: Maximum verbosity

```bash
ansible-playbook -i inventory.ini CHAPTER-3-EXECUTABLE.yml -vvvv
```

### Example 4: Check mode (dry-run)

```bash
ansible-playbook -i inventory.ini CHAPTER-3-EXECUTABLE.yml --check -v
```

### Example 5: Run specific task

```bash
# List all tasks first
ansible-playbook -i inventory.ini CHAPTER-3-EXECUTABLE.yml --list-tasks

# Run specific task
ansible-playbook -i inventory.ini CHAPTER-3-EXECUTABLE.yml --start-at-task "3.8 - Install ACL tools" -v
```

---

## ⏱️ EXECUTION TIME

| Section | Time |
|---------|------|
| Tasks 3.1-3.7 (Basic Setup) | ~2 minutes |
| Tasks 3.8-3.13 (ACL Setup) | ~2 minutes |
| Tasks 3.14-3.22 (Special Perms) | ~2 minutes |
| Tasks 3.23-3.27 (Examples) | ~1 minute |
| Tasks 3.28-3.30 (Reporting) | ~1 minute |
| **TOTAL** | **~8-12 minutes** |

---

## 📈 EXPECTED OUTPUT

### Task 3.3 - File Creation
```
==========================================
FILE & DIRECTORY CREATION
==========================================
✓ /opt/app
  - Type: directory
  - Owner: appuser:appgroup
  - Permissions: 0755
  - Description: Application directory

✓ /opt/app/config.yml
  - Type: file
  - Owner: appuser:appgroup
  - Permissions: 0640
  - Description: Application config file

... (more files)
==========================================
```

### Task 3.11 - ACL Application
```
==========================================
ACL RULES APPLIED
==========================================
✓ /opt/app
  - Entity: sysadmin (user)
  - Permissions: rx
  - Description: Allow sysadmin to read and execute

✓ /opt/app/data
  - Entity: backup (group)
  - Permissions: rx
  - Description: Allow backup group to read and execute

✓ /opt/app/config.yml
  - Entity: devuser (user)
  - Permissions: r
  - Description: Allow devuser to read config

==========================================
```

### Task 3.27 - File Listing
```
==========================================
DETAILED FILE LISTING
==========================================
total 20
drwxr-xr-x  4 root    root    4096 Apr  6 14:00 .
drwxr-xr-x  5 root    root    4096 Apr  6 14:00 ..
drwxr-xr-x  2 appuser appgroup 4096 Apr  6 14:00 app
-rw-r-----  1 appuser appgroup  100 Apr  6 14:00 app/config.yml
drwx------  2 appuser appgroup 4096 Apr  6 14:00 app/data
...
==========================================
```

### Task 3.30 - Final Summary
```
==========================================
✓ CHAPTER 3 EXECUTION COMPLETE
==========================================

EXECUTION STATISTICS:
- Total Tasks Executed: 30
- Host: server1
- OS: RedHat 8.5

SUMMARY:

FILES & DIRECTORIES CREATED: 4
✓ /opt/app (0755)
✓ /opt/app/config.yml (0640)
✓ /opt/app/data (0700)
✓ /var/log/app (0755)

ACL RULES APPLIED: 3
✓ sysadmin:rx on /opt/app
✓ backup:rx on /opt/app/data
✓ devuser:r on /opt/app/config.yml

SPECIAL PERMISSIONS: 2
✓ /opt/app/sensitive_script.sh (2755)
✓ /tmp/shared_dir/ (1777)

Status: ✓ SUCCESS
==========================================
```

---

## ⚠️ IMPORTANT NOTES

### Before Running

✅ **Update inventory.ini** with your server IPs  
✅ **Test connectivity** with `ansible all -m ping`  
✅ **Check syntax** with `--syntax-check`  
✅ **Run dry-run** with `--check`  
✅ **Ensure Chapter 2 ran first** (users/groups must exist)

### What Gets Modified

⚠️ `/opt/app/` - Files and directories created  
⚠️ `/var/log/app/` - Log directory created  
⚠️ `/opt/project/` - Directory created  
⚠️ Ownership changed to appuser/developers/etc  
⚠️ ACL rules applied  
⚠️ Special permissions set (SUID/SGID/sticky)  

### Safe to Run

✅ Non-destructive (no deletions)  
✅ Idempotent (safe to run multiple times)  
✅ All operations documented  
✅ Report generated for reference  

### Important Security Notes

⚠️ **SSH keys**: Always 0600  
⚠️ **SSL private keys**: Always 0600  
⚠️ **Configuration with passwords**: Maximum 0640  
⚠️ **Public directories**: Never 0777  
⚠️ **ACL overrides basic permissions**: Be careful!  

---

## 🆘 TROUBLESHOOTING

### ACL Not Working

```bash
# Check if filesystem supports ACL
mount | grep acl

# Enable ACL if needed (for ext4)
mount -o remount,acl /

# Verify ACL module loaded
lsmod | grep acl

# Check ACL is applied
getfacl /opt/app
```

### Permission Denied Issues

```bash
# Check current permissions
ls -la /opt/app

# Check user membership
groups devuser

# Test as specific user
sudo -u devuser ls -la /opt/app

# Check effective permissions
getfacl /opt/app
```

### Special Permissions Not Showing

```bash
# Check with ls
ls -la /opt/app/sensitive_script.sh

# Should show 's' for SGID or 't' for sticky
# Example: rwxr-sr-x (SGID) or rwxrwxrwt (sticky)

# Set manually if needed
chmod 2755 /opt/app/sensitive_script.sh
chmod 1777 /tmp/shared_dir
```

### Verify Operations

```bash
# Check syntax
ansible-playbook -i inventory.ini CHAPTER-3-EXECUTABLE.yml --syntax-check

# Dry-run
ansible-playbook -i inventory.ini CHAPTER-3-EXECUTABLE.yml --check -v

# Execute
ansible-playbook -i inventory.ini CHAPTER-3-EXECUTABLE.yml -v
```

---

## 🎯 NEXT STEPS

1. ✅ Run this playbook successfully
2. ✅ Verify all files and permissions created
3. ✅ Test access with different users
4. ✅ Verify ACL rules work correctly
5. ✅ Check special permissions behavior
6. ✅ Review generated report
7. ✅ Document custom permissions in your environment
8. ✅ Proceed to Chapter 4: (Next topic)

---

## 📚 RELATED COMMANDS

```bash
# Permissions
chmod, chown, chgrp, stat, ls -la

# ACL
getfacl, setfacl, getfattr, setfattr

# Find
find -perm, find -type

# Security
umask, rwx notation, octal values
```

---

## ✅ VERIFICATION CHECKLIST

After execution:
- [ ] All 4 files/directories created
- [ ] Permissions set correctly (verify with stat)
- [ ] ACL rules applied (verify with getfacl)
- [ ] Special permissions working
- [ ] Test access with different users successful
- [ ] Report generated at /tmp/chapter3_permissions_report.txt
- [ ] Manual verification commands executed
- [ ] All permissions follow security best practices

---

**Ready to Execute Chapter 3!** 🚀

```bash
ansible-playbook -i inventory.ini CHAPTER-3-EXECUTABLE.yml -v
```

