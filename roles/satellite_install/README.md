# satellite_install role

[![License: GPLv3](https://img.shields.io/badge/license-GPLv3-brightgreen.svg)](https://www.gnu.org/licenses/gpl-3.0)

Please see the collection main page for a higher level description.

## Configuration

Below are the role default values from defaults/main.yml:

<pre>
---
satellite_server_url:

satellite_organization:
satellite_location:

satellite_validate_certs: true

satellite_firewall_zone: public

satellite_register_insights: false

satellite_installer_custom_parameters: >
  --no-colors
  --tuning development
  --skip-checks-i-know-better
  --enable-foreman-plugin-rh-cloud
  --enable-foreman-plugin-remote-execution
  --enable-foreman-plugin-openscap
  --enable-foreman-plugin-webhooks
  --enable-foreman-proxy-plugin-openscap
  --no-enable-foreman-plugin-discovery
  --no-enable-foreman-proxy-plugin-discovery
  --foreman-proxy-content-enable-ansible true
  --foreman-proxy-content-enable-docker false
  --foreman-plugin-tasks-automatic-cleanup true
</pre>

## License

GPLv3+
