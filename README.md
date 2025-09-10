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
* [vars_capsule.yml](vars_capsule.yml)
  * Vars file for installing Satellite Capsules
* [vars_config.yml](vars_config.yml)
  * Vars file for configuring Satellite
* [vault_satellite.yml](vault_satellite.yml)
  * Unencrypted example vault file
* [satellite_host_prepare.yml](satellite_host_prepare.yml)
  * Playbook to prepare hosts for installation
* [satellite_install.yml](satellite_install.yml)
  * Playbook to install Red Hat Satellite
* [satellite_manifest.yml](satellite_manifest.yml)
  * Playbook to upload Satellite manifest
* [capsule_install.yml](capsule_install.yml)
  * Playbook to install Satellite Capsules
* [satellite_configure.yml](satellite_configure.yml)
  * Playbook to configure Satellite
* [satellite_sync_repos.yml](satellite_sync_repos.yml)
  * Playbook to sync repositories

Depending on the environment and requirements separate playbooks and/or
vars files, group vars, variables defined in an inventory, or some other
approach might be appropriate for providing Satellite configuration.
These examples aim to provide a known-good starting point for typical
installations.

## Quick Usage Example

In a typical use case these playbooks and roles would be used to install
Satellite and Capsules according to the provided configuration. After
that the example `satellite_configure.yml` playbook together with the
`vars_config.yml` file could be used as a starting point for defining a
real site- or organization-specific setup based on local needs and
requirements.

Note that the playbooks to install Satellite and Capsules expect basics
such as VMs, networking, DNS, timesync, RHEL repositories, and SELinux
being configured in advance according to local and
[Satellite requirements](https://access.redhat.com/documentation/en-us/red_hat_satellite/).
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
the available intaller parameters.

To install Red Hat Satellite, upload or refresh manifest, install
Capsules, and do initial Satellite configuration:

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
# Prepare Satellite/Capsule hosts for installation,
# make sure to review SSH/user config before applying
vi satellite_host_prepare.yml
ansible-playbook -i inventory satellite_host_prepare.yml
# Install Red Hat Satellite
ansible-playbook -i inventory satellite_install.yml \
  -e @vault_satellite.yml -e @vars_satellite.yml
# Upload or refresh manifest
ansible-playbook -i inventory satellite_manifest.yml \
  -e @vault_satellite.yml -e @vars_satellite.yml -e @vars_manifest.yml
# Apply initial Satellite configuration
ansible-playbook -i inventory satellite_configure.yml \
  -e @vault_satellite.yml -e @vars_satellite.yml -e @vars_config.yml
# Sync enabled repositories on Satellite
ansible-playbook -i inventory satellite_sync_repos.yml \
  -e @vault_satellite.yml -e @vars_satellite.yml
# Configure Capsule host repositories using Satellite
# This requires RHEL system roles to be installed on the current host
# If RHEL system roles not available, configure the required repos manually
ansible-playbook -i inventory capsule_host_repos.yml \
  -e @vault_satellite.yml
# Install Satellite Capsules
ansible-playbook -i inventory capsule_install.yml \
  -e @vars_capsule.yml
# ... proceed to complete full configuration ...
```

## See Also

See also
[https://access.redhat.com/documentation/en-us/red_hat_satellite](https://access.redhat.com/documentation/en-us/red_hat_satellite).

See also
[https://www.redhat.com/en/blog/21-things-every-red-hat-satellite-user-should-know](https://www.redhat.com/en/blog/21-things-every-red-hat-satellite-user-should-know).

See also
[https://console.redhat.com/ansible/automation-hub/repo/published/redhat/satellite](https://console.redhat.com/ansible/automation-hub/repo/published/redhat/satellite).

See also
[https://console.redhat.com/ansible/automation-hub/repo/published/redhat/satellite_operations](https://console.redhat.com/ansible/automation-hub/repo/published/redhat/satellite_operations).

See also
[https://console.redhat.com/ansible/automation-hub/repo/published/redhat/rhel_system_roles](https://console.redhat.com/ansible/automation-hub/repo/published/redhat/rhel_system_roles).

See also
[https://github.com/myllynen/rhel-ansible-roles](https://github.com/myllynen/rhel-ansible-roles).

## License

GPLv3+
