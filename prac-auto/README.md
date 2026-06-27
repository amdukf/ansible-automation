# Role Name

prac-auto

---

# Description

This role provides a general-purpose automation baseline for Linux systems commonly used by DevOps engineers.

It is designed to standardize and simplify repetitive infrastructure tasks across different environments and distributions.

The role focuses on:
- System user provisioning for automation and administration
- SSH access hardening and configuration
- Secure key-based authentication setup
- Sudo privilege management
- System security baseline configuration
- Optional OS-specific networking or system configuration tasks
- File deployment and system preparation utilities

This role is intended to be modular and extensible, so additional operating systems and use cases can be added over time.

---

# Requirements

- Ansible 2.15+
- SSH access with privilege escalation (sudo/root)
- Linux-based target systems (Debian, Ubuntu, and others supported progressively)

No external dependencies are required.

---

# Role Variables

All default variables for this role are defined in the `defaults/` directory and can be overridden using Ansible variable precedence.

Global variables for all hosts can also be defined in `group_vars/all/`, which will apply to every managed node unless explicitly overridden.

---

# Dependencies

This role does not depend on external Galaxy roles.

---

# Design Philosophy

This role follows these principles:

* **Generic by default** → works across multiple Linux distributions
* **Modular structure** → tasks are separated and reusable
* **Idempotent execution** → safe to run multiple times
* **Security-first approach** → SSH hardening and least-privilege design
* **Extensible architecture** → easy to add new system domains (networking, logging, etc.)

---

# Notes

* Some tasks may behave differently depending on the Linux distribution.
* SSH configuration changes (like port modification) should be tested carefully to avoid losing connectivity.
* Networking-related tasks are optional and may be extended for different systems (Netplan, NetworkManager, etc.).
* If you want to use *Apt repository settings and credentials* task in apt, you should pass `-e configure_apt_repo=true` to the command.
---

# License

Apache-2.0

---

# Author Information
```
Amir (Tyrell) Kheirandish
DevOps Engineer
Infrastructure automation • Kubernetes • CI/CD • System hardening
```
