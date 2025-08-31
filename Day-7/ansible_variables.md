# 📝 Variables in Ansible: Types and Precedence

Variables in Ansible are used to **store values** (strings, lists, dictionaries, etc.) that can be reused across playbooks, roles, and tasks. They make playbooks dynamic, flexible, and reusable.

---

## 1. Types of Variables in Ansible

Ansible supports different types of variables based on where they are defined and how they are used.

### 🔹 1.1 Playbook Variables
Defined directly inside a playbook under `vars`.

```yaml
- hosts: all
  vars:
    http_port: 80
  tasks:
    - name: Print HTTP Port
      debug:
        msg: "The web server will run on port {{ http_port }}"
```

---

### 🔹 1.2 Inventory Variables
Defined in the inventory file (`hosts`).

```ini
[web]
web1 ansible_host=192.168.1.10 http_port=8080
web2 ansible_host=192.168.1.11 http_port=9090
```

Playbook Example:
```yaml
- hosts: web
  tasks:
    - name: Show port from inventory
      debug:
        msg: "Port for {{ inventory_hostname }} is {{ http_port }}"
```

---

### 🔹 1.3 Host Variables
Defined in `host_vars/<hostname>.yml`.

```yaml
# host_vars/web1.yml
http_port: 8081
```

---

### 🔹 1.4 Group Variables
Defined in `group_vars/<groupname>.yml`.

```yaml
# group_vars/web.yml
http_port: 8082
```

---

### 🔹 1.5 Role Variables
Variables defined inside a role:
- `defaults/main.yml` → lowest precedence
- `vars/main.yml` → higher precedence

```yaml
# roles/webserver/defaults/main.yml
http_port: 8083
```

```yaml
# roles/webserver/vars/main.yml
http_port: 8084
```

---

### 🔹 1.6 Registered Variables
Variables created from the output of a task.

```yaml
- name: Get hostname
  command: hostname
  register: result

- debug:
    msg: "The hostname is {{ result.stdout }}"
```

---

### 🔹 1.7 Facts
Facts are system information gathered by Ansible automatically.

```yaml
- hosts: all
  tasks:
    - debug:
        msg: "The OS family is {{ ansible_facts['os_family'] }}"
```

---

### 🔹 1.8 Extra Variables
Passed from the command line using `-e`.

```bash
ansible-playbook site.yml -e "http_port=8085"
```

---

## 2. Variable Precedence in Ansible

When the same variable is defined in multiple places, **precedence rules** decide which value is used.

### 📌 Precedence Order (Lowest → Highest)
1. **Role defaults** (`defaults/main.yml`)
2. **Inventory variables** (group_vars, host_vars, inventory file)
3. **Playbook variables** (`vars:` in playbook)
4. **Registered variables**
5. **Facts** (gathered by setup module)
6. **Role variables** (`vars/main.yml`)
7. **Block vars**
8. **Task vars (directly in task)**
9. **Extra vars** (`-e`) → **Highest precedence**

---

### Example of Precedence
```yaml
# group_vars/all.yml
http_port: 80

# host_vars/web1.yml
http_port: 81

# playbook.yml
- hosts: web
  vars:
    http_port: 82
  tasks:
    - name: Show variable value
      debug:
        msg: "Port is {{ http_port }}"
```

Command:
```bash
ansible-playbook playbook.yml -e "http_port=83"
```

**Result → Port is 83** (because extra vars win).

---

## 3. Data Types in Ansible Variables

Ansible variables can hold different data types:

### String
```yaml
site_name: "MyWebsite"
```

### Integer
```yaml
max_connections: 100
```

### Boolean
```yaml
enabled: true
```

### List
```yaml
packages:
  - nginx
  - git
  - curl
```

### Dictionary (Hash/Map)
```yaml
user:
  name: "alice"
  shell: "/bin/bash"
```

### Using Variables
```yaml
- debug:
    msg: "User {{ user.name }} uses {{ user.shell }}"
```

---

# ✅ Summary
- **Types of Variables:** Playbook, Inventory, Host, Group, Role, Registered, Facts, Extra.
- **Data Types:** String, Integer, Boolean, List, Dictionary.
- **Precedence:** Role defaults < Inventory vars < Playbook vars < Registered < Facts < Role vars < Block vars < Task vars < Extra vars.

Variables make Ansible playbooks **dynamic, flexible, and powerful** 🚀
