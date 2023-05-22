# capsule_install role

[![License: GPLv3](https://img.shields.io/badge/license-GPLv3-brightgreen.svg)](https://www.gnu.org/licenses/gpl-3.0)

Please see the collection main page for a higher level description.

This role uses Capsule certificate archives from Satellite at
`/root/{{ inventory_hostname }}-certs.tar`. If the files do not
exist yet then the `capsule_generate_certs` role can be used to
create them.

## Configuration

Below are the role default values from defaults/main.yml:

<pre>
---
satellite_capsule_firewall_zone: public

satellite_capsule_register_insights: false

satellite_capsule_installer_custom_parameters: >
  --no-colors
  --skip-checks-i-know-better
  --enable-foreman-proxy-plugin-openscap
  --no-enable-foreman-proxy-plugin-discovery
  --foreman-proxy-content-enable-ansible true
  --foreman-proxy-content-enable-docker false
</pre>

## License

GPLv3+
