# satellite_manifest role

[![License: GPLv3](https://img.shields.io/badge/license-GPLv3-brightgreen.svg)](https://www.gnu.org/licenses/gpl-3.0)

Please see the collection main page for a higher level description.

## Configuration

Below are the role default values from defaults/main.yml:

<pre>
---
# Either use a manifest file from the local system
# or download one from Red Hat Customer Portal
# Downloading will be done from the Satellite host
satellite_manifest_download: false

# Refresh manifest in Satellite
satellite_manifest_refresh: false

# Manifest file path to use on the Satellite host
# This is used both when copying or downloading
satellite_manifest_file_remote: "{{ ansible_facts.env.HOME + '/satellite_manifest.zip' }}"

#
# Required when copying manifest
#
# Manifest local file path to copy to the Satellite host
satellite_manifest_file_local: "{{ lookup('env', 'HOME') + '/satellite_manifest.zip' }}"

#
# Required when downloading manifest
#
# Manifest UUID to download
satellite_manifest_uuid:

# Red Hat Customer Portal credentials for downloading
# These should come from vault
#satellite_rhsm_username:
#satellite_rhsm_password:
</pre>

## License

GPLv3+
