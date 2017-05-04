# Services Enablement Boot Camp

# 2 - CM Install

* Yum Repo List
* Yum install clouder manager 10
* Run SCM Prepare Database Script
* Output db.properties

```sh
[root@ip-172-31-2-184 ~]# ls /etc/yum.repos.d
cloudera-manager.repo  redhat.repo  redhat-rhui-client-config.repo  redhat-rhui.repo  rhui-load-balancers.conf

[root@ip-172-31-2-184 yum.repos.d]# /usr/share/cmf/schema/scm_prepare_database.sh -h manager.alanbluwol mysql scm cloudera cloudera
JAVA_HOME=/usr/java/jdk1.7.0_67-cloudera
Verifying that we can write to /etc/cloudera-scm-server
Creating SCM configuration file in /etc/cloudera-scm-server
Executing:  /usr/java/jdk1.7.0_67-cloudera/bin/java -cp /usr/share/java/mysql-connector-java.jar:/usr/share/java/oracle-connector-java.jar:/usr/share/cmf/schema/../lib/* com.cloudera.enterprise.dbutil.DbCommandExecutor /etc/cloudera-scm-server/db.properties com.cloudera.cmf.db.
[                          main] DbCommandExecutor              INFO  Successfully connected to database.
All done, your SCM database is configured correctly!

```

Supporting Scripts

```sh
[root@ip-172-31-2-184 yum.repos.d]# yum install cloudera-manager-daemons cloudera-manager-server
Loaded plugins: amazon-id, rhui-lb, search-disabled-repos
cloudera-manager                                                                                                                                            |  951 B  00:00:00     
rhui-REGION-client-config-server-7                                                                                                                          | 2.9 kB  00:00:00     
rhui-REGION-rhel-server-releases                                                                                                                            | 3.5 kB  00:00:00     
rhui-REGION-rhel-server-rh-common                                                                                                                           | 3.8 kB  00:00:00     
(1/7): rhui-REGION-client-config-server-7/x86_64/primary_db                                                                                                 | 5.4 kB  00:00:00     
(2/7): rhui-REGION-rhel-server-releases/7Server/x86_64/updateinfo                                                                                           | 1.9 MB  00:00:00     
(3/7): rhui-REGION-rhel-server-rh-common/7Server/x86_64/group                                                                                               |  104 B  00:00:00     
(4/7): rhui-REGION-rhel-server-releases/7Server/x86_64/group                                                                                                | 701 kB  00:00:00     
(5/7): rhui-REGION-rhel-server-rh-common/7Server/x86_64/primary_db                                                                                          | 118 kB  00:00:00     
(6/7): rhui-REGION-rhel-server-rh-common/7Server/x86_64/updateinfo                                                                                          |  33 kB  00:00:00     
(7/7): rhui-REGION-rhel-server-releases/7Server/x86_64/primary_db                                                                                           |  35 MB  00:00:00     
cloudera-manager/primary                                                                                                                                    | 4.3 kB  00:00:00     
cloudera-manager                                                                                                                                                               7/7
Resolving Dependencies
--> Running transaction check
---> Package cloudera-manager-daemons.x86_64 0:5.11.0-1.cm5110.p0.101.el7 will be installed
---> Package cloudera-manager-server.x86_64 0:5.11.0-1.cm5110.p0.101.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

===================================================================================================================================================================================
 Package                                          Arch                           Version                                            Repository                                Size
===================================================================================================================================================================================
Installing:
 cloudera-manager-daemons                         x86_64                         5.11.0-1.cm5110.p0.101.el7                         cloudera-manager                         610 M
 cloudera-manager-server                          x86_64                         5.11.0-1.cm5110.p0.101.el7                         cloudera-manager                         8.5 k

Transaction Summary
===================================================================================================================================================================================
Install  2 Packages

Total download size: 610 M
Installed size: 785 M
Is this ok [y/d/N]: y
Downloading packages:
(1/2): cloudera-manager-server-5.11.0-1.cm5110.p0.101.el7.x86_64.rpm                                                                                        | 8.5 kB  00:00:00     
(2/2): cloudera-manager-daemons-5.11.0-1.cm5110.p0.101.el7.x86_64.rpm                                                                                       | 610 MB  00:00:09     
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                               67 MB/s | 610 MB  00:00:09     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : cloudera-manager-daemons-5.11.0-1.cm5110.p0.101.el7.x86_64                                                                                                      1/2 
  Installing : cloudera-manager-server-5.11.0-1.cm5110.p0.101.el7.x86_64                                                                                                       2/2 
  Verifying  : cloudera-manager-server-5.11.0-1.cm5110.p0.101.el7.x86_64                                                                                                       1/2 
  Verifying  : cloudera-manager-daemons-5.11.0-1.cm5110.p0.101.el7.x86_64                                                                                                      2/2 

Installed:
  cloudera-manager-daemons.x86_64 0:5.11.0-1.cm5110.p0.101.el7                             cloudera-manager-server.x86_64 0:5.11.0-1.cm5110.p0.101.el7                            

Complete!
```
