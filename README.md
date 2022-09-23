# Red Hat Satellite Automation

[![License: GPLv3](https://img.shields.io/badge/license-GPLv3-brightgreen.svg)](https://www.gnu.org/licenses/gpl-3.0)

Ansible collection of playbooks and roles for Red Hat Satellite
automation.

## Contents

* [inventory](inventory)
  * Example inventory
* [vars_satellite.yml](vars_satellite.yml)
  * Vars file for installing and configuring Satellite
* [vault_satellite.yml](vault_satellite.yml)
  * Unencrypted example vault file
* [playbooks/satellite_install.yml](playbooks/satellite_install.yml)
  * Playbook to install Red Hat Satellite
* [playbooks/satellite_manifest](playbooks/satellite_manifest.yml)
  * Playbook to upload Satellite manifest
* [playbooks/satellite_configure.yml](playbooks/satellite_configure.yml)
  * Playbook to configure Satellite
* [playbooks/satellite_sync_repos.yml](playbooks/satellite_sync_repos.yml)
  * Playbook to sync repositories
* [playbooks/capsule_install.yml](playbooks/capsule_install.yml)
  * Playbook to install Satellite Capsules

Depending on the environment and requirements, separate vars files,
group vars, variables defined in an inventory, or some other approach
might be warranted for providing Satellite configuration. These
examples aim to provide a basic starting point for typical
installations.

These playbooks have been tested most recently using Ansible 2.12 to
install Satellite 6.11 on RHEL 8.6.

## Quick Usage Example

To install this collection from GitHub:

```
ansible-galaxy collection install git+https://github.com/myllynen/satellite-automation,master
```

Note that the playbooks to install Satellite and Capsules expect basics
such as VMs, networking, DNS, timesync, repositories, and SELinux being
configured in advance according to recommendations and
[requirements](https://access.redhat.com/documentation/en-us/red_hat_satellite/).
Use
[rhel-system-roles](https://console.redhat.com/ansible/automation-hub/repo/published/redhat/rhel_system_roles)
and
[rhel-ansible-roles](https://github.com/myllynen/rhel-ansible-roles)
to apply such basic configurations as needed.

To install Red Hat Satellite, upload and refresh manifest, configure
Satellite, and install Satellite Capsules:

```
# Edit inventory and settings to suite local environment
vi inventory vars_satellite.yml vault_satellite.yml
# Install Red Hat Satellite
ansible-playbook -i inventory -e @vars_aap.yml -e @vault_aap.yml \
  myllynen.satellite_automation.satellite_install.yml
# Upload and refresh manifest
ansible-playbook -i inventory -e @vars_aap.yml -e @vault_aap.yml \
  myllynen.satellite_automation.satellite_manifest.yml
# Configure Satellite
ansible-playbook -i inventory -e @vars_aap.yml -e @vault_aap.yml \
  myllynen.satellite_automation.satellite_configure.yml
# Sync repositories
ansible-playbook -i inventory -e @vars_aap.yml -e @vault_aap.yml \
  myllynen.satellite_automation.satellite_sync_repos.yml
# Install Satellite Capsules
ansible-playbook -i inventory -e @vars_aap.yml -e @vault_aap.yml \
  myllynen.satellite_automation.capsule_install.yml
```

## See Also

See also
[https://access.redhat.com/documentation/en-us/red_hat_satellite](https://access.redhat.com/documentation/en-us/red_hat_satellite).

See also
[https://console.redhat.com/ansible/automation-hub/repo/published/redhat/satellite](https://console.redhat.com/ansible/automation-hub/repo/published/redhat/satellite).

See also
[https://console.redhat.com/ansible/automation-hub/repo/published/redhat/rhel_system_roles](https://console.redhat.com/ansible/automation-hub/repo/published/redhat/rhel_system_roles).

See also
[https://github.com/myllynen/rhel-ansible-roles](https://github.com/myllynen/rhel-ansible-roles).

## License

GPLv3+
