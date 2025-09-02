
# 🔹 Using `register` in Ansible

## 1. What is `register`?
- In Ansible, `register` is used to **capture the output** of a task and store it in a variable.
- This variable can be reused in later tasks.

---

## 2. Basic Example

```yaml
- hosts: all
  tasks:
    - name: Check uptime
      command: uptime
      register: uptime_result

    - name: Show uptime output
      debug:
        var: uptime_result
```

### Output
```yaml
"uptime_result": {
    "changed": true,
    "cmd": ["uptime"],
    "rc": 0,
    "stderr": "",
    "stdout": " 10:32:01 up 5 days,  2:10,  2 users,  load average: 0.00, 0.01, 0.05",
    "stdout_lines": [
        " 10:32:01 up 5 days,  2:10,  2 users,  load average: 0.00, 0.01, 0.05"
    ]
}
```

---

## 3. Accessing Registered Variables

- Use `{{ variable.stdout }}` to fetch command output.
- Example:

```yaml
- hosts: all
  tasks:
    - name: Check hostname
      command: hostname
      register: host_output

    - name: Display hostname only
      debug:
        msg: "The hostname is {{ host_output.stdout }}"
```

---

## 4. Using `when` with `register`

You can use `register` along with **conditionals**.

```yaml
- hosts: all
  tasks:
    - name: Check OS type
      command: uname -s
      register: os_type

    - name: Install Nginx on Linux
      apt:
        name: nginx
        state: present
      when: os_type.stdout == "Linux"
```

---

## 5. Looping with Registered Variables

You can loop through output stored in a registered variable.

```yaml
- hosts: all
  tasks:
    - name: List files
      command: ls /etc
      register: file_list

    - name: Show files
      debug:
        var: file_list.stdout_lines
```

---

## 6. Key Fields in Registered Output
- `stdout` → Standard output (string)  
- `stdout_lines` → Output as a list of lines  
- `stderr` → Standard error  
- `rc` → Return code (0 = success)  
- `changed` → Whether Ansible thinks the task changed state  

---

✅ `register` is one of the most powerful features in Ansible for **capturing, reusing, and controlling logic** in playbooks.
