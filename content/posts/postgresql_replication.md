---
title: "PostgreSQL Replication"
date: 2023-11-03T13:22:02+01:00
description: "Learning from replicating a PosrgreSQL database"
tags: ["database", "PostgreSQL", "migration"]
draft: false
---

# Concepts & Terms
## WAL
Write-Ahead Log (WAL) is a record of the changes made to the data. It ensures that when there is a crash in the system or loss of connection, the database can be recovered.

## WAL Level
WAL level determines how much information is written to the WAL, it corresponds to the `wal_level` option in the `postgresql.conf` file.
The default value is replica, which writes enough data to support WAL archiving and replication, including running read-only queries on a standby server. minimal removes all logging except the information required to recover from a crash or immediate shutdown. Finally, logical adds information necessary to support logical decoding. Each level includes the information logged at all lower levels. This parameter can only be set at server start.

## Replication Slots
A replication slot is a PostgreSQL feature that ensures the master server keeps the WAL logs required by replicas even when they are disconnected from the master.


# Steps
1. A dedicated user for replication is recommanded, let's assume it is called `replica`
```sql
CREATEUSER replica REPLICATION LOGIN CONNECTION LIMIT 1 ENCRYPTED PASSWORD 'YOUR_PASSWORD';
```

2. Add the user to `pg_hba.conf` file, change the IP, user name and database accordingly
```
host    replication     replica         123.123.123.123/32        md5
```

3. Change these settings in `postgresql.conf` after the application, I personally find the defaults are okay to go. After changing the file, a restart is neccessary.
```
listen_addresses = 'localhost,MasterIP'
wal_level = replica
wal_keep_segments = 64
max_wal_senders = 10
```

4. Update the `listen_addresses` on the replication server
```
listen_addresses = 'localhost,SecondaryIP'
```

5. The most important step, run `pg_basebackp` to sync the replication database. `-R` is important as it will generate the replication configration automatically for you.
```shell
pg_basebackp -D -h -X stream -c fast -U replica -W -R
```

It will create replication slot on the master database, which can be seen by running:
```sql
SELECT * FROM pg_replication_slots;
```

In case things didn't work out, run the following command to drop the slots:

```sql
SELECT pg_drop_replication_slot('SLOT_NAME');
```

6. Restart the replication database and it should be good to go :)

## Links
1. Amazing website that documents all the options of PostgreSQL: [https://postgresqlco.nf](https://postgresqlco.nf)
2. A tutorial I referred to a lot, but a bit messy in my humble opinion: [https://kinsta.com/blog/postgresql-replication/#how-to-set-up-postgresql-replication](https://kinsta.com/blog/postgresql-replication/#how-to-set-up-postgresql-replication)
3. The official documentation of PostgreSQL that should always be the book of truth: [https://www.postgresql.org/docs/12/runtime-config-replication.html](https://www.postgresql.org/docs/12/runtime-config-replication.html)