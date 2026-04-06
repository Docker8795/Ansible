# Boot Configuration Playbook - Syntax & Usage Guide

## ✅ Syntax Corrections Made

### 1. **Playbook Header** (Fixed)
```yaml
---
- hosts: all
  become: yes
  gather_facts: yes
  tasks:
```
✅ Added proper YAML document start (`---`)
✅ Each key properly indented at same level
✅ Colon and space after each key

---

## 🎯 Task Breakdown

### Task 1.1 - Show Current Boot Target
```yaml
- name: 1.1 - Show current default boot target
  ansible.builtin.command: systemctl get-default
  register: current_target
  changed_when: false
```
- **Command:** Gets current boot target
- **Output:** Stored in `current_target.stdout`
- **Status:** OK (read-only, no changes)

---

### Task 1.2 - Display Output
```yaml
- name: 1.2 - Display current boot target
  ansible.builtin.debug:
    msg: "Current default target is: {{ current_target.stdout }}"
```
- **Shows:** Boot target during execution
- **Variable:** `{{ current_target.stdout }}` displays the output

---

### Task 1.3 - Set Boot Target
```yaml
- name: 1.3 - Set default boot target to multi-user
  ansible.builtin.command: systemctl set-default multi-user.target
  register: set_target
  changed_when: "'Created symlink' in set_target.stderr or 'Removed' in set_target.stderr"
```
- **Command:** Changes boot target to CLI mode
- **Detection:** Marks as changed only if specific messages appear
- **Status:** Depends on actual changes made

---

### Task 1.4 - Check GRUB File
```yaml
- name: 1.4 - Check GRUB2 configuration file exists
  ansible.builtin.stat:
    path: /etc/default/grub
  register: grub_file
```
- **Checks:** If GRUB config file exists
- **Output:** `grub_file.stat.exists` = true/false

---

### Task 1.6 - Backup GRUB
```yaml
- name: 1.6 - Backup GRUB configuration
  ansible.builtin.copy:
    src: /etc/default/grub
    dest: /etc/default/grub.backup
    remote_src: yes
    mode: '0644'
  when: grub_file.stat.exists
```
- **Condition:** Only runs if GRUB file exists
- **Action:** Creates backup copy
- **`remote_src: yes`:** Copies on remote host (not from master)

---

### Task 1.7 - Modify GRUB
```yaml
- name: 1.7 - Set GRUB timeout to 5 seconds
  ansible.builtin.lineinfile:
    path: /etc/default/grub
    regexp: '^GRUB_TIMEOUT='
    line: 'GRUB_TIMEOUT=5'
    state: present
  when: grub_file.stat.exists
```
- **Finds:** Line starting with `GRUB_TIMEOUT=`
- **Replaces:** With `GRUB_TIMEOUT=5`
- **Condition:** Only if GRUB file exists

---

### Task 1.8 - Regenerate GRUB
```yaml
- name: 1.8 - Regenerate GRUB2 config
  ansible.builtin.command: grub2-mkconfig -o /boot/grub2/grub.cfg
  when: ansible_os_family == "RedHat"
  changed_when: true
```
- **Condition:** Only runs on RedHat/CentOS systems
- **`ansible_os_family`:** Built-in variable for OS type
- **Action:** Regenerates GRUB2 boot menu

---

### Task 1.9 - Check Boot Errors
```yaml
- name: 1.9 - Check last boot logs (dmesg)
  ansible.builtin.command: dmesg --level=err,warn
  register: boot_errors
  changed_when: false
  failed_when: false
```
- **Gets:** Error and warning logs
- **`failed_when: false`:** Won't fail even if errors found
- **Output:** Stored in `boot_errors`

---

### Task 1.11 - Check Uptime
```yaml
- name: 1.11 - Check system uptime
  ansible.builtin.command: uptime
  register: uptime_result
  changed_when: false
```
- **Gets:** System uptime information
- **Output:** Stored in `uptime_result.stdout`

---

## 📊 OUTPUT CAPTURE (Tasks 1.13-1.17)

### Task 1.13 - Create Output Directory
```yaml
- name: 1.13 - Create output directory on each node
  ansible.builtin.file:
    path: /tmp/ansible_outputs
    state: directory
    mode: '0755'
```
- **Creates:** `/tmp/ansible_outputs/` directory

---

### Task 1.14 - Save All Outputs to File
```yaml
- name: 1.14 - Save all task outputs to file
  ansible.builtin.copy:
    content: |
      (formatted output report)
    dest: /tmp/ansible_outputs/playbook_output_{{ inventory_hostname }}.txt
    mode: '0644'
```
- **Creates:** File with all task outputs
- **Location:** `/tmp/ansible_outputs/playbook_output_<nodename>.txt`
- **On:** Each node

---

### Task 1.16 - Fetch to Master
```yaml
- name: 1.16 - Fetch output files to Ansible master
  ansible.builtin.fetch:
    src: /tmp/ansible_outputs/playbook_output_{{ inventory_hostname }}.txt
    dest: /root/playbooks/outputs/{{ inventory_hostname }}_playbook_output.txt
    flat: yes
```
- **Copies:** Output from nodes to master
- **Location on Master:** `/root/playbooks/outputs/<nodename>_playbook_output.txt`
- **`flat: yes`:** Saves in flat directory (not nested by hostname)

---

## 🚀 How to Run

### 1. Syntax Check
```bash
ansible-playbook boot_complete_playbook.yaml --syntax-check
```

### 2. Run Playbook
```bash
ansible-playbook boot_complete_playbook.yaml
```

### 3. Run with Verbose Output
```bash
ansible-playbook boot_complete_playbook.yaml -v
```

### 4. Run with Extra Verbose
```bash
ansible-playbook boot_complete_playbook.yaml -vv
```

### 5. View Output on Nodes
```bash
cat /tmp/ansible_outputs/playbook_output_node1.txt
cat /tmp/ansible_outputs/playbook_output_node2.txt
```

### 6. View Output on Master
```bash
cat /root/playbooks/outputs/node1_playbook_output.txt
cat /root/playbooks/outputs/node2_playbook_output.txt
```

---

## 📋 Variable Reference

| Variable | Contains |
|----------|----------|
| `current_target.stdout` | Boot target name |
| `set_target.stderr` | Symlink creation message |
| `set_target.rc` | Return code (0=success) |
| `grub_file.stat.exists` | true/false |
| `boot_errors.stdout_lines` | List of error lines |
| `uptime_result.stdout` | Uptime text |
| `inventory_hostname` | Current node name |
| `ansible_date_time.iso8601` | Current timestamp |
| `ansible_os_family` | OS type (RedHat, Debian, etc) |

---

## ✅ Common Issues & Fixes

### Issue 1: Syntax Error
```bash
# Check syntax
ansible-playbook boot_complete_playbook.yaml --syntax-check

# Fix indentation (2 spaces per level)
```

### Issue 2: GRUB File Not Found
- Task 1.6 & 1.7 will be skipped
- Check with: `ansible all -m stat -a "path=/etc/default/grub"`

### Issue 3: grub2-mkconfig Command Not Found
- Task 1.8 only runs on RedHat systems
- For Debian: Use `update-grub` instead

### Issue 4: Output Files Not Created
- Check directory permissions: `ls -la /tmp/ansible_outputs/`
- Check master directory: `mkdir -p /root/playbooks/outputs/`

---

## 🎯 Expected Output Structure

### On Node (/tmp/ansible_outputs/playbook_output_node1.txt):
```
==========================================
ANSIBLE PLAYBOOK EXECUTION REPORT
==========================================
Node: node1
Execution Date: 2026-04-05T10:30:45.123456+00:00

========== TASK OUTPUTS ==========

Task 1.1 - Current Boot Target:
Status: OK
Output: multi-user.target

Task 1.2 - Boot Target Display:
Status: OK
Message: Current default target is: multi-user.target

[... more tasks ...]

========== SUMMARY ==========
Total Tasks: 12
All Tasks Completed: YES
==========================================
```

### On Master (/root/playbooks/outputs/):
```
node1_playbook_output.txt
node2_playbook_output.txt
```

---

## 💡 Pro Tips

1. **Always backup files** before modification (Task 1.6 does this)
2. **Use `changed_when: false`** for read-only commands
3. **Use `failed_when: false`** if command might fail but it's OK
4. **Use `when:`** conditions to skip irrelevant tasks
5. **Register variables** to capture output
6. **Use `debug:`** to display during execution
7. **Use `fetch:`** to copy results to master
8. **Check `ansible_os_family`** before OS-specific commands

---

## 📞 Quick Commands

```bash
# Validate syntax
ansible-playbook boot_complete_playbook.yaml --syntax-check

# Run playbook
ansible-playbook boot_complete_playbook.yaml

# View on nodes
for i in 1 2; do echo "=== Node$i ==="; ssh node$i cat /tmp/ansible_outputs/playbook_output_node$i.txt; done

# View on master
cat /root/playbooks/outputs/node*.txt
```

