# Inventory Guide

## File: `inventory/hosts.yml`

The inventory is split into four groups:

| Group | Purpose |
|---|---|
| `checkmk_server` | The host where CheckMK Raw Edition is installed (exactly one host) |
| `linux_hosts` | Linux servers to be monitored with the CheckMK agent |
| `windows_hosts` | Windows servers to be monitored with the CheckMK agent |
| `network_devices` | Network devices (switches, routers) monitored via SNMP |

## Adding a new Linux host
1. Add the hostname under `linux_hosts` in `inventory/hosts.yml`
2. Run: `ansible-playbook -i inventory/hosts.yml playbooks/02_deploy_linux_agents.yml --ask-vault-pass`

## Adding a new Windows host
1. Add the hostname under `windows_hosts` in `inventory/hosts.yml`
2. Ensure WinRM is enabled (see [prerequisites.md](prerequisites.md))
3. Run: `ansible-playbook -i inventory/hosts.yml playbooks/03_deploy_windows_agents.yml --ask-vault-pass`

## Adding a new network device
1. Add the hostname/IP under `network_devices` in `inventory/hosts.yml`
2. Set `checkmk_snmp_version` per host if different from default (`2c`)
3. Run: `ansible-playbook -i inventory/hosts.yml playbooks/04_configure_snmp_devices.yml --ask-vault-pass`

## Key variables (`inventory/group_vars/all.yml`)

| Variable | Description |
|---|---|
| `checkmk_version` | CheckMK Raw version to install (e.g. `2.3.0p1`) |
| `checkmk_site` | Name of the OMD monitoring site |
| `checkmk_admin_password` | Admin UI password (vault-encrypted) |
| `checkmk_api_user` | Automation user name |
| `checkmk_api_secret` | Automation user secret (vault-encrypted) |
| `checkmk_snmp_community` | Default SNMP community string (vault-encrypted) |
| `checkmk_smtp_host` | SMTP relay hostname |
| `checkmk_slack_webhook_url` | Slack incoming webhook URL (vault-encrypted) |
| `checkmk_teams_webhook_url` | MS Teams webhook URL (vault-encrypted) |

Per-host SNMP version override (in `inventory/hosts.yml`):
```yaml
network_devices:
  hosts:
    myswitch.example.com:
      checkmk_snmp_version: "3"
```
