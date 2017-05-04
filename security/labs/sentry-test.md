# Services Enablement Boot Camp

# 1 - Sentry Lab

## Install Sentry Service

### Change Privileges Hive Warehouse HDFS

Script

```sh
sudo -i
su - hdfs
hdfs dfs -chmod -R 771 /user/hive/warehouse
hdfs dfs -chown -R hive:hive /user/hive/warehouse
```

Output

```sh
[ec2-user@node1 ~]$ sudo -i
[root@node1 ~]# su - hdfs
Last login: Wed May  3 18:56:03 EDT 2017 on pts/0
[hdfs@node1 ~]$ hdfs dfs -chmod -R 771 /user/hive/warehouse
[hdfs@node1 ~]$ hdfs dfs -chown -R hive:hive /user/hive/warehouse

```

### Disable Impersionation

* Go to the Hive service.
* Click the Configuration tab.
* Select Scope > HiveServer2.
* Select Category > Main.
* Uncheck the HiveServer2 Enable Impersonation checkbox.
* Click Save Changes to commit the changes.

* Go to the MapReduce service.
* Click the Configuration tab.
* Select Scope > NodeManager.
* Select Category > Security.
* Set the Minimum User ID for Job Submission property to zero (the default is 1000).
* Click Save Changes to commit the changes.
* Repeat steps 1-6 for every TaskTracker role group for the MapReduce service that is associated with Hive.
* Restart the MapReduce service.

* Go to the YARN service.
* Click the Configuration tab.
* Select Scope > NodeManager.
* Select Category > Security.
* Ensure the Allowed System Users property includes the hive user. If not, add hive.
* Click Save Changes to commit the changes.
* Repeat steps 1-6 for every NodeManager role group for the YARN service that is associated with Hive.
* Restart the YARN service.

### Add Sentry Service

* Do this in Cloudera Manager > Add Service

### Enabling the Sentry Service for Hive

* Go to the Hive service.
* Click the Configuration tab.
* Select Scope > Hive (Service-Wide).
* Select Category > Main.
* Locate the Sentry Service property and select Sentry.
* Click Save Changes to commit the changes.
* Restart the Hive service.

### Enabling the Sentry Service for Hue

* Go to the Hue service.
* Click the Configuration tab.
* Select Scope > Hue (Service-Wide).
* Select Category > Main.
* Locate the Sentry Service property and select Sentry.
* Click Save Changes to commit the changes.
* Restart Hue.

### Add the Hive and Hue Groups to Sentry's Admin Groups

* Go to the Sentry service.
* Click the Configuration tab.
* Select Scope > Sentry (Service-Wide).
* Select Category > Main.
* Locate the Admin Groups property and add the hive, impala and hue groups to the list. 
	* If an end user is in one of these admin groups, that user has administrative privileges on the Sentry Server.
* Click Save Changes to commit the changes.

### Configure Sentry to recognize this account as an administrator

* Add your test user's primary group to the sentry.service.admin.group list in CM
* Restart Sentry, Hue, Hive, and Impala services

### Verify user privileges

Script

```sh
sudo -i
su - alanbluwol
kinit
beeline
!connect jdbc:hive2://node1.alanbluwol:10000/default;principal=hive/node1.alanbluwol@CLOUDERA.COM
```

Output

```sh
[root@node1 ~]# su - alanbluwol
Last login: Wed May  3 03:19:27 EDT 2017 on pts/1
[alanbluwol@node1 ~]$ kinit
Password for alanbluwol@CLOUDERA.COM: 
[alanbluwol@node1 ~]$ beeline
Beeline version 1.1.0-cdh5.9.2 by Apache Hive
beeline> !connect jdbc:hive2://node1.alanbluwol:10000/default;principal=hive/node1.alanbluwol@CLOUDERA.COM
scan complete in 2ms
Connecting to jdbc:hive2://node1.alanbluwol:10000/default;principal=hive/node1.alanbluwol@CLOUDERA.COM
Connected to: Apache Hive (version 1.1.0-cdh5.9.2)
Driver: Hive JDBC (version 1.1.0-cdh5.9.2)
Transaction isolation: TRANSACTION_REPEATABLE_READ
0: jdbc:hive2://node1.alanbluwol:10000/defaul> 
0: jdbc:hive2://node1.alanbluwol:10000/defaul> SHOW TABLES;
INFO  : Compiling command(queryId=hive_20170503201111_48b20724-ed8d-4396-b628-4d3f29d595ad): SHOW TABLES
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:[FieldSchema(name:tab_name, type:string, comment:from deserializer)], properties:null)
INFO  : Completed compiling command(queryId=hive_20170503201111_48b20724-ed8d-4396-b628-4d3f29d595ad); Time taken: 0.623 seconds
INFO  : Executing command(queryId=hive_20170503201111_48b20724-ed8d-4396-b628-4d3f29d595ad): SHOW TABLES
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503201111_48b20724-ed8d-4396-b628-4d3f29d595ad); Time taken: 0.242 seconds
INFO  : OK
+-----------+--+
| tab_name  |
+-----------+--+
+-----------+--+
No rows selected (2.217 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> 
```