# Ansible Practice Setup

This repository contains Ansible playbooks and roles for provisioning, configuring, and managing servers in a structured and reusable way.


## 📁 Project Structure

```
ansible-practice/
├── README.md
├── ansible.cfg               # Ansible configuration file
├── inventory                 # Managed hosts inventory
├── inventory_old             # Backup/old inventory
├── site.yml                  # Main playbook to run complete automation
├── site_before_roles.yml     # Alternate playbook for execution before roles
├── bootstrap.yml             # Bootstrap playbook for initial setup
├── install_apache.yml        # Playbook to install Apache
├── remove_apache.yml         # Playbook to remove Apache
├── update_install_apache.yml # Apache update + install tasks
├── service.yml               # Service management playbook
├── files/                    # Static files to push to remote nodes
├── host_vars/                # Host-level variable files
├── roles/                    # Ansible roles folder
│   ├── base                  # Base configuration role
│   ├── db_servers            # DB server tasks
│   ├── file_servers          # File server tasks
│   ├── web_servers           # Web server tasks
│   └── workstations          # Workstation tasks
```

## 🚀 Getting Started


### 0. Generate SSH Key & Share to Remote Nodes

Generate SSH key on control machine (Ansible master):

```bash
ssh-keygen -t rsa -b 4096
```

Default location: `~/.ssh/id_rsa`

Copy public key to remote servers:

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub ubuntu@<server-ip>
```

OR manually copy key:

```bash
cat ~/.ssh/id_rsa.pub
# Paste content into remote server at:
~/.ssh/authorized_keys
```

Then test SSH connection:

```bash
ssh ubuntu@<server-ip>
```

### 1. Test SSH Connection to Servers

Before running playbooks, ensure SSH access is working:

```bash
ssh -i <private-key> ubuntu@<server-ip>
```

If using a default SSH key location (`~/.ssh/id_rsa`):

```bash
ssh ubuntu@<server-ip>
```

Test via Ansible ping:

```bash
ansible all -m ping
```

### 1. Install Ansible

```bash
sudo apt update
sudo apt install ansible -y
```

### 2. Configure Ansible

Edit **ansible.cfg** if required:

```ini
[defaults]
inventory = inventory
host_key_checking = False
```

### 3. Define Hosts

Edit `inventory` file:

```ini
[web_servers]
server-ip-1
server-ip-2

[db_servers]
db-ip
```

Optional per-host configuration inside `host_vars/<IP>.yml`.

### 4. Run Playbooks

Run main deployment:

```bash
ansible-playbook site.yml
```

Install Apache:

```bash
ansible-playbook install_apache.yml
```

Remove Apache:

```bash
ansible-playbook remove_apache.yml
```

Bootstrap servers:

```bash
ansible-playbook bootstrap.yml
```

## 🧱 Roles Overview

| Role         | Description                                                  |
| ------------ | ------------------------------------------------------------ |
| base         | Basic configuration tasks like sshd config, user setup, etc. |
| web_servers  | Deploy web content & configure Apache/Nginx.                 |
| db_servers   | DB installation/configuration tasks.                         |
| file_servers | File server setup automation.                                |
| workstations | Dev/Workstation environment setup.                           |

## Contribution

Feel free to fork and contribute by improving roles or adding new automation.

I am triggering jenkins build automatically.
---

Made with 🛠️ using **Ansible**

