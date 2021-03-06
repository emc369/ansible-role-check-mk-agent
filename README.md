# ansible-role-check-mk-agent
[![Build Status](https://travis-ci.org/elnappo/ansible-role-check-mk-agent.svg?branch=master)](https://travis-ci.org/elnappo/ansible-role-check-mk-agent) [![Ansible Galaxy](https://img.shields.io/badge/galaxy-elnappoo.check--mk--agent-blue.svg?style=flat)](https://galaxy.ansible.com/elnappoo/check-mk-agent/)

Installs check mk\_agent. Run it with systemd-socket, SSH with sudo or SSH as root (default). Get more information about check\_mk at [https://mathias-kettner.de/check_mk.html]()

## Features
* Install check_mk agent
* Query check_mk agent over systemd-socket (only with check_mk_agent >= v1.4), SSH as root or SSH with sudo
* Setup firewall if systemd-socket ist used (ufw or firewalld)
* Add SSH host key to check_mk server
* Install check_mk agent plugins/local checks and their dependencies
* Add hosts to check_mk server via WATO API

## Requirements
* Python requests >= v2.5.0

Only tested on Ubuntu 14.04, 16.04 and CentOS 7, should also run under Debian and RedHat.

## Install
    $ ansible-galaxy install elnappoo.check-mk-agent

## Role Variables
* `check_mk_agent_over_ssh: True`
* `check_mk_agent_with_sudo: False` Adds a user which is allowed to run check_mk_agent with sudo
* `check_mk_agent_add_host_pubkey: False` Import SSH host keys into your check_mk servers known_hosts file
* `check_mk_agent_monitoring_host:` Hostname of your check_mk server
* `check_mk_agent_monitoring_user:` Username under which your check_mk instance runs
* `check_mk_agent_plugins_requirements: []` Requirements for extra plugins
* `check_mk_agent_plugins: []` List of extra plugins to install
* `check_mk_agent_local_checks: {}`
* `check_mk_agent_pubkey_file:` Path to SSH pubkey file
* `check_mk_agent_add_to_wato: False`
* `check_mk_agent_monitoring_host_folder: ""`
* `check_mk_agent_monitoring_host_discovery_mode: new`
* `check_mk_agent_monitoring_host_url:`
* `check_mk_agent_monitoring_host_wato_username:`
* `check_mk_agent_monitoring_host_wat_secret:`
* `check_mk_agent_setup_firewall: True` Add firewall rule (ufw/firewalld) when using systemd-socket

## Included check_mk extra plugins
* apache\_status
* db2\_mem
* dnsclient
* hpux\_lunstats
* hpux\_statgrab
* jar\_signature
* kaspersky\_av
* lnx\_quota
* mailman\_lists
* mk\_inventory.aix
* mk\_inventory.linux
* mk\_inventory.solaris
* mk\_jolokia
* mk\_logins
* mk\_logwatch
* mk\_logwatch\_aix
* mk\_mysql
* mk\_oracle
* mk\_oracle.aix
* mk\_oracle.solaris
* mk\_oracle\_asm
* mk\_oracle\_crs
* mk\_postgres
* mk\_sap
* mk\_tsm
* mk\_zypper
* netstat.aix
* netstat.linux
* nfsexports
* nfsexports.solaris
* nginx\_status
* plesk\_backups
* plesk\_domains
* runas
* smart
* symantec\_av
* unitrends\_backup
* unitrends\_replication
* vxvm
* websphere\_mq

## Dependencies
None.

## Example Playbook

```yaml
- hosts: servers
  vars:
    check_mk_agent_pubkey_file: omd_rsa.pub
    check_mk_agent_add_host_pubkey: True
    check_mk_agent_monitoring_host: checkmk.example.com
    check_mk_agent_monitoring_user: monitoring
    check_mk_agent_add_to_wato: True
    check_mk_agent_monitoring_host_url: http://cmk.example.com/monitoring/
    check_mk_agent_monitoring_host_wato_username: ansible
    check_mk_agent_monitoring_host_wato_secret: 7JTuBt6nETYHG1GS
    check_mk_agent_local_checks:
      filecount:
        src: files/check_mk_local_checks/filecount
        cache_time: 600
      filestat:
        src: files/check_mk_local_checks/filestat

  roles:
     - elnappoo.check-mk-agent
```

## License

MIT

## Author Information

elnappo <elnappo@nerdpol.io>
