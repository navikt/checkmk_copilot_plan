# Prerequisites

## Control Node (where Ansible runs)
- Ansible >= 2.14
- Python >= 3.9
- Collections required:
  ```bash
  ansible-galaxy collection install ansible.posix community.windows ansible.windows
  ```

## CheckMK Server Host
- RHEL / CentOS Stream / Rocky Linux 8 or 9 (x86_64)
- Minimum 2 vCPU, 4 GB RAM, 20 GB disk
- SSH access with sudo for the Ansible user
- Internet access to `download.checkmk.com` (or a local mirror set via `checkmk_rpm_url`)

## Linux Monitored Hosts
- SSH access with sudo for the Ansible user
- Port 6556/tcp reachable from the CheckMK server

## Windows Monitored Hosts
WinRM must be enabled and configured before running the Windows agent playbook.

Run the following on each Windows host (PowerShell as Administrator):
```powershell
# Enable WinRM with basic/NTLM auth
Enable-PSRemoting -Force
winrm set winrm/config/service/auth '@{Basic="true"}'
winrm set winrm/config/service '@{AllowUnencrypted="true"}'
# Or for HTTPS (recommended for production):
# New-SelfSignedCertificate -DnsName <hostname> -CertStoreLocation cert:\LocalMachine\My
```

Port **5985** (HTTP) or **5986** (HTTPS) must be reachable from the Ansible control node.

## Network Devices (SNMP)
- SNMP v2c or v3 must be enabled on each device
- The CheckMK server must be able to reach the device on UDP port 161
- Note the SNMP community string (v2c) or credentials (v3) — set them in `group_vars/all.yml` with Ansible Vault

## Ansible Vault Setup
All secrets are stored in Ansible Vault. To encrypt a value:
```bash
ansible-vault encrypt_string 'mysecretvalue' --name 'vault_checkmk_admin_password'
```
Paste the output into `inventory/group_vars/all.yml`.

To run playbooks with vault:
```bash
ansible-playbook -i inventory/hosts.yml playbooks/site.yml --ask-vault-pass
# or use a vault password file:
ansible-playbook -i inventory/hosts.yml playbooks/site.yml --vault-password-file ~/.vault_password
```
