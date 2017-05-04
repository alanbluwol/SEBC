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


### Create a Sentry role with full authorization

Script

```sh
beeline
!connect jdbc:hive2://node1.alanbluwol:10000/default;principal=hive/node1.alanbluwol@CLOUDERA.COM

CREATE ROLE sentry_admin;
GRANT ALL ON SERVER server1 TO ROLE sentry_admin;
GRANT ROLE sentry_admin TO GROUP alanbluwol;
SHOW TABLES;
```

Output

```sh
0: jdbc:hive2://node1.alanbluwol:10000/defaul> CREATE ROLE sentry_admin;
INFO  : Compiling command(queryId=hive_20170503201616_e28e0fa9-4230-4d70-970d-2e5d288c232d): CREATE ROLE sentry_admin
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:null, properties:null)
INFO  : Completed compiling command(queryId=hive_20170503201616_e28e0fa9-4230-4d70-970d-2e5d288c232d); Time taken: 0.079 seconds
INFO  : Executing command(queryId=hive_20170503201616_e28e0fa9-4230-4d70-970d-2e5d288c232d): CREATE ROLE sentry_admin
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503201616_e28e0fa9-4230-4d70-970d-2e5d288c232d); Time taken: 0.687 seconds
INFO  : OK
No rows affected (0.782 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> GRANT ALL ON SERVER server1 TO ROLE sentry_admin;
INFO  : Compiling command(queryId=hive_20170503201717_941be416-c340-40dd-9cde-b65a14de85c6): GRANT ALL ON SERVER server1 TO ROLE sentry_admin
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:null, properties:null)
INFO  : Completed compiling command(queryId=hive_20170503201717_941be416-c340-40dd-9cde-b65a14de85c6); Time taken: 0.088 seconds
INFO  : Executing command(queryId=hive_20170503201717_941be416-c340-40dd-9cde-b65a14de85c6): GRANT ALL ON SERVER server1 TO ROLE sentry_admin
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503201717_941be416-c340-40dd-9cde-b65a14de85c6); Time taken: 0.077 seconds
INFO  : OK
No rows affected (0.176 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> GRANT ROLE sentry_admin TO GROUP alanbluwol;
INFO  : Compiling command(queryId=hive_20170503201717_32b18526-c2a0-45a1-8c93-45adb78e286a): GRANT ROLE sentry_admin TO GROUP alanbluwol
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:null, properties:null)
INFO  : Completed compiling command(queryId=hive_20170503201717_32b18526-c2a0-45a1-8c93-45adb78e286a); Time taken: 0.057 seconds
INFO  : Executing command(queryId=hive_20170503201717_32b18526-c2a0-45a1-8c93-45adb78e286a): GRANT ROLE sentry_admin TO GROUP alanbluwol
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503201717_32b18526-c2a0-45a1-8c93-45adb78e286a); Time taken: 0.084 seconds
INFO  : OK
No rows affected (0.153 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> SHOW TABLES;
INFO  : Compiling command(queryId=hive_20170503201717_fa29bcfb-c4ef-4a71-9818-624b0236a9fe): SHOW TABLES
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:[FieldSchema(name:tab_name, type:string, comment:from deserializer)], properties:null)
INFO  : Completed compiling command(queryId=hive_20170503201717_fa29bcfb-c4ef-4a71-9818-624b0236a9fe); Time taken: 0.059 seconds
INFO  : Executing command(queryId=hive_20170503201717_fa29bcfb-c4ef-4a71-9818-624b0236a9fe): SHOW TABLES
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503201717_fa29bcfb-c4ef-4a71-9818-624b0236a9fe); Time taken: 0.121 seconds
INFO  : OK
+-------------+--+
|  tab_name   |
+-------------+--+
| alanbluwol  |
+-------------+--+
1 row selected (0.233 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> 
```

### Create additional test users

* Add new users to all cluster nodes

Script - On every node

```sh
sudo -i
groupadd selector
groupadd inserters
useradd -u 1100 -g selector george
useradd -u 1200 -g inserters ferdinand

kadmin.local
add_principal george
add_principal ferdinand
```

Output

```sh
[ec2-user@manager ~]$ sudo -i
[root@manager ~]# groupadd selector
[root@manager ~]# groupadd inserters
[root@manager ~]# useradd -u 1100 -g selector george
[root@manager ~]# useradd -u 1200 -g inserters ferdinand
[root@manager ~]# kadmin.local
Authenticating as principal root/admin@CLOUDERA.COM with password.
kadmin.local:  add_principal george
WARNING: no policy specified for george@CLOUDERA.COM; defaulting to no policy
Enter password for principal "george@CLOUDERA.COM": 
Re-enter password for principal "george@CLOUDERA.COM": 
Principal "george@CLOUDERA.COM" created.
kadmin.local:  add_principal ferdinand
WARNING: no policy specified for ferdinand@CLOUDERA.COM; defaulting to no policy
Enter password for principal "ferdinand@CLOUDERA.COM": 
Re-enter password for principal "ferdinand@CLOUDERA.COM": 
Principal "ferdinand@CLOUDERA.COM" created.
kadmin.local:  exit
```

### Create test roles

Script

* Connect
* Create test roles
* Grant read privilege for all tables to reads
* Grant read privilege for default.sample07 only to 'writes':

```sh
su - alanbluwol
beeline
!connect jdbc:hive2://node1.alanbluwol:10000/default;principal=hive/node1.alanbluwol@CLOUDERA.COM

CREATE ROLE reads;
CREATE ROLE writes;

GRANT SELECT ON DATABASE default TO ROLE reads;
GRANT ROLE reads TO GROUP selector;

REVOKE ALL ON DATABASE default FROM ROLE writes;
GRANT SELECT ON default.sample_07 TO ROLE writes;
GRANT ROLE writes TO GROUP inserters;
```

Output

```sh
[root@node1 ~]# su - alanbluwol
Last login: Wed May  3 20:10:47 EDT 2017 on pts/1
[alanbluwol@node1 ~]$ beeline
Beeline version 1.1.0-cdh5.9.2 by Apache Hive
beeline> !connect jdbc:hive2://node1.alanbluwol:10000/default;principal=hive/node1.alanbluwol@CLOUDERA.COM
scan complete in 1ms
Connecting to jdbc:hive2://node1.alanbluwol:10000/default;principal=hive/node1.alanbluwol@CLOUDERA.COM
Connected to: Apache Hive (version 1.1.0-cdh5.9.2)
Driver: Hive JDBC (version 1.1.0-cdh5.9.2)
Transaction isolation: TRANSACTION_REPEATABLE_READ
0: jdbc:hive2://node1.alanbluwol:10000/defaul> CREATE ROLE reads;
INFO  : Compiling command(queryId=hive_20170503202424_335d1b22-d383-4078-8cb0-a6bc9d8d3427): CREATE ROLE reads
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:null, properties:null)
INFO  : Completed compiling command(queryId=hive_20170503202424_335d1b22-d383-4078-8cb0-a6bc9d8d3427); Time taken: 0.062 seconds
INFO  : Executing command(queryId=hive_20170503202424_335d1b22-d383-4078-8cb0-a6bc9d8d3427): CREATE ROLE reads
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503202424_335d1b22-d383-4078-8cb0-a6bc9d8d3427); Time taken: 0.038 seconds
INFO  : OK
No rows affected (0.155 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> CREATE ROLE writes;
INFO  : Compiling command(queryId=hive_20170503202424_9d53c2e4-aa97-45df-b487-db3b0d0bdd52): CREATE ROLE writes
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:null, properties:null)
INFO  : Completed compiling command(queryId=hive_20170503202424_9d53c2e4-aa97-45df-b487-db3b0d0bdd52); Time taken: 0.054 seconds
INFO  : Executing command(queryId=hive_20170503202424_9d53c2e4-aa97-45df-b487-db3b0d0bdd52): CREATE ROLE writes
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503202424_9d53c2e4-aa97-45df-b487-db3b0d0bdd52); Time taken: 0.022 seconds
INFO  : OK
No rows affected (0.087 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> GRANT SELECT ON DATABASE default TO ROLE reads;
INFO  : Compiling command(queryId=hive_20170503202424_6d57c3bd-2b4b-4583-a7b4-fd566149766e): GRANT SELECT ON DATABASE default TO ROLE reads
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:null, properties:null)
INFO  : Completed compiling command(queryId=hive_20170503202424_6d57c3bd-2b4b-4583-a7b4-fd566149766e); Time taken: 0.062 seconds
INFO  : Executing command(queryId=hive_20170503202424_6d57c3bd-2b4b-4583-a7b4-fd566149766e): GRANT SELECT ON DATABASE default TO ROLE reads
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503202424_6d57c3bd-2b4b-4583-a7b4-fd566149766e); Time taken: 0.038 seconds
INFO  : OK
No rows affected (0.112 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> GRANT ROLE reads TO GROUP selector;
INFO  : Compiling command(queryId=hive_20170503202424_c6585f2e-9cae-436c-aed7-9054cdf71e67): GRANT ROLE reads TO GROUP selector
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:null, properties:null)
INFO  : Completed compiling command(queryId=hive_20170503202424_c6585f2e-9cae-436c-aed7-9054cdf71e67); Time taken: 0.074 seconds
INFO  : Executing command(queryId=hive_20170503202424_c6585f2e-9cae-436c-aed7-9054cdf71e67): GRANT ROLE reads TO GROUP selector
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503202424_c6585f2e-9cae-436c-aed7-9054cdf71e67); Time taken: 0.025 seconds
INFO  : OK
No rows affected (0.109 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> REVOKE ALL ON DATABASE default FROM ROLE writes;
INFO  : Compiling command(queryId=hive_20170503202626_65a56b6f-ee16-4f9f-9d1c-362bb0cb5de3): REVOKE ALL ON DATABASE default FROM ROLE writes
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:null, properties:null)
INFO  : Completed compiling command(queryId=hive_20170503202626_65a56b6f-ee16-4f9f-9d1c-362bb0cb5de3); Time taken: 0.055 seconds
INFO  : Executing command(queryId=hive_20170503202626_65a56b6f-ee16-4f9f-9d1c-362bb0cb5de3): REVOKE ALL ON DATABASE default FROM ROLE writes
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503202626_65a56b6f-ee16-4f9f-9d1c-362bb0cb5de3); Time taken: 0.081 seconds
INFO  : OK
No rows affected (0.147 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> GRANT SELECT ON default.sample_07 TO ROLE writes;
INFO  : Compiling command(queryId=hive_20170503202626_61820299-1717-4f63-9047-8449dc3b6d7c): GRANT SELECT ON default.sample_07 TO ROLE writes
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:null, properties:null)
INFO  : Completed compiling command(queryId=hive_20170503202626_61820299-1717-4f63-9047-8449dc3b6d7c); Time taken: 0.054 seconds
INFO  : Executing command(queryId=hive_20170503202626_61820299-1717-4f63-9047-8449dc3b6d7c): GRANT SELECT ON default.sample_07 TO ROLE writes
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503202626_61820299-1717-4f63-9047-8449dc3b6d7c); Time taken: 0.033 seconds
INFO  : OK
No rows affected (0.099 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> GRANT ROLE writes TO GROUP inserters;
INFO  : Compiling command(queryId=hive_20170503202626_8985705b-5aa8-490f-a54f-27277a9796f4): GRANT ROLE writes TO GROUP inserters
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:null, properties:null)
INFO  : Completed compiling command(queryId=hive_20170503202626_8985705b-5aa8-490f-a54f-27277a9796f4); Time taken: 0.053 seconds
INFO  : Executing command(queryId=hive_20170503202626_8985705b-5aa8-490f-a54f-27277a9796f4): GRANT ROLE writes TO GROUP inserters
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503202626_8985705b-5aa8-490f-a54f-27277a9796f4); Time taken: 0.024 seconds
INFO  : OK
No rows affected (0.087 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> 
```

### Test Users

Script

* George can read tables
* Ferdinand cannot read tables

```sh
exit
su - george
kinit
klist
beeline
!connect jdbc:hive2://node1.alanbluwol:10000/default;principal=hive/node1.alanbluwol@CLOUDERA.COM
SHOW TABLES;

exit
su - ferdinand
kinit
klist
beeline
!connect jdbc:hive2://node1.alanbluwol:10000/default;principal=hive/node1.alanbluwol@CLOUDERA.COM
SHOW TABLES;
```

Output

```sh
[alanbluwol@node1 ~]$ exit
logout
[root@node1 ~]# su - george
[george@node1 ~]$ kinit
Password for george@CLOUDERA.COM: 
[george@node1 ~]$ klist
Ticket cache: FILE:/tmp/krb5cc_1100
Default principal: george@CLOUDERA.COM

Valid starting       Expires              Service principal
05/03/2017 20:28:16  05/04/2017 20:28:16  krbtgt/CLOUDERA.COM@CLOUDERA.COM
	renew until 05/10/2017 20:28:16
[george@node1 ~]$ beeline
Beeline version 1.1.0-cdh5.9.2 by Apache Hive
beeline> !connect jdbc:hive2://node1.alanbluwol:10000/default;principal=hive/node1.alanbluwol@CLOUDERA.COM
scan complete in 1ms
Connecting to jdbc:hive2://node1.alanbluwol:10000/default;principal=hive/node1.alanbluwol@CLOUDERA.COM
Connected to: Apache Hive (version 1.1.0-cdh5.9.2)
Driver: Hive JDBC (version 1.1.0-cdh5.9.2)
Transaction isolation: TRANSACTION_REPEATABLE_READ
0: jdbc:hive2://node1.alanbluwol:10000/defaul> SHOW TABLES;
INFO  : Compiling command(queryId=hive_20170503202828_9261f6d7-d9b2-4ffe-a084-e6326d512f4f): SHOW TABLES
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:[FieldSchema(name:tab_name, type:string, comment:from deserializer)], properties:null)
INFO  : Completed compiling command(queryId=hive_20170503202828_9261f6d7-d9b2-4ffe-a084-e6326d512f4f); Time taken: 0.06 seconds
INFO  : Executing command(queryId=hive_20170503202828_9261f6d7-d9b2-4ffe-a084-e6326d512f4f): SHOW TABLES
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503202828_9261f6d7-d9b2-4ffe-a084-e6326d512f4f); Time taken: 0.121 seconds
INFO  : OK
+-------------+--+
|  tab_name   |
+-------------+--+
| alanbluwol  |
+-------------+--+
1 row selected (0.271 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> 


[george@node1 ~]$ exit
logout
[root@node1 ~]# su - ferdinand
[ferdinand@node1 ~]$ kinit
Password for ferdinand@CLOUDERA.COM: 
[ferdinand@node1 ~]$ klist
Ticket cache: FILE:/tmp/krb5cc_1200
Default principal: ferdinand@CLOUDERA.COM

Valid starting       Expires              Service principal
05/03/2017 20:29:53  05/04/2017 20:29:53  krbtgt/CLOUDERA.COM@CLOUDERA.COM
	renew until 05/10/2017 20:29:53
[ferdinand@node1 ~]$ beeline
Beeline version 1.1.0-cdh5.9.2 by Apache Hive
beeline> !connect jdbc:hive2://node1.alanbluwol:10000/default;principal=hive/node1.alanbluwol@CLOUDERA.COM
scan complete in 2ms
Connecting to jdbc:hive2://node1.alanbluwol:10000/default;principal=hive/node1.alanbluwol@CLOUDERA.COM
Connected to: Apache Hive (version 1.1.0-cdh5.9.2)
Driver: Hive JDBC (version 1.1.0-cdh5.9.2)
Transaction isolation: TRANSACTION_REPEATABLE_READ
0: jdbc:hive2://node1.alanbluwol:10000/defaul> SHOW TABLES;
INFO  : Compiling command(queryId=hive_20170503203030_9263758d-7c8c-40fd-ade2-6eceb056a5b6): SHOW TABLES
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:[FieldSchema(name:tab_name, type:string, comment:from deserializer)], properties:null)
INFO  : Completed compiling command(queryId=hive_20170503203030_9263758d-7c8c-40fd-ade2-6eceb056a5b6); Time taken: 0.061 seconds
INFO  : Executing command(queryId=hive_20170503203030_9263758d-7c8c-40fd-ade2-6eceb056a5b6): SHOW TABLES
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503203030_9263758d-7c8c-40fd-ade2-6eceb056a5b6); Time taken: 0.124 seconds
INFO  : OK
+-----------+--+
| tab_name  |
+-----------+--+
+-----------+--+
No rows selected (0.269 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> 
```
