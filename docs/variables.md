Role variables
==============

Every variables are defined in [defaults/main.yml](defaults/main.yml). I will only describe some of them here.

Installation related variables
------------------------------

Packages are installed using the `telegraf_install_user` variable. Default user: root.

**Using a custom version**  
Telegraf version is defined by the `telegraf_install_version` variable.  
The default value (packaged) will use the package manager.

If you don't want to install the packaged version, you can specify a version number.

Example:
```yaml
telegraf_install_version: 0.13.1
```

This will download and install the binary from https://dl.influxdata.com/

Configuration file variables
----------------------------

* `telegraf_config_dir`: Defines the configuration directory. Default: "/etc/telegraf"
* `telegraf_config_file`: Defines the default configuration file. Default: "telegraf.conf"
* `telegraf_group`: Group who owns the configuration files. Default: root
* `telegraf_user`: User who owns the configuration files. Default: root

Defining global tags
--------------------

You can define global tags with the `telegraf__conf__global_tags` dictionary.

Example:
```yaml
telegraf__conf__global_tags:
    dc: "us-east-1"
    rack: "1a"
    user: "$USER"
```

Defining agent variables
------------------------

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

Defining plugins (outputs and inputs)
-------------------------------------

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
| tagpass    | dictionary | -       | Tag names and arrays of strings that are used to filter measurements by the current input. Each string in the array is tested as a glob match against the tag name, and if it matches the measurement is emitted. |
| tagdrop    | dictionary | -       |  The inverse of tagpass. If a tag matches, the measurement is not emitted. This is tested on measurements that have passed the tagpass test. |
| tags       | dictionary | -       | Adds some tags specific for this inputs |

### Example

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

Defining extra plugins
----------------------

Suppose you want to add an inputs only when a service is activated.  
For instance, let's suppose you will install elasticsearch and want to activate
the elasticsearch inputs in telegraf.  
This is not possible with the `telegraf__conf__plugins` because you want
it to be the same for every servers.  

What you can do is create a file in telegraf.d directory for each extra 
plugins you want to activate.
The `telegraf__conf__plugins__extra` variable is here for that.

### Example

```yml
# cat group_vars/search_servers
telegraf__conf__plugins__extra:
    - name: elasticsearch
      type: inputs
      enabled: true
      parameters:
        servers:
          - "http://localhost:9200"
        local: true
        cluster_health: false
```

This will produce a telegraf.d/elasticsearch.conf file
