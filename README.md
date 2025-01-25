Chrony NTP Server
=========

This role is designed to setup a GPS-based Stratum 1 timeserver on a Raspberry Pi using Chrony.

Goals:

 - Reasonably hardened client config using NTS sources only
 - GPS config using PPS with NEMA for startup
 - Hardened server config that can be publicly exposed
 - Future: support for being an NTS server
 - Future: support publishing status

Testing tools:

 - Verify PPS with `sudo ppstest /dev/pps0`
 - Verify gpsd with `gpsmon`
 - Verify GPS fix with `cgps -s`
 - Configure GPS with `ubxtool -P 18.00 --device /dev/ttyS0 <command>` see [man ubxtool](https://gpsd.gitlab.io/gpsd/ubxtool.html)
 - Verify chrony sources with `watch -n 1 chronyc sources -v`

You may wish to run this one-time GPS module configuration based on [this page](https://conorrobinson.ie/raspberry-pi-ntp-server-part-6/#pifinding-zero-tc):

 - `ubxtool -P 18.00 --device /dev/ttyS0 -p CFG-PMS,0` set full power mode
 - `ubxtool -P 18.00 --device /dev/ttyS0 -p MODEL,2` set dynamics model to stationary
 - `ubxtool -P 18.00 --device /dev/ttyS0 -d SBAS` disable SBAS
 - `ubxtool -P 18.00 --device /dev/ttyS0 -e GALILEO` enable Galileo constellation
 - `ubxtool -P 18.00 --device /dev/ttyS0 -d GLONASS` disable GLONASS constellation
 - `ubxtool -P 18.00 --device /dev/ttyS0 -d BDS` disable Beido constellation
 - `ubxtool -P 18.00 --device /dev/ttyS0 -p SAVE` save to flash

Requirements
------------

This role is designed to run on Ubuntu 24.04 Server on an RPi4 with an [uputronics GPS hat](https://store.uputronics.com/products/raspberry-pi-gps-rtc-expansion-board). 

This role is likely to work with other GPS hats, Rpi versions, and Ubuntu versions, but is not tested.

PRs to add support for other configurations are welcome.

Role Variables
--------------

See [argument_specs.yml](meta/argument_specs.yml) for a list of variables in this role.

All variables have a default set in [defaults/main.yml](defaults/main.yml)

Dependencies
------------

Requires `community.general.ufw` module if `ansible_royco_ntp_use_ufw: true`. This module is included by default in ansible, but may not be in ansible-core.

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

