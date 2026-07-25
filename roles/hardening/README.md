# Hardening

## Description

This role provides a general-purpose automation baseline for Linux systems commonly used by DevOps engineers.

It is designed to standardize and simplify repetitive infrastructure tasks across different environments and distributions.

The role focuses on:

- System user provisioning for automation and administration
- SSH access hardening and configuration
- Secure key-based authentication setup
- Sudo privilege management
- System security baseline configuration
- Firewall configuration and hardening (iptables or UFW)
- Optional OS-specific networking or system configuration tasks
- File deployment and system preparation utilities

This role is intended to be modular and extensible, so additional operating systems and use cases can be added over time.

---

## Requirements

- Ansible 2.15+
- SSH access with privilege escalation (sudo/root)
- Linux-based target systems (Debian, Ubuntu, and others supported progressively)

No external dependencies are required.

---

## Role Variables

All default variables for this role are defined in the `defaults/` directory and can be overridden using Ansible variable precedence.

Global variables for all hosts can also be defined in `group_vars/all/`, which will apply to every managed node unless explicitly overridden.

---

## Dependencies

This role does not depend on external Galaxy roles.

---

## Design Philosophy

This role follows these principles:

* **Generic by default** → works across multiple Linux distributions
* **Modular structure** → tasks are separated and reusable
* **Idempotent execution** → safe to run multiple times
* **Security-first approach** → SSH hardening, firewall configuration, and least-privilege design
* **Extensible architecture** → easy to add new system domains (networking, logging, etc.)

---

## Firewall Package Conflict

When configuring firewall hardening, only one firewall management approach should be enabled on a system.

## UFW

If you want to use `ufw`, make sure the following packages are not installed:

- `iptables-persistent`
- `netfilter-persistent`

These packages conflict with UFW and will be removed if UFW configuration is enabled.

Example:

```bash
ansible-playbook -i inventory/inventory.yml playbooks/hardening.yml --tags ufw_hardening -e configure_ufw=true
````

## Iptables Persistent

If you want to use `iptables-persistent`, installing it will remove `ufw`.

Example:

```bash
ansible-playbook -i inventory/inventory.yml playbooks/hardening.yml --tags iptables_hardening -e configure_iptables=true
```

Only one firewall management solution should be used on each managed system.

---

## Usage Examples

### Apply Hardening Role

```bash
ansible-playbook -i inventory/inventory.yml playbooks/hardening.yml --tags debian-hardening
```

### Configure Apt Repository

To configure Apt using your own repository settings and credentials:

```bash
ansible-playbook -i inventory/inventory.yml playbooks/pracauto.yml --tags install_apt_packages -e configure_apt_repo=true
```

### Apply Firewall Configuration (Iptables)

```bash
ansible-playbook -i inventory/inventory.yml playbooks/hardening.yml --tags iptables_hardening -e configure_iptables=true
```

### Apply Firewall Configuration (UFW)

```bash
ansible-playbook -i inventory/inventory.yml playbooks/hardening.yml --tags ufw_hardening -e configure_ufw=true
```

---

I added a section covering password-based Ansible connections, Vault usage, and the default vault password note. I would place it under **Requirements** or **Usage Examples**. Here is the updated section:


## Ansible Connection Authentication

By default, this role assumes SSH key-based authentication. 

If you are connecting to managed servers using a password instead of SSH keys, update the connection method:

```yaml
ansible_connection_method: password
````

The server password should not be stored as plain text. Store it securely using **Ansible Vault**.

The default Ansible Vault file is already included with this role. The default vault password is:

```
changeme@123
```

For production environments, you should change the vault password and update the encrypted variables with your own credentials.

Example:

```bash
ansible-vault edit group_vars/all/vault.yml
```

Run the playbook using the vault password:

```bash
ansible-playbook -i inventory/inventory.yml playbooks/hardening.yml --ask-vault-pass
```

For better security, use a vault password file instead of providing the password interactively:

```bash
ansible-playbook -i inventory/inventory.yml playbooks/hardening.yml --vault-password-file .vault_password
```

Never commit unencrypted passwords or vault password files to source control.

---

## License

Apache-2.0

---

## Author Information

```
Amir (Tyrell) Kheirandish
DevOps Engineer
Infrastructure automation • Kubernetes • CI/CD • System hardening
```
A