.. title: Restoring a Database in Dockered InfluxDB
.. slug: restoring-a-database-in-dockered-influxdb
.. date: 2017-10-17 13:14:13 UTC+08:00
.. tags: draft,private
.. category: 
.. link: 
.. description: 
.. type: text

Started the container
docker run -v influxdb-data:/var/lib/influxdb -v $PWD/panoptez-influx:/tmp/backup -p 8086:8086 influxdb

Created an initial database `brian`

  $ influx
  Visit https://enterprise.influxdata.com to register for updates, InfluxDB server management, and monitoring.
  Connected to http://localhost:8086 version 1.3.5
  InfluxDB shell 0.10.0
  > create database brian
  > show databases
  name: databases
  ---------------
  name
  _internal
  brian

Stopped the container with ^C

Attempt #1: Restore backed-up `panoptez` db to `trumid` db:

$ docker run -v influxdb-data:/var/lib/influxdb -v $PWD/panoptez-influx:/tmp/backup -p 8086:8086 influxdb influxd restore -datadir /var/lib/influxdb/data  -database trumid /tmp/backup
Restoring from backup /tmp/backup/trumid.*
restore: no backup files for /tmp/backup/trumid.* in /tmp/backup

Attempt #2: Restore backed-up `panoptez` 

$ docker run -v influxdb-data:/var/lib/influxdb -v $PWD/panoptez-influx:/tmp/backup -p 8086:8086 influxdb influxd restore -datadir /var/lib/influxdb/data  -database /tmp/backup
restore: path with backup files required

Database has to be restored in two steps, first the metastore and 2nd the database
$ docker run --name restored-influx -v influxdb-data:/var/lib/influxdb -v $PWD/panoptez-influx:/tmp/backup -p 8086:8086 influxdb influxd restore -metadir /var/lib/influxdb/meta /tmp/backup
docker run -v influxdb-data:/var/lib/influxdb -v $PWD/panoptez-influx:/tmp/backup -p 8086:8086 influxdb influxd restore -metadir /var/lib/influxdb/meta /tmp/backup
Using metastore snapshot: /tmp/backup/meta.00

$ docker run -v influxdb-data:/var/lib/influxdb -v $PWD/panoptez-influx:/tmp/backup -p 8086:8086 influxdb influxd restore -datadir /var/lib/influxdb/data -database panoptez /tmp/backup


Trumid on its standalone instance was set up to use a `panoptez` InfluxDB database. Currently, FundKo in k8s is also using a `panoptez` database in the production namespace. 

My initial plan was to back up the Trumid `panoptez` database and restore it to a new database named `trumid` on the production k8s namespace. Unfortunately neither restoring a backup to a new name, nor renaming a database are supported in InfluxDB. There are a bunch of GitHub issues and discussions around these issues already:

1. [The ability to restore an InfluxDB backup to a different database name](https://github.com/influxdata/influxdb/issues/8519)
2. [How to copy or clone a database on the same server with different name?](https://groups.google.com/forum/#!topic/influxdb/UYMr8FIOAs8)
3. [InfluxDB can't rename database](https://agrrh.com/2017/influxdb-cant-rename-database)

         There is a [workaround](https://community.influxdata.com/t/workaround-to-rename-databases/1060) which I will test on my local machine first before attempting on k8s. 
