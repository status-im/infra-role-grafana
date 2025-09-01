# Backups

There is one file that needs to be backed up:

* `/docker/grafana/data/lib/grafana.db` - SQLite database file

The database backup is done with a [systemd timer](https://www.freedesktop.org/software/systemd/man/systemd.timer.html) and then backed up with [Restic](https://github.com/status-im/infra-role-restic-backups):
```
vedran@dash-01.do-ams3.hq.metrics:/docker/grafana % s list-timers -a | grep grafana
Tue 2025-09-02 00:00:00 UTC 13h left      Mon 2025-09-01 00:00:02 UTC 10h ago      backup-grafana-db.timer      backup-grafana-db.service
```

# Restoring

## Backup Existing Data

Create a copy of Grafana folder:
```bash
cd /docker/grafana
docker compose down
cd ../
sudo cp -r grafana grafana_bkp
```

## Restore Backup

To restore a backup to the same directory the destination permissions need to be changed:
```bash
sudo chmod g+w -R /docker/grafana/data/lib
sudo -i -u restic restic restore --target=/ 23d58be4
```
By using `--target=/` we restore the backup to the same location from which it was copied.

### Test the restore

* Start the application `docker compose up -d`