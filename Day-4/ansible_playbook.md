# 📘 Ansible Playbook

## What is a Playbook?

An **Ansible Playbook** is a YAML file that defines a set of automation
tasks to be executed on managed nodes.\
It is the core feature of Ansible that allows you to describe
configurations, deployments, and orchestrations in a human-readable
format.

Playbooks are written in **YAML (Yet Another Markup Language)** and
consist of one or more **plays**.\
Each play maps a group of hosts to tasks, which are executed using
Ansible modules.

------------------------------------------------------------------------

## Key Features of Playbooks

-   **Declarative**: Define the desired state of the system, not just
    step-by-step instructions.
-   **Idempotent**: Running the playbook multiple times won't cause
    unintended changes.
-   **Reusable**: Playbooks can be shared, versioned, and reused across
    environments.
-   **Extensible**: Can include roles, variables, handlers, and
    conditionals.

------------------------------------------------------------------------

## Structure of a Playbook

A playbook typically includes: 1. **Hosts** → Define target machines or
groups. 2. **Become** → Run tasks with elevated privileges (like
`sudo`). 3. **Tasks** → List of actions to perform using Ansible
modules. 4. **Handlers** → Special tasks triggered by `notify` when
something changes. 5. **Variables** → Values that can be reused within
tasks.

### Example:

``` yaml
---
- name: Configure web servers
  hosts: web
  become: yes
  tasks:
    - name: Install Apache
      apt:
        name: apache2
        state: present

    - name: Start Apache service
      service:
        name: apache2
        state: started
        enabled: yes
```

------------------------------------------------------------------------

## Tasks in Playbooks

-   Each **task** runs a single module with specific arguments.

-   Tasks are executed in order, from top to bottom.

-   Example:

    ``` yaml
    - name: Install Nginx
      apt:
        name: nginx
        state: present
    ```

------------------------------------------------------------------------

## Variables in Playbooks

Variables allow flexibility and reusability.

Example:

``` yaml
---
- hosts: web
  vars:
    http_port: 80
  tasks:
    - name: Install Nginx
      apt:
        name: nginx
        state: present

    - name: Configure firewall
      ufw:
        rule: allow
        port: "{{ http_port }}"
        proto: tcp
```

------------------------------------------------------------------------

## Handlers in Playbooks

Handlers are tasks that run only when notified.\
Useful for restarting services after configuration changes.

Example:

``` yaml
---
- hosts: web
  tasks:
    - name: Update configuration file
      template:
        src: apache.conf.j2
        dest: /etc/apache2/sites-available/000-default.conf
      notify: Restart Apache

  handlers:
    - name: Restart Apache
      service:
        name: apache2
        state: restarted
```

------------------------------------------------------------------------

## Roles in Playbooks

Roles help organize playbooks into reusable components.\
They follow a structured directory layout (`tasks/`, `handlers/`,
`vars/`, `templates/`, `files/`).

Example usage:

``` yaml
---
- hosts: web
  roles:
    - nginx
    - mysql
```

------------------------------------------------------------------------

## Running a Playbook

To run a playbook, use the command:

``` bash
ansible-playbook -i inventory.ini playbook.yml
```

------------------------------------------------------------------------

## Conclusion

Playbooks are the **heart of Ansible automation**.\
They enable consistent, repeatable, and scalable management of IT
infrastructure by describing desired states in a simple YAML format.
