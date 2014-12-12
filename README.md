## Elasticsearch Logstash Index Management

I folked [imperialwicket/elasticsearch-logstash-index-mgmt](http://github.com/imperialwicket/elasticsearch-logstash-index-mgmt), but added 2 functions which cause i cannot merge back to my upstream.

1. allow backup non-logstash index to backup
2. allow logstash index with different type, such as logstash-xxxyyy-YYYYMMDD

> for close or remove old index, i suggest use [Curator](http://github.com/elasticsearch/curator)

### elasticsearch-backup-index.sh

Backup handles making a backup and a restore script for a given index. The
default is yesterday's index, or you can pass a specific date for backup. You
can optionally keep the backup locally, and/or push it to s3. If you want to get
tricky, you can override the s3cmd command, and you could have this script push
the backups to a storage server on your network (or whatever you want, really).

### elasticsearch-restore-index.sh

Restore handles retrieving a backup file and restore script (from S3), and then
executing the restore script locally after download.


## Cron

Something like this might be helpful, assuming you placed the scripts in the
/opt/es/ directory (formatted for an /etc/cron.d/ file):

    00 7 * * * root /bin/bash /opt/es/elasticsearch-backup-index.sh -b "s3://es-bucket" -i "/opt/elasticsearch/data/elasticsearch/nodes/0/indices" -c "s3cmd put -c /path/to/.s3cfg"
    00 9 * * * root /bin/bash /opt/es/elasticsearch-remove-old-indices.sh -i 21



