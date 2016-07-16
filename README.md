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
| Debian 8.4 (jessie)    | 2016-07-16 20:28:00 | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg) | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg)  |
| CentOS 6.8 (Final)     | 2016-07-16 20:17:06 | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg) | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg)  |
| CentOS 7.2.1511 (Core) | 2016-07-16 20:21:02 | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg) | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg)  |
| Debian 7.9 (wheezy)    | 2016-07-16 17:20:59 | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg) | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg)  |
| Debian 7.10 (wheezy)   | 2016-07-16 20:24:30 | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg) | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg)  |
| Debian 8.2 (jessie)    | 2016-07-16 17:30:40 | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg) | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg)  |
| Debian 8.3 (jessie)    | 2016-07-16 17:34:20 | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg) | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg)  |
| Ubuntu 12.04 (precise) | 2016-07-16 20:31:10 | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg) | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg)  |
| Ubuntu 14.04 (trusty)  | 2016-07-16 21:06:32 | ![FAIL](https://img.shields.io/badge/status-fail-red.svg) | ![FAIL](https://img.shields.io/badge/status-fail-red.svg)  |
| Ubuntu 15.04 (vivid)   | 2016-07-16 20:37:11 | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg) | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg)  |
| Ubuntu 15.10 (wily)    | 2016-07-16 20:40:31 | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg) | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg)  |
| Ubuntu 16.04 (xenial)  | 2016-07-16 20:43:57 | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg) | ![OK](https://img.shields.io/badge/status-pass-brightgreen.svg)  |

_**Note:** Ubuntu 14.04 currently is failing because it cannot download the package._

> * **From scratch** represents the results when you run your ansible playbook on a clean distribution
> * **Idempotency** represents the second run of your ansible playbook. It should not contains any changes tasks


Requirements
------------

**Ansible version**  
This role requires ansible 2.1.0+ (apt_repository use filename)  

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
