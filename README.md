Ansible Role Telegraf
=====================

[![Build Status](https://travis-ci.org/juliendufresne/ansible-role-telegraf.svg?branch=master)](https://travis-ci.org/juliendufresne/ansible-role-telegraf)

Fully configurable [InfluxDB](https://influxdata.com/time-series-platform/telegraf/) installation and management for versions 0.13+.  

Requirements
------------

Tested with latest version of ansible (2.1.0)  
Should work for ansible >= 1.9 (usage of `become` and `become_user`)

Role Variables
--------------

Every variables are defined in [defaults/main.yml](defaults/main.yml). I will only describe some of them here.  
This role will install some packages with the `telegraf_install_user` user (Default: root).  

You can define global tags with the `telegraf__conf__global_tags` dictionary.  
Example:
```yaml
telegraf__conf__global_tags:
    dc: "us-east-1"
    rack: "1a"
    user: "$USER"
```

Every agent configuration are available with the `telegraf__conf__agent__*` prefix:
```yaml
telegraf__conf__agent__interval: "10s"
telegraf__conf__agent__round_interval: true
telegraf__conf__agent__metric_batch_size: 1000
telegraf__conf__agent__metric_buffer_limit:  10000
telegraf__conf__agent__collection_jitter: "0s"
telegraf__conf__agent__flush_interval: "10s"
telegraf__conf__agent__flush_jitter: "0s"
telegraf__conf__agent__debug: false
telegraf__conf__agent__quiet: false
telegraf__conf__agent__hostname: ""
telegraf__conf__agent__omit_hostname: false
```

### Defining outputs and inputs

The `telegraf__conf__plugins` variable is an array that allows you to 
control the list of inputs and outputs.  
Each item may contains:

| name       | type       | default | description                                                                                                      |
| ---------- | ---------- | ------- | ---------------------------------------------------------------------------------------------------------------- |
| type       | string     | inputs  | type of plugin. one of "inputs" or "outputs"                                                                     |
| name       | string     | -       | Name of the plugin (REQUIRED)                                                                                    |
| enabled    | boolean    | true    | Is the plugin enabled or not ? Allows you to keep configuration and be able to disable a plugin anyway           |
| parameters | dictionary | -       | Defines the list of parameters required                                                                          |
| sections   | array      | -       | If the plugin requires some subsection, you can define them in here. A section is basically the same as a plugin |

Example:
```yaml

telegraf__conf__plugins:
  - type: outputs
    name: influxdb
    parameters:
      urls:
        - "http://localhost:8086"
      database: "telegraf"
      precision: "s"
      retention_policy: "default"
      write_consistency: "any"
      timeout: "5s"
  - name: cpu
    parameters:
      percpu: true
      totalcpu: true
      fielddrop:
        - "time_*"
  - name: disk
    parameters:
      ignore_fs:
        - "tmpfs"
        - "devtmpfs"
  - name: diskio
  - name: kernel
  - name: mem
  - name: processes
  - name: swap
  - name: system
  - name: cloudwatch
    enabled: true
    parameters:
      region: 'us-east-1'
      period: "1m"
      delay: "1m"
      interval: "1m"
      namespace: "AWS/ELB"
    sections:
      - name: cloudwatch.metrics
        enabled: true
        parameters:
          names:
            - 'Latency'
            - 'RequestCount'
        sections:
          - name: cloudwatch.metrics.dimensions
            enabled: false
            parameters:
              name: 'LoadBalancerName'
              value: 'p-example'
          - name: cloudwatch.metrics.dimensions
            parameters:
              name: 'AvailabilityZone'
              value: '*'
```

will output something like:

```

###############################################################################
#                            OUTPUT PLUGINS                                   #
###############################################################################
[[outputs.influxdb]]
  retention_policy = "default"
  timeout = "5s"
  database = "telegraf"
  precision = "s"
  write_consistency = "any"
  urls = [
    "http://localhost:8086",
  ]


###############################################################################
#                            INPUT PLUGINS                                    #
###############################################################################
[[inputs.cpu]]
  fielddrop = [
    "time_*",
  ]
  totalcpu = true
  percpu = true

[[inputs.disk]]
  ignore_fs = [
    "tmpfs",
    "devtmpfs",
  ]

[[inputs.diskio]]

[[inputs.kernel]]

[[inputs.mem]]

[[inputs.processes]]

[[inputs.swap]]

[[inputs.system]]

[[inputs.cloudwatch]]
  delay = "1m"
  region = "us-east-1"
  interval = "1m"
  namespace = "AWS/ELB"
  period = "1m"
  [[inputs.cloudwatch.metrics]]
    names = [
      "Latency",
      "RequestCount",
    ]
    [[inputs.cloudwatch.metrics.dimensions]]
      name = "AvailabilityZone"
      value = "*"
```

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

MIT / BSD

Author Information
------------------

This role was created in 2016 by [Julien Dufresne](http://www.juliendufresne.fr).
