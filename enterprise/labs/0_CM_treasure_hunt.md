# Services Enablement Boot Camp

## 0 - CM Monitoring Lab

### Questions & Answers

* What is ubertask optimization?

	> Search: ubertask optimization
	> A YARN configuration parameter
	> By default this is disabled
	> Whether to enable ubertask optimization, which runs "sufficiently small" jobs sequentially within a single JVM. "Small" is defined by the mapreduce.job.ubertask.maxmaps, mapreduce.job.ubertask.maxreduces, and mapreduce.job.ubertask.maxbytes settings.

* Where in CM is the Kerberos Security Realm value displayed?

	> Search: Kerberos Security Realm
	> Administration > Settings > Kerberos
	> Default: HADOOP.COM

* Which CDH service(s) host a property for enabling Kerberos authentication?

	> Search: Enable Kerberos Authentication
	> Zookeeper
	> YARN
	> HDFS

* How do you upgrade the CM agents?

	> Google Search External
	> Upgrade the Cloudera Manager Agent. You can use an upgrade wizard that is invoked when you connect to the Admin Console or manually install the Cloudera Manager Agent packages.

* Give the tsquery statement used to chart Hue's CPU utilization?

	> Search: Chart CPU Cores Used
	> Settings > Open in Chart Builder
	
```sql
select cpu_system_rate + cpu_user_rate 
where category=ROLE 
and serviceName=$SERVICENAME
-- $SERVICENAME = hue  $CLUSTERID = 1
```

* Name all the roles that make up the Hive service

	> Search: Hive Add Roles
	> HMS  Hive Metastore Server × 1
	> WHCS  WebHCat Server
	> HS2  HiveServer2 × 1
	> G  Gateway × 4

* What steps must be completed before integrating Cloudera Manager with Kerberos?

	> Google Search External
	> Prerequisites - These instructions assume you know how to install and configure Kerberos, you already have a working Kerberos key distribution center (KDC) and realm setup, and that you've installed the following Kerberos client packages on all cluster hosts and hosts that will be used to access the cluster, depending on the OS in use.

