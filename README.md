Chrony NTP Server
=========

This role is designed to setup a GPS-based Stratum 1 timeserver on a Raspberry Pi using Chrony.

Requirements
------------

This role is designed to run on Ubuntu 24.04 Server on an RPi4 with an [uputronics GPS hat](https://store.uputronics.com/products/raspberry-pi-gps-rtc-expansion-board). 

This role is likely to work with other GPS hats, Rpi versions, and Ubuntu versions, but is not tested.

PRs to add support for other configurations are welcome.

Role Variables
--------------

See [argument_specs.yml](meta/argument_specs.yml) for a list of variables in this role.

Dependencies
------------

Requires `community.general.ufw` module if `ansible_royco_ntp_use_ufw: true`

Example Playbook
----------------

This playbook would deploy Chrony as an NTS client and NTP server on hosts without a GPS:

    - hosts: ntp-servers
      roles:
         - { role: ansible-royco-ntp, ansible_royco_ntp_use_gps: false }

This playbook would deploy Chrony as an NTS client and NTP server using GPS and excluding non-US sources for time:

    - hosts: ntp-servers
      roles:
         - { role: ansible-royco-ntp, ansible_royco_ntp_sources_use_global: false }


License
-------

GNU GENERAL PUBLIC LICENSE Version 3

Author Information
------------------

Matthew Roy

