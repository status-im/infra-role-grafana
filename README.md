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
You should also configure OAuth:
```yaml
grafana_oauth_id: '123qwe123qwe123'
grafana_oauth_secret: 'qweasdqweasdqweasdqweasd'
grafana_oauth_gh_org: 'evil-corp'
grafana_oauth_gh_team_ids: [ 1234, 5678 ]
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

# Details

Configuration consists of two templates:

* `grafana.ini.j2` - Grafana main configuration file.
* `prometheus.yml.j2` - Initial configuration of the query backend(s).

# Backups

Done by a Sytemd timer daily:
```
 > sudo systemctl list-timers 'grafana-backup'
NEXT                        LEFT          LAST PASSED UNIT                 ACTIVATES             
Wed 2020-12-09 00:00:00 UTC 3h 10min left n/a  n/a    grafana-backup.timer grafana-backup.service

 > sudo systemctl status grafana-backup
● grafana-backup.service - Grafana batabase backup
     Loaded: loaded (/lib/systemd/system/grafana-backup.service; static; vendor preset: enabled)
     Active: inactive (dead) since Tue 2020-12-08 20:48:45 UTC; 6s ago
TriggeredBy: ● grafana-backup.timer
       Docs: https://github.com/status-im/infra-role-systemd-timer
    Process: 1938613 ExecStart=/usr/local/bin/grafana-backup (code=exited, status=0/SUCCESS)
   Main PID: 1938613 (code=exited, status=0/SUCCESS)
```
```
 > ls -lh /var/backups/grafana
total 2.3M
-rw-r--r-- 1 root root 563K Dec  5 20:20 grafana-20201205-202043.db.gz
-rw-r--r-- 1 root root 563K Dec  6 20:48 grafana-20201206-204838.db.gz
-rw-r--r-- 1 root root 563K Dec  7 20:48 grafana-20201207-204840.db.gz
-rw-r--r-- 1 root root 563K Dec  8 20:48 grafana-20201208-204845.db.gz
```
