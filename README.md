Ansible Role Telegraf
=====================

[![Build Status](https://travis-ci.org/juliendufresne/ansible-role-telegraf.svg?branch=master)](https://travis-ci.org/juliendufresne/ansible-role-telegraf)

Fully configurable [Telegraf](https://influxdata.com/time-series-platform/telegraf/) installation and management.  
By default, it will be installed using your Operating System package 
system but you can specify the version you want.

> THE “T” IN THE TICK STACK
> Telegraf is an open source agent written in Go for collecting metrics and data on the system it's running on or from other services. Telegraf writes data it collects to InfluxDB in the correct format.
> 
> — _From https://influxdata.com/time-series-platform/telegraf/_

Supported platforms
-------------------

_Table generated with [juliendufresne/test-ansible-roles](https://github.com/juliendufresne/test-ansible-roles)_

| Distribution           | Last check date     | From scratch    | Idempotency |
| ---------------------- | ------------------- | --------------- | ----------- |
| CentOS 6.8 (Final)     | 2016-07-14 18:04:24 | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg) | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg)  |
| CentOS 7.2.1511 (Core) | 2016-07-14 18:07:34 | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg) | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg)  |
| Debian 7.9 (wheezy)    | 2016-07-14 18:10:31 | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg) | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg)  |
| Debian 7.10 (wheezy)   | 2016-07-14 18:14:01 | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg) | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg)  |
| Debian 8.2 (jessie)    | 2016-07-14 18:21:14 | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg) | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg)  |
| Debian 8.3 (jessie)    | 2016-07-14 18:24:38 | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg) | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg)  |
| Debian 8.4 (jessie)    | 2016-07-14 18:28:05 | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg) | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg)  |
| Ubuntu 12.04 (precise) | 2016-07-14 18:31:13 | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg) | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg)  |
| Ubuntu 14.04 (trusty)  | 2016-07-14 18:34:32 | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg) | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg)  |
| Ubuntu 15.04 (vivid)   | 2016-07-14 18:38:18 | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg) | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg)  |
| Ubuntu 15.10 (wily)    | 2016-07-14 18:42:02 | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg) | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg)  |
| Ubuntu 16.04 (xenial)  | 2016-07-14 19:54:28 | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg) | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg)  |


Requirements
------------

**Ansible version**  
Tested with latest version of ansible (2.1.0)  
Should work for ansible >= 1.9 (usage of `become` and `become_user`)

**Jinja2 version**  
Works with version [2.8](http://jinja.pocoo.org/docs/dev/changelog/#version-2-8)+  
Requires equalto filter.

**Python version**  
If you use the packaged telegraf version, it should work with the packaged python version.

Specifying a custom telegraf version requires SSL certificate verification.  
Hence, you will either need python version 2.7.9+ or install `urllib3` 
`pyopenssl` `ndg-httpsclient` `pyasn1` python modules.

Role Variables
--------------

For a full list of all variables, please see the file called [variables.md](docs/variables.md).

Dependencies
------------

None.

Example Playbook
----------------

    - hosts: servers
      roles:
         - { role: juliendufresne.telegraf }

License
-------

MIT

Author Information
------------------

This role was created in 2016 by [Julien Dufresne](http://www.juliendufresne.fr).
