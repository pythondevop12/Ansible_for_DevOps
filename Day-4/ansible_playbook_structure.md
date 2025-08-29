# 📘 Structure of an Ansible Playbook

## Introduction

An **Ansible Playbook** is a YAML file that defines automation steps to
be executed on managed nodes.\
Understanding its structure is crucial for writing clear and efficient
automation scripts.

------------------------------------------------------------------------

## Core Components of a Playbook

### 1. Hosts

-   Defines **where** the playbook should run.\

-   Can target individual hosts, groups, or all hosts defined in the
    inventory.\

-   Example:

    ``` yaml
    - hosts: webservers
    ```

------------------------------------------------------------------------

### 2. Tasks

-   A list of actions to be executed on the target hosts.\

-   Each task uses an **Ansible module**.\

-   Tasks are executed sequentially, from top to bottom.\

-   Example:

    ``` yaml
    tasks:
      - name: Install Nginx
        apt:
          name: nginx
          state: present

      - name: Ensure Nginx is running
        service:
          name: nginx
          state: started
          enabled: yes
    ```

------------------------------------------------------------------------

### 3. Modules

-   Modules are reusable units of code that perform a specific function
    (like installing packages, managing files, or configuring
    services).\

-   Common modules include:

    -   **apt/yum** → Install or remove packages\
    -   **service** → Manage services\
    -   **copy/template** → Manage files\
    -   **user** → Manage users\
    -   **file** → Manage file permissions, directories\

-   Example:

    ``` yaml
    - name: Create a directory
      file:
        path: /var/www/html
        state: directory
        mode: '0755'
    ```

------------------------------------------------------------------------

## Example: Complete Playbook Structure

``` yaml
---
- name: Example Playbook Structure
  hosts: webservers
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

    - name: Deploy index.html file
      copy:
        src: index.html
        dest: /var/www/html/index.html
        mode: '0644'
```

------------------------------------------------------------------------

## Summary

-   **Hosts**: Define target machines.\
-   **Tasks**: List of actions to perform.\
-   **Modules**: Building blocks of tasks, performing specific
    functions.

This structure makes playbooks **simple, powerful, and reusable** for
automating IT operations.
