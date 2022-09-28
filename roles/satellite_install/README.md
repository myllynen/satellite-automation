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

satellite_installer_custom_parameters: >
  --no-colors
  --disable-system-checks
  --enable-foreman-plugin-rh-cloud
  --no-enable-foreman-plugin-discovery
  --foreman-proxy-plugin-remote-execution-ssh-install-key true
</pre>

## License

GPLv3+
