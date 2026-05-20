# Red Hat Satellite Automation

[![License: GPLv3](https://img.shields.io/badge/license-GPLv3-brightgreen.svg)](https://www.gnu.org/licenses/gpl-3.0)

Ansible playbooks for Red Hat Satellite automation.

## Contents

* [inventory](inventory)
  * Example inventory
* [vars_satellite.yml](vars_satellite.yml)
  * Vars file for installing Satellite
* [vars_manifest.yml](vars_manifest.yml)
  * Vars file for Satellite manifest handling
* [vars_config.yml](vars_config.yml)
  * Vars file for configuring Satellite
* [vars_capsule.yml](vars_capsule.yml)
  * Vars file for installing Satellite Capsules
* [vault_satellite.yml](vault_satellite.yml)
  * Unencrypted example vault file
* [satellite_host_repos.yml](satellite_host_repos.yml)
  * Playbook to configure Satellite host repositories
* [satellite_host_prepare.yml](satellite_host_prepare.yml)
  * Playbook to prepare hosts for installation
* [satellite_install.yml](satellite_install.yml)
  * Playbook to install Red Hat Satellite
* [satellite_manifest.yml](satellite_manifest.yml)
  * Playbook to upload Satellite manifest
* [satellite_configure.yml](satellite_configure.yml)
  * Playbook to configure Satellite
* [satellite_sync_repos.yml](satellite_sync_repos.yml)
  * Playbook to sync repositories
* [capsule_host_repos.yml](capsule_host_repos.yml)
  * Playbook to configure Satellite Capsules host repositories
* [capsule_install.yml](capsule_install.yml)
  * Playbook to install Satellite Capsules
* [satellite_sync_capsules.yml](satellite_sync_capsules.yml)
  * Playbook to sync Capsules from Satellite
* [satellite_resources.yml](satellite_resources.yml)
  * Playbook to display Satellite resources
* [satellite_update.yml](satellite_update.yml)
  * Playbook to update Satellite and Capsules
* [satellite_upgrade.yml](satellite_upgrade.yml)
  * Playbook to upgrade Satellite and Capsules
* [client_host_repos.yml](client_host_repos.yml)
  * Playbook to register and configure managed hosts

To create the required Satellite manifest, see
[Red Hat Subscription Allocations tool](https://access.redhat.com/management/subscription_allocations)
and
[obtaining a manifest file](https://docs.redhat.com/en/documentation/red_hat_satellite/6.19/html/installing_satellite_server_in_a_connected_network_environment/installing-satellite-server#importing-red-hat-subscription-manifests-into-satellite).

Depending on the environment and requirements separate playbooks and/or
vars files, group vars, variables defined in an inventory, or some other
approach might be appropriate for providing Satellite configuration.
These examples aim to provide a known-good starting point for typical
installations.

## Quick Usage Example

In a typical use case these playbooks and roles would be used to install
and configure Satellite and Capsules according to requirements. The
example `satellite_configure.yml` playbook together with the
`vars_config.yml` file could be used as a starting point for defining a
complete real-world setup based on local requirements.

Note that the playbooks to install Satellite and Capsules expect basics
such as VMs, networking, DNS, timesync, RHEL repositories, and SELinux
being configured in advance according to local and
[Satellite requirements](https://docs.redhat.com/en/documentation/red_hat_satellite/).
Use
[rhel-system-roles](https://console.redhat.com/ansible/automation-hub/repo/published/redhat/rhel_system_roles)
and
[rhel-ansible-roles](https://github.com/myllynen/rhel-ansible-roles)
to apply such basic configurations as needed. Use the example
`satellite_host_prepare.yml` playbook to automate this process.

To use a custom certificate with Satellite please refer to the Satellite
installation guide for the needed installer parameters. By default a
self-signed certificate will be used.

The `capsule_install.yml` playbook creates Capsule certificate archives
on the Satellite host for each Capsule as
`/root/{{ inventory_hostname }}-certs.tar`. Create these archives with
custom certificates manually before running the playbook if needed.

Use `satellite-installer --scenario satellite --full-help` to see all
the available intaller parameters. NB. Newer Satellite versions often
change installer options so be prepared to review them when using a
different version than these examples were tested against.

To install Red Hat Satellite, upload or refresh manifest, configure
Satellite, and install and sync Capsules:

```
# Edit inventory and settings to suite local environment
vi inventory vault_satellite.yml
vi vars_satellite.yml vars_manifest.yml vars_capsule.yml vars_config.yml

# Configure connected Satellite host repositories
# This requires RHEL system roles to be installed on the current host
# If RHEL system roles not available or in a disconnected environment,
# configure the required repos manually using subscription-manager
ansible-playbook -i inventory satellite_host_repos.yml \
  -e @vault_satellite.yml

# Example to prepare Satellite host for installation,
# make sure to review SSH/user config before applying
vi satellite_host_prepare.yml
ansible-playbook -i inventory -l satellite satellite_host_prepare.yml

# Install Red Hat Satellite
ansible-playbook -i inventory satellite_install.yml \
  -e @vault_satellite.yml -e @vars_satellite.yml

# Upload or refresh manifest
ansible-playbook -i inventory satellite_manifest.yml \
  -e @vault_satellite.yml -e @vars_satellite.yml -e @vars_manifest.yml

# Apply initial Satellite configuration, including products and repos
ansible-playbook -i inventory satellite_configure.yml \
  -e @vault_satellite.yml -e @vars_satellite.yml -e @vars_config.yml \
  -e satellite_full_config=false

# Sync enabled repositories on Satellite
ansible-playbook -i inventory satellite_sync_repos.yml \
  -e @vault_satellite.yml -e @vars_satellite.yml

# Apply full Satellite configuration, including AKs, CVs, and CCVs
ansible-playbook -i inventory satellite_configure.yml \
  -e @vault_satellite.yml -e @vars_satellite.yml -e @vars_config.yml

# Configure Capsule hosts repositories using Satellite
# This requires RHEL system roles to be installed on the current host
# If RHEL system roles not available, configure the required repos manually
ansible-playbook -i inventory capsule_host_repos.yml \
  -e @vault_satellite.yml -e @vars_satellite.yml

# Example to prepare Capsule hosts for installation,
# make sure to review SSH/user config before applying
vi satellite_host_prepare.yml
ansible-playbook -i inventory -l capsules satellite_host_prepare.yml

# Install Satellite Capsules
ansible-playbook -i inventory capsule_install.yml \
  -e @vars_capsule.yml

# Apply full Satellite configuration, including Capsules and HGs
ansible-playbook -i inventory satellite_configure.yml \
  -e @vault_satellite.yml -e @vars_satellite.yml -e @vars_config.yml
  -e satellite_content_view_publish=false

# Sync Capsules from Satellite
ansible-playbook -i inventory satellite_sync_capsules.yml \
  -e @vault_satellite.yml -e @vars_satellite.yml

# Update Satellite and Capsules as needed
ansible-playbook -i inventory satellite_update.yml

# Register and configure managed hosts
ansible-playbook -i inventory -l clients client_host_repos.yml \
  -e @vault_satellite.yml -e @vars_satellite.yml
```

## See Also

See also
[https://docs.redhat.com/en/documentation/red_hat_satellite/](https://docs.redhat.com/en/documentation/red_hat_satellite/).

See also
[https://www.redhat.com/en/blog/21-things-every-red-hat-satellite-user-should-know](https://www.redhat.com/en/blog/21-things-every-red-hat-satellite-user-should-know).

See also
[https://console.redhat.com/ansible/automation-hub/collections/published/redhat/satellite/](https://console.redhat.com/ansible/automation-hub/collections/published/redhat/satellite/).

See also
[https://console.redhat.com/ansible/automation-hub/collections/published/redhat/satellite_operations/](https://console.redhat.com/ansible/automation-hub/collections/published/redhat/satellite_operations/).

See also
[https://console.redhat.com/ansible/automation-hub/collections/published/redhat/rhel_system_roles](https://console.redhat.com/ansible/automation-hub/collections/published/redhat/rhel_system_roles).

See also
[https://github.com/myllynen/rhel-ansible-roles](https://github.com/myllynen/rhel-ansible-roles).

## License

GPLv3+
