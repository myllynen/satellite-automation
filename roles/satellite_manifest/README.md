# satellite_manifest role

[![License: GPLv3](https://img.shields.io/badge/license-GPLv3-brightgreen.svg)](https://www.gnu.org/licenses/gpl-3.0)

Please see the collection main page for a higher level description.

## Configuration

Below are the role default values from defaults/main.yml:

<pre>
---
# Either copy a manifest file from the local system
# or download directly from Red Hat Customer Portal
satellite_manifest_download: false

# Refresh manifest in Satellite
satellite_manifest_refresh: false

# Manifest path on the target host
satellite_manifest_path: /root/satellite_manifest.zip

#
# Required when copying manifest
#
# Manifest file to copy to target
satellite_manifest_file: files/satellite_manifest.zip

#
# Required when downloading manifest
#
# Manifest download UUID
satellite_manifest_uuid:
</pre>

## License

GPLv3+
