# Services Enablement Boot Camp

# 3 - Cloudera Cluster

* Create directories
* List directories
* API Show Hosts
* API Cluster Services
* See PNG Hive Sample Tables

```sh
[root@ip-172-31-5-118 ~]# su - hdfs
[hdfs@node1 ~]$ hdfs dfs -mkdir /user/cate
[hdfs@node1 ~]$ hdfs dfs -mkdir /user/jemaine
[hdfs@node1 ~]$ hdfs dfs -chown cate:aussies /user/cate
[hdfs@node1 ~]$ hdfs dfs -chown jemaine:kiwis /user/jemaine
[hdfs@node1 ~]$ hdfs dfs -ls /user/
Found 7 items
drwxr-xr-x   - cate    aussies          0 2017-05-04 20:09 /user/cate
drwxrwxrwx   - mapred  hadoop           0 2017-05-04 19:58 /user/history
drwxrwxr-t   - hive    hive             0 2017-05-04 19:58 /user/hive
drwxrwxr-x   - hue     hue              0 2017-05-04 19:59 /user/hue
drwxrwxr-x   - impala  impala           0 2017-05-04 19:59 /user/impala
drwxr-xr-x   - jemaine kiwis            0 2017-05-04 20:09 /user/jemaine
drwxrwxr-x   - oozie   oozie            0 2017-05-04 19:59 /user/oozie


[hdfs@node1 ~]$ curl -X GET -u "admin:admin" -i http://13.55.126.110:7180/api/v14/hosts
HTTP/1.1 200 OK
Expires: Thu, 01-Jan-1970 00:00:00 GMT
Set-Cookie: CLOUDERA_MANAGER_SESSIONID=1eaqx8gpsw34izkslaju96bec;Path=/;HttpOnly
Content-Type: application/json
Date: Fri, 05 May 2017 00:11:35 GMT
Transfer-Encoding: chunked
Server: Jetty(6.1.26.cloudera.4)

{
  "items" : [ {
    "hostId" : "90192eba-6d58-451f-8065-df69e38720ab",
    "ipAddress" : "172.31.5.118",
    "hostname" : "node1.alanbluwol",
    "rackId" : "/default",
    "hostUrl" : "http://manager.alanbluwol:7180/cmf/hostRedirect/90192eba-6d58-451f-8065-df69e38720ab",
    "maintenanceMode" : false,
    "maintenanceOwners" : [ ],
    "commissionState" : "COMMISSIONED",
    "numCores" : 4,
    "numPhysicalCores" : 2,
    "totalPhysMemBytes" : 16389316608
  }, {
    "hostId" : "ee0a1454-51aa-4d45-8936-cc7f1c89c737",
    "ipAddress" : "172.31.9.102",
    "hostname" : "node2.alanbluwol",
    "rackId" : "/default",
    "hostUrl" : "http://manager.alanbluwol:7180/cmf/hostRedirect/ee0a1454-51aa-4d45-8936-cc7f1c89c737",
    "maintenanceMode" : false,
    "maintenanceOwners" : [ ],
    "commissionState" : "COMMISSIONED",
    "numCores" : 4,
    "numPhysicalCores" : 2,
    "totalPhysMemBytes" : 16389316608
  }, {
    "hostId" : "2fe75fbd-3c8f-4e94-90db-a4936b4d3e76",
    "ipAddress" : "172.31.15.9",
    "hostname" : "node3.alanbluwol",
    "rackId" : "/default",
    "hostUrl" : "http://manager.alanbluwol:7180/cmf/hostRedirect/2fe75fbd-3c8f-4e94-90db-a4936b4d3e76",
    "maintenanceMode" : false,
    "maintenanceOwners" : [ ],
    "commissionState" : "COMMISSIONED",
    "numCores" : 4,
    "numPhysicalCores" : 2,
    "totalPhysMemBytes" : 16389316608
  }, {
    "hostId" : "87c7ced0-54fc-4665-b0b8-5a25f4ff51e5",
    "ipAddress" : "172.31.15.19",
    "hostname" : "node4.alanbluwol",
    "rackId" : "/default",
    "hostUrl" : "http://manager.alanbluwol:7180/cmf/hostRedirect/87c7ced0-54fc-4665-b0b8-5a25f4ff51e5",
    "maintenanceMode" : false,
    "maintenanceOwners" : [ ],
    "commissionState" : "COMMISSIONED",
    "numCores" : 4,
    "numPhysicalCores" : 2,
    "totalPhysMemBytes" : 16389316608
  } ]
}


[hdfs@node1 ~]$ curl -X GET -u "admin:admin" -i http://13.55.126.110:7180/api/v6/clusters/alanbluwol/services
HTTP/1.1 200 OK
Expires: Thu, 01-Jan-1970 00:00:00 GMT
Set-Cookie: CLOUDERA_MANAGER_SESSIONID=c3hy5ah1fey7imtchwlwzacw;Path=/;HttpOnly
Content-Type: application/json
Date: Fri, 05 May 2017 00:12:35 GMT
Transfer-Encoding: chunked
Server: Jetty(6.1.26.cloudera.4)

{
  "items" : [ {
    "name" : "hive",
    "type" : "HIVE",
    "clusterRef" : {
      "clusterName" : "cluster"
    },
    "serviceUrl" : "http://manager.alanbluwol:7180/cmf/serviceRedirect/hive",
    "serviceState" : "STARTED",
    "healthSummary" : "GOOD",
    "healthChecks" : [ {
      "name" : "HIVE_HIVEMETASTORES_HEALTHY",
      "summary" : "GOOD"
    }, {
      "name" : "HIVE_HIVESERVER2S_HEALTHY",
      "summary" : "GOOD"
    } ],
    "configStalenessStatus" : "FRESH",
    "clientConfigStalenessStatus" : "FRESH",
    "maintenanceMode" : false,
    "maintenanceOwners" : [ ],
    "displayName" : "Hive"
  }, {
    "name" : "zookeeper",
    "type" : "ZOOKEEPER",
    "clusterRef" : {
      "clusterName" : "cluster"
    },
    "serviceUrl" : "http://manager.alanbluwol:7180/cmf/serviceRedirect/zookeeper",
    "serviceState" : "STARTED",
    "healthSummary" : "GOOD",
    "healthChecks" : [ {
      "name" : "ZOOKEEPER_CANARY_HEALTH",
      "summary" : "GOOD"
    }, {
      "name" : "ZOOKEEPER_SERVERS_HEALTHY",
      "summary" : "GOOD"
    } ],
    "configStalenessStatus" : "FRESH",
    "clientConfigStalenessStatus" : "FRESH",
    "maintenanceMode" : false,
    "maintenanceOwners" : [ ],
    "displayName" : "ZooKeeper"
  }, {
    "name" : "hue",
    "type" : "HUE",
    "clusterRef" : {
      "clusterName" : "cluster"
    },
    "serviceUrl" : "http://manager.alanbluwol:7180/cmf/serviceRedirect/hue",
    "serviceState" : "STARTED",
    "healthSummary" : "GOOD",
    "healthChecks" : [ {
      "name" : "HUE_HUE_SERVERS_HEALTHY",
      "summary" : "GOOD"
    } ],
    "configStalenessStatus" : "FRESH",
    "clientConfigStalenessStatus" : "FRESH",
    "maintenanceMode" : false,
    "maintenanceOwners" : [ ],
    "displayName" : "Hue"
  }, {
    "name" : "oozie",
    "type" : "OOZIE",
    "clusterRef" : {
      "clusterName" : "cluster"
    },
    "serviceUrl" : "http://manager.alanbluwol:7180/cmf/serviceRedirect/oozie",
    "serviceState" : "STARTED",
    "healthSummary" : "GOOD",
    "healthChecks" : [ {
      "name" : "OOZIE_OOZIE_SERVERS_HEALTHY",
      "summary" : "GOOD"
    } ],
    "configStalenessStatus" : "FRESH",
    "clientConfigStalenessStatus" : "FRESH",
    "maintenanceMode" : false,
    "maintenanceOwners" : [ ],
    "displayName" : "Oozie"
  }, {
    "name" : "impala",
    "type" : "IMPALA",
    "clusterRef" : {
      "clusterName" : "cluster"
    },
    "serviceUrl" : "http://manager.alanbluwol:7180/cmf/serviceRedirect/impala",
    "serviceState" : "STARTED",
    "healthSummary" : "GOOD",
    "healthChecks" : [ {
      "name" : "IMPALA_ASSIGNMENT_LOCALITY",
      "summary" : "DISABLED"
    }, {
      "name" : "IMPALA_CATALOGSERVER_HEALTH",
      "summary" : "GOOD"
    }, {
      "name" : "IMPALA_IMPALADS_HEALTHY",
      "summary" : "GOOD"
    }, {
      "name" : "IMPALA_STATESTORE_HEALTH",
      "summary" : "GOOD"
    } ],
    "configStalenessStatus" : "FRESH",
    "clientConfigStalenessStatus" : "FRESH",
    "maintenanceMode" : false,
    "maintenanceOwners" : [ ],
    "displayName" : "Impala"
  }, {
    "name" : "yarn",
    "type" : "YARN",
    "clusterRef" : {
      "clusterName" : "cluster"
    },
    "serviceUrl" : "http://manager.alanbluwol:7180/cmf/serviceRedirect/yarn",
    "serviceState" : "STARTED",
    "healthSummary" : "GOOD",
    "healthChecks" : [ {
      "name" : "YARN_JOBHISTORY_HEALTH",
      "summary" : "GOOD"
    }, {
      "name" : "YARN_NODE_MANAGERS_HEALTHY",
      "summary" : "GOOD"
    }, {
      "name" : "YARN_RESOURCEMANAGERS_HEALTH",
      "summary" : "GOOD"
    }, {
      "name" : "YARN_USAGE_AGGREGATION_HEALTH",
      "summary" : "DISABLED"
    } ],
    "configStalenessStatus" : "FRESH",
    "clientConfigStalenessStatus" : "FRESH",
    "maintenanceMode" : false,
    "maintenanceOwners" : [ ],
    "displayName" : "YARN (MR2 Included)"
  }, {
    "name" : "hdfs",
    "type" : "HDFS",
    "clusterRef" : {
      "clusterName" : "cluster"
    },
    "serviceUrl" : "http://manager.alanbluwol:7180/cmf/serviceRedirect/hdfs",
    "serviceState" : "STARTED",
    "healthSummary" : "GOOD",
    "healthChecks" : [ {
      "name" : "HDFS_BLOCKS_WITH_CORRUPT_REPLICAS",
      "summary" : "GOOD"
    }, {
      "name" : "HDFS_CANARY_HEALTH",
      "summary" : "GOOD"
    }, {
      "name" : "HDFS_DATA_NODES_HEALTHY",
      "summary" : "GOOD"
    }, {
      "name" : "HDFS_FREE_SPACE_REMAINING",
      "summary" : "GOOD"
    }, {
      "name" : "HDFS_HA_NAMENODE_HEALTH",
      "summary" : "GOOD"
    }, {
      "name" : "HDFS_MISSING_BLOCKS",
      "summary" : "GOOD"
    }, {
      "name" : "HDFS_UNDER_REPLICATED_BLOCKS",
      "summary" : "GOOD"
    } ],
    "configStalenessStatus" : "FRESH",
    "clientConfigStalenessStatus" : "FRESH",
    "maintenanceMode" : false,
    "maintenanceOwners" : [ ],
    "displayName" : "HDFS"
  } ]
}

```

