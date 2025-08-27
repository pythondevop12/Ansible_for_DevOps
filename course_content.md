# Ansible Learning Roadmap (Day-wise)

This roadmap is designed for **15 days** of focused learning. Each day has specific topics, hands-on labs, and resources to build a strong foundation in **Ansible**.

---

## Day 1: Introduction to Ansible

* What is Ansible?
* Why use Ansible? (Advantages over other tools)
* Ansible architecture & components
* Control Node vs Managed Nodes
* Installation of Ansible on Linux/Mac/Windows (WSL)
* Verify installation with `ansible --version`

---

## Day 2: Getting Started with Inventory & Ad-Hoc Commands

* Introduction to Inventory (`/etc/ansible/hosts`)
* Static vs Dynamic inventory
* Run your first ad-hoc commands:

  * `ansible all -m ping`
  * `ansible all -a "uptime"`
* Using groups and host variables

---

## Day 3: YAML Basics

* What is YAML?
* Syntax & indentation rules
* Key-Value pairs, lists, dictionaries
* Hands-on: Write sample YAML files

---

## Day 4: Playbooks Introduction

* What is a playbook?
* Structure of a playbook (hosts, tasks, modules)
* Writing your first playbook
* Running playbooks with `ansible-playbook`

---

## Day 5: Ansible Modules Basics

* Commonly used modules:

  * `ping`, `command`, `shell`
  * `copy`, `file`, `service`
  * `yum`/`apt` (package management)
* Hands-on: Create a playbook to install & start a service

---

## Day 6: Variables in Ansible

* Defining variables in playbooks
* Host and group variables (`group_vars/`, `host_vars/`)
* Facts and gathered variables
* Hands-on: Use variables in playbooks

---

## Day 7: Conditionals and Loops

* Using `when` for conditional tasks
* Using loops with `with_items`, `loop`
* Hands-on: Install multiple packages using loops

---

## Day 8: Handlers and Notifications

* What are handlers?
* Notify and trigger handlers
* Hands-on: Restart a service after configuration changes

---

## Day 9: Templates (Jinja2)

* Introduction to Jinja2 templating
* Using variables in templates
* Create `.j2` templates for configuration files
* Hands-on: Deploy an Nginx/Apache config template

---

## Day 10: Roles in Ansible

* Why roles?
* Role directory structure
* Creating and using roles
* Hands-on: Convert a playbook into a role

---

## Day 11: Ansible Galaxy

* Introduction to Ansible Galaxy
* Searching & installing community roles
* Hands-on: Use a Galaxy role in your project

---

## Day 12: Error Handling and Debugging

* Debug module
* Ignore errors
* Block and rescue
* Hands-on: Debug failing tasks

---

## Day 13: Advanced Topics

* Ansible Vault (secrets management)
* Tags and limiting execution
* Delegation and Local actions

---

## Day 14: Ansible in CI/CD

* Integrating Ansible with Jenkins/GitHub Actions
* Running playbooks in pipelines
* Hands-on: Automate deployment with CI/CD

---

## Day 15: Project & Best Practices

* Build a complete Ansible project:

  * Setup a web server with roles, templates, handlers
* Ansible best practices (folder structure, idempotency, reusability)
* Next steps: Learn AWX/Ansible Tower for enterprise automation

---

## Resources

* [Official Documentation](https://docs.ansible.com/)
* [Ansible Galaxy](https://galaxy.ansible.com/)
* [YAML Basics](https://yaml.org/)
* Books: *Ansible for DevOps* by Jeff Geerling

---

✅ By following this roadmap, you will be ready to **use Ansible in real-world DevOps projects** within 15 days.
