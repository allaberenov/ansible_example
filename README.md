# Infrastructure Automation with Ansible

This repository contains an Ansible-based automation setup designed to configure and manage a Linux environment.  
It includes a reusable Ansible role, supporting both **Ubuntu 22.04** and **CentOS 7**, and a top-level playbook that applies the role to the target host.

The automation performs system preparation, package installation, service configuration, templating, cron scheduling, and runtime verification.

---

## 📁 Repository Structure

```

.
├── playbook.yml             # Main Ansible playbook
├── hosts.ini                # Inventory for running with Docker or local VMs
└── service/                 # Ansible role
├── tasks/
│   ├── install.yml      # Package installation and service handling
│   ├── config.yml       # Templating and configuration files
│   ├── cron.yml         # Cron job installation
│   └── verify.yml       # Runtime verification logic
├── handlers/
│   └── main.yml         # Service restart handlers for Ubuntu & CentOS
├── templates/
│   ├── service_state.json.j2   # JSON template
│   └── nginx.conf.j2           # Nginx virtual host configuration
└── defaults/
└── main.yml         # Default variables (if needed)

````

---

## 🧩 Requirements

To run the playbook locally:

- Python 3.8+
- Ansible 2.16+
- Docker (optional, for testing)
- SSH or Docker access to target hosts

For CentOS 7 testing inside Docker, Ansible uses the Docker connection plugin.

### Run against the included Docker hosts (from `hosts.ini`)

```bash
ansible-playbook -i hosts.ini playbook.yml
```

This will:

* Connect to the container(s) via Docker
* Install and configure required packages
* Deploy templates
* Configure services
* Install cron tasks
* Verify environment state

---

### Run against a remote or local VM

Edit `hosts.ini`:

```ini
[myserver]
192.168.1.10 ansible_user=ubuntu ansible_become=true
```

Then run:

```bash
ansible-playbook -i hosts.ini playbook.yml
```


## 🛠 Notes

* The role is idempotent: repeated runs do not break state or overwrite runtime data.
* Configuration changes in templates automatically trigger a controlled service reload.
* Compatible with both systemd-enabled hosts (Ubuntu) and non-systemd environments (CentOS 7 inside Docker).

---
