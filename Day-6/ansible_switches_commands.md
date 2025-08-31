# 🔹 Ansible Commands & Switches Guide

Ansible provides several command-line options (switches) that make execution flexible and powerful.  
Here are the most commonly used switches explained with examples.

---

## 1. Run a Playbook
```bash
ansible-playbook playbook.yml
```

---

## 2. Inventory Selection (-i)
Use a specific inventory file instead of the default (`/etc/ansible/hosts`):
```bash
ansible-playbook -i inventory.ini playbook.yml
```

---

## 3. Limit Hosts (-l)
Run playbook only on a specific host or group:
```bash
ansible-playbook -i inventory.ini playbook.yml -l node1
```

---

## 4. Check Mode (--check)
Dry-run mode (no changes are made, just shows what would happen):
```bash
ansible-playbook -i inventory.ini playbook.yml --check
```

---

## 5. Syntax Check (--syntax-check)
Validate playbook syntax before execution:
```bash
ansible-playbook playbook.yml --syntax-check
```

---

## 6. Verbosity (-v, -vv, -vvv, -vvvv)
Increase verbosity level for debugging:
```bash
ansible-playbook playbook.yml -v      # Basic details
ansible-playbook playbook.yml -vv     # More details (tasks)
ansible-playbook playbook.yml -vvv    # Connection details
ansible-playbook playbook.yml -vvvv   # Full debug (including SSH)
```

---

## 7. Become Root (--become / -b)
Run tasks with privilege escalation (sudo):
```bash
ansible-playbook playbook.yml --become
```

---

## 8. Run Specific Tags (--tags)
Execute only tasks with certain tags:
```bash
ansible-playbook playbook.yml --tags "install"
```

Skip tasks with certain tags:
```bash
ansible-playbook playbook.yml --skip-tags "configure"
```

---

## 9. Start at Task (--start-at-task)
Resume execution from a specific task name:
```bash
ansible-playbook playbook.yml --start-at-task="Install Nginx"
```

---

## 10. Ad-hoc Commands (Quick Execution)
Ping all hosts:
```bash
ansible all -i inventory.ini -m ping
```

Run a shell command on all hosts:
```bash
ansible all -i inventory.ini -m shell -a "uptime"
```

Install a package (example for Debian/Ubuntu):
```bash
ansible all -i inventory.ini -m apt -a "name=nginx state=present" --become
```

---

# ✅ Summary
- `-i` → Inventory file  
- `-l` → Limit hosts  
- `--check` → Dry run  
- `--syntax-check` → Validate playbook  
- `-v/-vv/-vvv/-vvvv` → Verbosity levels  
- `--become` → Run with sudo/root  
- `--tags` / `--skip-tags` → Filter tasks  
- `--start-at-task` → Resume from task  
