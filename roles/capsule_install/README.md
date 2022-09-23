# capsule_install role

[![License: GPLv3](https://img.shields.io/badge/license-GPLv3-brightgreen.svg)](https://www.gnu.org/licenses/gpl-3.0)

Please see the collection main page for a higher level description.

## Configuration

Below are the role default values from defaults/main.yml:

<pre>
---
satellite_capsule_firewall_zone: public

satellite_capsule_installer_custom_parameters: >
  --no-colors
  --disable-system-checks
  --enable-foreman-proxy-plugin-openscap
</pre>

## License

GPLv3+
