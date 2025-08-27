# Ansible Inventory

## Introduction to Inventory (/etc/ansible/hosts)

The Ansible inventory is a file that contains information about the
hosts and groups of hosts that Ansible manages.\
By default, the inventory file is located at `/etc/ansible/hosts`.\
This file allows you to specify which machines Ansible should connect
to, and it can be static or dynamic.

------------------------------------------------------------------------

## Static vs Dynamic Inventory

### Static Inventory

-   Defined in a simple text file (INI or YAML format).\

-   You manually list all the hosts and groups.\

-   Example (`/etc/ansible/hosts`):

    ``` ini
    [web]
    web1.example.com
    web2.example.com

    [db]
    db1.example.com
    ```

### Dynamic Inventory

-   Hosts are fetched from external sources like cloud providers (AWS,
    GCP, Azure) or custom scripts.\
-   Useful when infrastructure is dynamic and frequently changing.\
-   Example: Using AWS EC2 plugin to dynamically fetch inventory.

------------------------------------------------------------------------

## Run Your First Ad-hoc Commands

Ad-hoc commands are simple one-liners that allow you to run tasks
quickly without writing a playbook.

-   **Ping all hosts**:

    ``` bash
    ansible all -m ping
    ```

-   **Check uptime on all hosts**:

    ``` bash
    ansible all -a "uptime"
    ```

------------------------------------------------------------------------

## Using Groups and Host Variables

You can organize your hosts into groups and assign variables.

Example inventory file:

``` ini
[web]
web1 ansible_host=192.168.1.10 ansible_user=ubuntu
web2 ansible_host=192.168.1.11 ansible_user=ubuntu

[db]
db1 ansible_host=192.168.1.20 ansible_user=ec2-user

[all:vars]
ansible_ssh_private_key_file=~/.ssh/id_rsa
```

-   **Groups**: `[web]`, `[db]` define logical sets of hosts.\
-   **Host variables**: Specific to individual hosts (`ansible_host`,
    `ansible_user`).\
-   **Group variables**: Common variables shared across groups
    (`all:vars`).

------------------------------------------------------------------------

## Conclusion

Ansible inventory is the backbone of automation as it defines the
infrastructure.\
You can start with a static inventory and gradually move to dynamic
inventory for larger and cloud-based environments.
