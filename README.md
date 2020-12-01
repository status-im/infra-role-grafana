# Description

This role configures a docker container running [Grafana](http://docs.grafana.org/) as a metrics dashboard.

It connects to a configured instance of [Prometheus](https://prometheus.io/docs/introduction/overview/) or [Cortex](https://cortexmetrics.io/) to query for metrics.

# Configuration

The main settings that matter are:
```yaml
grafana_domain: 'grafana.example.org'
grafana_version: '7.3.4'
grafana_prometheus_sources:
  - { name: 'node-01', addr: '1.1.1.1', port: 8080 }
  - { name: 'node-02', addr: '1.1.1.2', port: 8080, path: 'proxy/' }
```
You should also configure OAuth:
```yaml
grafana_oauth_id: '123qwe123qwe123'
grafana_oauth_secret: 'qweasdqweasdqweasdqweasd
```
Optional email configuration might be useful:
```yaml
grafana_smtp_enabled: true
grafana_smtp_from_addr: 'grafana@example.org'
grafana_smtp_from_name: ~
grafana_smtp_host: ~
grafana_smtp_port: ~
grafana_smtp_user: ~
grafana_smtp_pass: ~
```
You can optionally allow anonymous access:
```yaml
grafana_anonymous: true
```

# Details

Configuration consists of two templates:

* `grafana.ini.j2` - Grafana main configuration file.
* `backends.yml.j2` - Initial configuration of the query backend(s).
