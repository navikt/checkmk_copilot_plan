# checkmk_copilot_plan

Ansible-based automation to install **CheckMK Raw Edition** (community) on a RHEL/Rocky Linux server and configure monitoring for Linux servers, Windows servers, and network devices (SNMP).

## Quick Start

### Prerequisites
See [docs/prerequisites.md](docs/prerequisites.md).

### 1. Configure inventory
Copy and edit the inventory:
```bash
cp inventory/hosts.yml.example inventory/hosts.yml
# Edit inventory/hosts.yml with your hosts
```

Edit `inventory/group_vars/all.yml` and fill in your values (use Ansible Vault for secrets).

### 2. Run the full setup
```bash
ansible-playbook -i inventory/hosts.yml playbooks/site.yml --ask-vault-pass
```

### 3. Run individual phases
```bash
# Install CheckMK server only
ansible-playbook -i inventory/hosts.yml playbooks/01_install_checkmk_server.yml --ask-vault-pass

# Deploy Linux agents
ansible-playbook -i inventory/hosts.yml playbooks/02_deploy_linux_agents.yml --ask-vault-pass

# Deploy Windows agents
ansible-playbook -i inventory/hosts.yml playbooks/03_deploy_windows_agents.yml --ask-vault-pass

# Add SNMP / network devices
ansible-playbook -i inventory/hosts.yml playbooks/04_configure_snmp_devices.yml --ask-vault-pass

# Configure notifications (email + Slack/Teams)
ansible-playbook -i inventory/hosts.yml playbooks/05_configure_notifications.yml --ask-vault-pass
```

## Documentation
- [Prerequisites](docs/prerequisites.md)
- [Inventory Guide](docs/inventory_guide.md)
- [Post-Install Guide](docs/post_install.md)

## Structure
```
checkmk_copilot_plan/
├── inventory/
│   ├── hosts.yml
│   └── group_vars/
├── playbooks/
├── roles/
│   ├── checkmk_server/
│   ├── checkmk_agent_linux/
│   ├── checkmk_agent_windows/
│   └── checkmk_notifications/
└── docs/
```
