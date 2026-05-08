# Post-Install Guide

## Accessing the CheckMK UI
After the server playbook completes, open a browser and navigate to:
```
http://<checkmk_server_hostname>/monitoring
```
Log in with:
- **Username**: `cmkadmin`
- **Password**: value of `vault_checkmk_admin_password`

## Verifying monitored hosts
1. In the CheckMK UI, go to **Monitor → All hosts**
2. All hosts defined in `inventory/hosts.yml` should appear
3. Services should be discovered and in a green state within a few minutes

## Verifying notifications
1. Go to **Setup → Notifications**
2. You should see rules for Email, Slack, and/or MS Teams
3. Use **Test notifications** (available per rule) to send a test alert

## Running a full re-check
To re-run discovery on all hosts after adding services:
```bash
ansible-playbook -i inventory/hosts.yml playbooks/site.yml \
  --tags agents,snmp --ask-vault-pass
```

## Adding more hosts later
See the [Inventory Guide](inventory_guide.md) for step-by-step instructions for each host type.

## Updating CheckMK version
1. Change `checkmk_version` in `inventory/group_vars/all.yml`
2. Re-run the server playbook:
   ```bash
   ansible-playbook -i inventory/hosts.yml playbooks/01_install_checkmk_server.yml --ask-vault-pass
   ```
3. Re-deploy agents to all hosts to match the server version:
   ```bash
   ansible-playbook -i inventory/hosts.yml playbooks/site.yml --tags agents --ask-vault-pass
   ```

## Useful OMD commands (run on CheckMK server)
```bash
omd status monitoring     # check site status
omd stop monitoring       # stop the site
omd start monitoring      # start the site
omd restart monitoring    # restart the site
omd backup monitoring     # create a site backup
```
