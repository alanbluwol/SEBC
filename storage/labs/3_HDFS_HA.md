# Services Enablement Boot Camp

# Lab 3: High Availability

## Create Directory

Script

```sh
mkdir /data01/opt/cloudera/jn
chmod 777 /data01/opt/cloudera/jn
```

	> Add JournalNode Role to three hosts
	> Restart HDFS
	

## Add Journal Nodes

Done via Cloudera Manager

## Enable High Availability

Via Cloudera Manager

See screenshot PNG

## Post Configuration Steps

* The following manual steps must be performed after completing this wizard:
	* Configure the HDFS Web Interface Role of Hue service(s) Hue to be an HTTPFS role instead of a NameNode. Documentation
	* For each of the Hive service(s) Hive, stop the Hive service, back up the Hive Metastore Database to a persistent store, run the service command "Update Hive Metastore NameNodes", then restart the Hive services.

## Clean up HDFS Trash

Script

```sh
hadoop fs -rm -R /user/hdfs/.Trash/Current/*
hadoop fs -rm -R /user/hdfs/.Trash/170501220157/*
```