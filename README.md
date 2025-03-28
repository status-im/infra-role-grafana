# Description

This role configures a docker container running [Grafana](http://docs.grafana.org/) as a metrics dashboard.

It connects to a configured instance of [Prometheus](https://prometheus.io/docs/introduction/overview/) or [Cortex](https://cortexmetrics.io/) to query for metrics.

# Configuration

The main settings that matter are:
```yaml
grafana_domain: 'grafana.example.org'
grafana_username: 'admin'
grafana_password: 'super-secret-password'
grafana_version: '7.3.4'
grafana_prometheus_sources:
  - { name: 'node-01', addr: '1.1.1.1', port: 8080 }
  - { name: 'node-02', addr: '1.1.1.2', port: 8080, path: 'proxy/' }
```
You should also configure OAuth with Github:
```yaml
grafana_oauth_id: '123qwe123qwe123'
grafana_oauth_secret: 'qweasdqweasdqweasdqweasd'
grafana_oauth_gh_org: 'evil-corp'
grafana_oauth_gh_team_ids: [ 1234, 5678 ]
```
Or with Keycloak:
```yaml
grafana_oauth_id:     'client-id'
grafana_oauth_secret: 'client-secret'
grafana_oauth_type:   'keycloak'
grafana_oauth_keycloak_role: "contains(groups[*], 'admin') && 'Admin' || contains(groups[*], 'editor') && 'Editor' || 'Viewer'"
grafana_oauth_keycloak_url: 'https://keycloak.com/realms/realm'
```
Optional email configuration might be useful:
```yaml
grafana_smtp_enabled: true
grafana_smtp_from_addr: 'grafana@example.org'
grafana_smtp_from_name: 'Grafana Email'
grafana_smtp_host: 'smtp'.example.org
grafana_smtp_port: 587
grafana_smtp_user: 'grafana'
grafana_smtp_pass: 'super-secret-password'
```
You can optionally allow anonymous access:
```yaml
grafana_anonymous: true
```
You can defined Postgre data source connections with:
```yaml
grafana_postgres_sources:
  - name:     'database'
    host:     'hostname'
    database: 'db-name'
    port:     port
    user:     'user'
    password: 'password'
```

Default date format can be updated:
```yaml
grafana_full_date_format: 'YYYY-MM-DD'
```

# Details

Configuration consists of two templates:

* `grafana.ini.j2` - Grafana main configuration file.
* `prometheus.yml.j2` - Initial configuration of the query backend(s).
