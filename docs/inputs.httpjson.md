# [[inputs.httpjson]]

[plugin on GitHub](https://github.com/influxdata/telegraf/tree/master/plugins/inputs/httpjson)

Example:

```yml
telegraf__conf__inputs__httpjson:
  - enabled: false
    name: "webserver_stats"
    servers:
      - "http://localhost:9999/stats/"
      - "http://localhost:9998/stats/"
    method: "GET"
    tag_keys: []
    parameters:
      event_type: "cpu_spike"
      threshold: "0.75"
    headers:
      "X-Auth-Token": "my-xauth-token"
      apiVersion: "v1"
    ssl__enabled: false
    ssl__ssl_ca: "/etc/telegraf/ca.pem"
    ssl__ssl_cert: "/etc/telegraf/cert.pem"
    ssl__ssl_key: "/etc/telegraf/key.pem"
    ssl__insecure_skip_verify: false
  - enabled: true
    name: "webserver_stats2"
    servers:
      - "http://localhost:9999/stats/"
      - "http://localhost:9998/stats/"
    method: "GET"
    tag_keys: []
    parameters:
      event_type: "cpu_spike"
      threshold: "0.75"
    headers:
      "X-Auth-Token": "my-xauth-token"
      apiVersion: "v1"
    ssl__enabled: false
    ssl__ssl_ca: "/etc/telegraf/ca.pem"
    ssl__ssl_cert: "/etc/telegraf/cert.pem"
    ssl__ssl_key: "/etc/telegraf/key.pem"
    ssl__insecure_skip_verify: false
```

