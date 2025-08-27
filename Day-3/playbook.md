# 📘 Ansible Playbooks - Complete Guide

An **Ansible Playbook** is a YAML file that defines a set of automation tasks to be executed on managed nodes.  
Playbooks allow you to **orchestrate complex automation workflows** like installing packages, configuring services, managing files, and deploying applications.

---

## 🔹 1. Playbook Structure
A basic playbook is written in **YAML** and contains:

- **Name** → Description of the playbook or task
- **Hosts** → Target nodes (inventory group or hostname)
- **Become** → Whether to run tasks as `root` (sudo)
- **Tasks** → List of steps to run on the target machine(s)

Example skeleton:

```yaml
---
- name: Example Playbook
  hosts: all
  become: yes
  tasks:
    - name: Ensure Nginx is installed
      apt:
        name: nginx
        state: present
```

---

## 🔹 2. Key Components of a Playbook

### ✅ Hosts
Defines the **target systems** from your inventory file.

```yaml
hosts: managed_nodes
```

### ✅ Tasks
Each task runs a **module** (pre-built Ansible functionality).

```yaml
tasks:
  - name: Install Nginx
    apt:
      name: nginx
      state: present
```

### ✅ Handlers
Special tasks triggered only when notified (useful for service restarts).

```yaml
tasks:
  - name: Copy nginx config
    copy:
      src: nginx.conf
      dest: /etc/nginx/nginx.conf
    notify: Restart nginx

handlers:
  - name: Restart nginx
    service:
      name: nginx
      state: restarted
```

### ✅ Variables
Allow dynamic configuration.

```yaml
vars:
  package_name: nginx

tasks:
  - name: Install a package
    apt:
      name: "{{ package_name }}"
      state: present
```

### ✅ Become
Escalates privileges (like `sudo`).

```yaml
become: yes
```

---

## 🔹 3. Example Playbooks

### Example 1: Install Nginx

```yaml
---
- name: Install NGINX on managed nodes
  hosts: managed_nodes
  become: yes
  tasks:
    - name: Update apt repo and cache
      apt:
        update_cache: yes

    - name: Install nginx
      apt:
        name: nginx
        state: present
```

📖 **Explanation:**
1. Connects to `managed_nodes` defined in `inventory.ini`
2. Runs as root (`become: yes`)
3. Updates apt repo
4. Installs **nginx** package

---

### Example 2: Create a User

```yaml
---
- name: Create a user on managed nodes
  hosts: managed_nodes
  become: yes
  tasks:
    - name: Create user 'deploy'
      user:
        name: deploy
        state: present
        groups: sudo
        shell: /bin/bash
```

📖 **Explanation:**
- Uses the `user` module to create a **deploy** user  
- Adds user to `sudo` group  
- Default shell is `/bin/bash`  

---

### Example 3: Deploy an Application (Nginx + HTML Page)

```yaml
---
- name: Deploy a static website with Nginx
  hosts: managed_nodes
  become: yes

  tasks:
    - name: Install nginx
      apt:
        name: nginx
        state: present
        update_cache: yes

    - name: Copy index.html to server
      copy:
        src: files/index.html
        dest: /var/www/html/index.html

    - name: Ensure nginx is running
      service:
        name: nginx
        state: started
        enabled: yes
```

📖 **Explanation:**
1. Installs nginx  
2. Copies a custom `index.html` file from control node to web root  
3. Ensures nginx is running and enabled on boot  

---

## 🔹 4. Best Practices for Playbooks
- Always use `become: yes` for privileged tasks  
- Use **variables** instead of hardcoding values  
- Group tasks logically and use **handlers** for service restarts  
- Keep playbooks **idempotent** (re-runs should not break things)  
- Store sensitive data in **Ansible Vault**  

---

## ✅ Running a Playbook
Run using:

```bash
ansible-playbook -i inventory.ini playbook.yml
```

Example:

```bash
ansible-playbook -i inventory.ini install_nginx.yml
```

---

# 🎯 Summary
- Playbooks = **automation scripts in YAML**
- Contain: `hosts`, `tasks`, `handlers`, `variables`
- Enable repeatable, idempotent automation  
- Can handle **package installation, user management, file copy, service restart, and deployments**  

🚀 With playbooks, you can automate **entire infrastructure provisioning and app deployment** with ease!
