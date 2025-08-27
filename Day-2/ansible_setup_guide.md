# 🔹 Ansible Setup Guide (Ubuntu Control Node + Managed Nodes on AWS)

## 1. Environment Setup
- **Control Node (Ansible Host):** `54.145.168.225`
- **Managed Node 1:** `34.235.115.233`
- **Managed Node 2:** `50.17.140.17`

---

## 2. Install Ansible on Control Node
SSH into your control node:

```bash
ssh -i my-aws-key.pem ubuntu@54.145.168.225
```

Then install Ansible:

```bash
sudo apt update
sudo apt install ansible -y
```

---

## 3. Generate SSH Keys on Control Node
On the control node, generate SSH keys:

```bash
ssh-keygen -t rsa -b 4096
```

Press **Enter** for defaults (no passphrase required).  
This will create:

- Private key → `/home/ubuntu/.ssh/id_rsa`  
- Public key → `/home/ubuntu/.ssh/id_rsa.pub`  

Check:

```bash
ls -l ~/.ssh/
```

---

## 4. Copy Public Key to Managed Nodes
Since AWS EC2 blocks password login, we must **manually add the control node’s public key** to each managed node.

Display the public key on the control node:

```bash
cat ~/.ssh/id_rsa.pub
```

Example output:

```
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQD... ubuntu@ip-172-31-18-48
```

Now from your **local machine**, SSH into each managed node with your AWS key:

```bash
ssh -i my-aws-key.pem ubuntu@34.235.115.233
ssh -i my-aws-key.pem ubuntu@50.17.140.17
```

On each managed node, add the control node’s public key:

```bash
mkdir -p ~/.ssh
nano ~/.ssh/authorized_keys
```

Paste the key from **Step 1**.  
Save and set correct permissions:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

✅ Test SSH from the control node:

```bash
ssh ubuntu@34.235.115.233
ssh ubuntu@50.17.140.17
```

You should log in **without `.pem` or password**.

---

## 5. Create Ansible Inventory
On the control node, create a file **`inventory.ini`**:

```ini
[managed_nodes]
node1 ansible_host=34.235.115.233 ansible_user=ubuntu
node2 ansible_host=50.17.140.17 ansible_user=ubuntu
```

---

## 6. Test Ansible Connection
Run from control node:

```bash
ansible -i inventory.ini managed_nodes -m ping
```

Expected output:

```json
node1 | SUCCESS => {"changed": false, "ping": "pong"}
node2 | SUCCESS => {"changed": false, "ping": "pong"}
```

---

## 7. First Playbook Example (Install Nginx)
Create a file **`install_nginx.yml`**:

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

Run it:

```bash
ansible-playbook -i inventory.ini install_nginx.yml
```

Verify in browser:

- http://34.235.115.233  
- http://50.17.140.17  

✅ You should see the default **Nginx Welcome Page** 🎉
