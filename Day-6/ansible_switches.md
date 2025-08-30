# 🔌 Ansible for Network Switches

## Introduction

Ansible is not only used for servers and cloud automation but also for
managing **network devices** such as routers, firewalls, and
**switches**.\
It allows network engineers to automate repetitive tasks like VLAN
configuration, interface management, and firmware upgrades.

------------------------------------------------------------------------

## Why Use Ansible for Switches?

-   **Agentless** → Uses SSH or API, no agent needed on devices.\
-   **Multi-vendor support** → Cisco, Juniper, Arista, Dell, HP, etc.\
-   **Consistency** → Same playbooks can manage different switch
    vendors.\
-   **Scalability** → Manage hundreds of switches at once.\
-   **Idempotent** → Avoids duplicate configuration changes.

------------------------------------------------------------------------

## Supported Vendors & Modules

Some commonly supported switch vendors and their modules:

-   **Cisco IOS** → `ios_config`, `ios_facts`\
-   **Arista EOS** → `eos_config`, `eos_facts`\
-   **Juniper JunOS** → `junos_config`, `junos_facts`\
-   **Dell OS10** → `dellos10_config`\
-   **HP Comware** → `comware_config`

Each module allows configuring interfaces, VLANs, ACLs, SNMP, and more.

------------------------------------------------------------------------

## Inventory for Switches

Switches are added to the Ansible inventory just like servers.

Example `inventory.ini`:

``` ini
[switches]
cisco_sw1 ansible_host=192.168.1.10 ansible_user=admin ansible_password=MyPassword ansible_network_os=ios
arista_sw1 ansible_host=192.168.1.11 ansible_user=admin ansible_password=MyPassword ansible_network_os=eos
juniper_sw1 ansible_host=192.168.1.12 ansible_user=admin ansible_password=MyPassword ansible_network_os=junos
```

------------------------------------------------------------------------

## Example Playbook: Configure VLAN on Cisco Switch

``` yaml
---
- name: Configure VLAN on Cisco Switch
  hosts: cisco_sw1
  gather_facts: no
  connection: network_cli

  tasks:
    - name: Create VLAN 100
      ios_config:
        lines:
          - name VLAN100
          - vlan 100
          - name DEV_NET
```

------------------------------------------------------------------------

## Example Playbook: Configure Interface on Arista Switch

``` yaml
---
- name: Configure Interface on Arista Switch
  hosts: arista_sw1
  gather_facts: no
  connection: network_cli

  tasks:
    - name: Configure Ethernet1 with IP
      eos_config:
        lines:
          - interface Ethernet1
          - ip address 192.168.50.1/24
          - no shutdown
```

------------------------------------------------------------------------

## Advantages of Using Ansible for Switches

-   Centralized configuration management\
-   Reduce manual errors in network changes\
-   Automate firmware updates across switches\
-   Easier compliance and auditing\
-   Faster deployment of new configurations

------------------------------------------------------------------------

## Conclusion

Ansible simplifies **network automation** by providing a consistent,
agentless, and vendor-agnostic framework for managing switches.\
With playbooks and inventory, you can automate VLANs, interfaces, ACLs,
and more across hundreds of devices.
