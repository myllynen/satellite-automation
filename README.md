# Red Hat Satellite Automation

[![License: GPLv3](https://img.shields.io/badge/license-GPLv3-brightgreen.svg)](https://www.gnu.org/licenses/gpl-3.0)

Ansible collection of playbooks and roles for Red Hat Satellite
automation.

## Contents

* [inventory](inventory)
  * Example inventory
* [vars_satellite.yml](vars_satellite.yml)
  * Vars file for installing and configuring Satellite and Capsules
* [vault_satellite.yml](vault_satellite.yml)
  * Unencrypted example vault file
* [playbooks/satellite_install.yml](playbooks/satellite_install.yml)
  * Playbook to install Red Hat Satellite
* [playbooks/satellite_manifest](playbooks/satellite_manifest.yml)
  * Playbook to upload Satellite manifest
* [playbooks/capsule_install.yml](playbooks/capsule_install.yml)
  * Playbook to install Satellite Capsules
* [playbooks/satellite_configure.yml](playbooks/satellite_configure.yml)
  * Playbook to configure Satellite
* [playbooks/satellite_sync_repos.yml](playbooks/satellite_sync_repos.yml)
  * Playbook to sync repositories

Depending on the environment and requirements separate playbooks and/or
vars files, group vars, variables defined in an inventory, or some other
approach might be appropriate for providing Satellite configuration.
These examples aim to provide a known-good starting point for typical
installations.

These playbooks have been tested most recently using Ansible 2.14 to
install Satellite 6.13 on RHEL 8.8.

## Quick Usage Example

In a typical use case these playbooks and roles would be used to install
Satellite and Capsules according to the provided configuration. After
that the example `satellite_configure.yml` playbook together with the
`vars_satellite.yml` file could be used as a starting point for defining
a real site- or organization-specific setup based on local needs and
requirements.

To install this collection from GitHub:

```
ansible-galaxy collection install git+https://github.com/myllynen/satellite-automation,master
```

Note that the playbooks to install Satellite and Capsules expect basics
such as VMs, networking, DNS, timesync,
[repositories](https://github.com/myllynen/rhel-ansible-roles/tree/master/roles/repository_setup),
and SELinux being configured in advance according to recommendations
and
[requirements](https://access.redhat.com/documentation/en-us/red_hat_satellite/).
Use
[rhel-system-roles](https://console.redhat.com/ansible/automation-hub/repo/published/redhat/rhel_system_roles)
and
[rhel-ansible-roles](https://github.com/myllynen/rhel-ansible-roles)
to apply such basic configurations as needed.

To use a custom certificate with Satellite please refer to the Satellite
installation guide for the needed installer parameters. By default a
self-signed certificate will be used.

The `capsule_install.yml` playbook uses the `capsule_generate_certs`
role to create Capsule certificate archives at Satellite for each
Capsule at `/root/{{ inventory_hostname }}-certs.tar`. Create these
archives with custom certificates manually before running the playbook
if needed.

Use `satellite-installer --scenario satellite --full-help` to see all
the available intaller parameters.

To install Red Hat Satellite, upload and refresh manifest, install
Capsules, and do initial Satellite configuration:

```
# Edit inventory and settings to suite local environment
vi inventory vars_satellite.yml vault_satellite.yml
# Install Red Hat Satellite
ansible-playbook -i inventory -e @vars_satellite.yml -e @vault_satellite.yml \
  myllynen.satellite_automation.satellite_install.yml
# Upload and refresh manifest
ansible-playbook -i inventory -e @vars_satellite.yml -e @vault_satellite.yml \
  myllynen.satellite_automation.satellite_manifest.yml
# Install Satellite Capsules
ansible-playbook -i inventory -e @vars_satellite.yml -e @vault_satellite.yml \
  myllynen.satellite_automation.capsule_install.yml
# Configure Satellite
ansible-playbook -i inventory -e @vars_satellite.yml -e @vault_satellite.yml \
  myllynen.satellite_automation.satellite_configure.yml
# Sync repositories
ansible-playbook -i inventory -e @vars_satellite.yml -e @vault_satellite.yml \
  myllynen.satellite_automation.satellite_sync_repos.yml
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
