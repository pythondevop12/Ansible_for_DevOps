# 🔄 Loops in Ansible

Loops in Ansible allow you to **repeat a task multiple times** with different values, making playbooks more efficient and reusable. Instead of writing the same task multiple times, you can use loops to iterate through items.

---

## 1. Basic Loop with `loop`
The `loop` keyword is the most commonly used method for looping in Ansible.

### Example:
```yaml
- name: Install multiple packages
  apt:
    name: "{{ item }}"
    state: present
  loop:
    - git
    - curl
    - htop
```

### Explanation:
- `loop:` defines the list of items.
- `item` is the default variable representing the current value in the loop.
- In this case, Ansible will install `git`, `curl`, and `htop`.

---

## 2. Loop with Dictionaries
You can loop through key-value pairs (dictionaries).

### Example:
```yaml
- name: Create users with specific shells
  user:
    name: "{{ item.name }}"
    shell: "{{ item.shell }}"
  loop:
    - { name: 'alice', shell: '/bin/bash' }
    - { name: 'bob', shell: '/bin/zsh' }
```

### Explanation:
- Each `item` is a dictionary.
- Access dictionary values using dot notation (`item.name`, `item.shell`).

---

## 3. Using `with_items`
Older Ansible versions used `with_items` instead of `loop`.

### Example:
```yaml
- name: Install multiple packages (legacy way)
  yum:
    name: "{{ item }}"
    state: present
  with_items:
    - httpd
    - nginx
```

⚠️ **Note:** `with_items` is still supported but **deprecated**. Use `loop` instead.

---

## 4. Nested Loops with `with_nested`
You can loop through combinations of lists.

### Example:
```yaml
- name: Combine users with groups
  debug:
    msg: "User {{ item[0] }} will be added to group {{ item[1] }}"
  with_nested:
    - [ 'alice', 'bob' ]
    - [ 'admin', 'developers' ]
```

### Output:
```
User alice will be added to group admin
User alice will be added to group developers
User bob will be added to group admin
User bob will be added to group developers
```

---

## 5. Loop with `with_dict`
Used to loop through dictionary key-value pairs.

### Example:
```yaml
- name: Print dictionary values
  debug:
    msg: "Key={{ item.key }} Value={{ item.value }}"
  with_dict:
    server1: 192.168.1.10
    server2: 192.168.1.20
```

---

## 6. Loop with `subelements`
Used when looping through nested lists inside dictionaries.

### Example:
```yaml
- name: Show users and their groups
  debug:
    msg: "User {{ item.0.name }} is part of group {{ item.1 }}"
  with_subelements:
    -
      - { name: 'alice', groups: ['admin', 'dev'] }
      - { name: 'bob', groups: ['dev'] }
    - groups
```

---

## 7. Loop with `until` (Retries with Loops)
You can retry a task until a condition is met.

### Example:
```yaml
- name: Wait for service to be available
  uri:
    url: http://localhost:8080
    status_code: 200
  register: result
  until: result.status == 200
  retries: 5
  delay: 10
```

### Explanation:
- `until` keeps retrying until the condition is satisfied.
- `retries` defines how many times to try.
- `delay` defines the interval between retries.

---

## 8. Loop with `loop_control`
You can customize loop variables and labels.

### Example:
```yaml
- name: Install packages with custom loop_var
  apt:
    name: "{{ pkg }}"
    state: present
  loop:
    - git
    - curl
    - htop
  loop_control:
    loop_var: pkg
```

### Explanation:
- Instead of the default `item`, we use `pkg`.

---

## ✅ Best Practices
1. Use `loop` instead of `with_items` (modern approach).
2. Use `loop_control` for readability in complex loops.
3. Avoid deeply nested loops if possible (use roles or templates instead).
4. Always add comments for clarity when using advanced loops.

---

# 📌 Summary
- **Basic Loops** → `loop`, `with_items`
- **Nested/Complex Loops** → `with_nested`, `with_dict`, `with_subelements`
- **Retries** → `until`
- **Customization** → `loop_control`

Loops in Ansible make playbooks **clean, efficient, and scalable** 🚀
