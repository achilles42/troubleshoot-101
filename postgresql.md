In this session will discuss about basic commands and debugging on postgresql database

## [Installation of postgresql](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-postgresql-on-ubuntu-14-04)

## Best Guide to understand postgresql intenals
    http://www.interdb.jp/pg/index.html

## Basics Commands

  Total Connnections:
  ```
    select count(*) as total_connections from pg_stat_activity;
  ```  

  Slave Count:
  ```
     select count(*) as slave_count from pg_stat_replication;
  ```
  
  Slave Lag:
  ```
     select now() - pg_last_xact_replay_timestamp() AS replication_delay;
  ```

  Dumping and Restore"

  ```
     Dumping: pg_dump -Fc -d database_name -U postgres > database_name.dump
     Restore: pg_restore  -F c -d database_name -U postgres /home/ubuntu/database_name.dump
  ```   

## Attaching External Disk on data directory of postgresql

     service postgresql status
     service postgresql stop
     lsblk
     ps aux | grep postgres
     vi /etc/postgresql/9.5/main/postgresql.conf
     ls -lah /var/lib/
     mv /var/lib/postgresql /var/lib/postgresql.original
     mkdir /var/lib/postgresql
     chmod 0755 /var/lib/postgresql
     chown postgres:postgres /var/lib/postgresql
     lsblk
     mkfs.ext4 /dev/xvdf
     mount -t ext4 /dev/xvdf /var/lib/postgresql
     df -kh
     rsync -lavzhd /var/lib/postgresql.original/* /var/lib/postgresql
     ls /var/lib/postgresql
     ls -asl /var/lib/postgresql
     df -kh
     service postgresql start

## After Adding external disk don't forget to add entry on fstab

    UUID="41cb2b22-716f-4c9f-b470-37c9d1003c78"  /var/lib/postgresql  ext4  defaults  0 0

## Resize external mounted disk
  * resize2fs /dev/xvdf

## READ ONLY USER CREATION
```
1. login to sql
2. connect to the desired db
3. `CREATE ROLE xxx WITH LOGIN PASSWORD 'password';`
4. `grant select on all tables in schema public to xxx;`
5. `grant select on all sequences in schema public to xxx;`
````

This only affects tables that have already been created. More powerfully, you can automatically have default roles assigned to new objects in future:

```
1. `alter default privileges in schema public
      grant select on tables to xxx;`
2. `alter default privileges in schema public
      grant select on sequences to xxx;`
```

## REPLICATION LAG COMMAND

Check slave on master:
```
  select client_addr, state, sent_location, write_location, flush_location, replay_location from pg_stat_replication;
```

Check Lag on slave:
```
  select now() - pg_last_xact_replay_timestamp() AS replication_delay;
```

```
  SELECT CASE WHEN pg_last_xlog_receive_location() = pg_last_xlog_replay_location()
            THEN 0
          ELSE EXTRACT (EPOCH FROM now() - pg_last_xact_replay_timestamp())
     END AS log_delay;
```

## Fix Broken Replication

```
http://www.rassoc.com/gregr/weblog/2013/02/16/zero-to-postgresql-streaming-replication-in-10-mins/
*** master IP_ADDRESS: 1.2.3.4
*** Postgres version: 9.2
*** Replication user: replicator
*** Replication password: thepassword
***
### echo Stopping PostgreSQL
sudo service postgresql stop

### echo Cleaning up old cluster directory
sudo -u postgres rm -rf /var/lib/postgresql/9.5/main

### echo Starting base backup as replicator
sudo -u postgres pg_basebackup -h 10.150.1.49 -D /var/lib/postgresql/9.5/main -U rep -v -P

### echo Writing recovery.conf file
sudo -u postgres bash -c "cat > /var/lib/postgresql/9.5/main/recovery.conf <<- _EOF1_
  standby_mode = 'on'
  primary_conninfo = 'host=11.140.2.23 port=5432 user=rep password=GV3UonBnhOHXTqsV sslmode=require'
  trigger_file = '/tmp/postgresql.trigger'
_EOF1_
"

### echo Startging PostgreSQL
sudo service postgresql start
```

## Run and datacopy command on screen

screen -d -m yourcommand


