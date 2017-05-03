# Services Enablement Boot Camp

## 3 - API Hive State

Write curl statements that stop, start, and check the current state of your Hive service


```sh
# List Cluster
curl -X GET -u "alanbluwol:cloudera" -i http://52.64.211.37:7180/api/v12/clusters

# List Services
curl -X GET -u "alanbluwol:cloudera" -i http://52.64.211.37:7180/api/v12/clusters/alanbluwol/services

# Hive Service - STARTED | GOOD
curl -X GET -u "alanbluwol:cloudera" -i http://52.64.211.37:7180/api/v12/clusters/alanbluwol/services/hive

# Stop Service
curl -X POST -u "alanbluwol:cloudera" -i http://52.64.211.37:7180/api/v12/clusters/alanbluwol/services/hive/commands/stop

# Hive Service - STOPPED
curl -X GET -u "alanbluwol:cloudera" -i http://52.64.211.37:7180/api/v12/clusters/alanbluwol/services/hive

# Start Service
curl -X POST -u "alanbluwol:cloudera" -i http://52.64.211.37:7180/api/v12/clusters/alanbluwol/services/hive/commands/start

# Hive Service - STARTING | DISABLED
curl -X GET -u "alanbluwol:cloudera" -i http://52.64.211.37:7180/api/v12/clusters/alanbluwol/services/hive

# Hive Service - STARTED | GOOD
curl -X GET -u "alanbluwol:cloudera" -i http://52.64.211.37:7180/api/v12/clusters/alanbluwol/services/hive
```

Output

```sh
[root@ip-172-31-1-85 ~]# curl -X GET -u "alanbluwol:cloudera" -i http://52.64.211.37:7180/api/v12/clusters
HTTP/1.1 200 OK
Expires: Thu, 01-Jan-1970 00:00:00 GMT
Set-Cookie: CLOUDERA_MANAGER_SESSIONID=1grp82nh4jqi71bxw6z2zrhh9w;Path=/;HttpOnly
Content-Type: application/json
Date: Wed, 03 May 2017 03:43:01 GMT
Transfer-Encoding: chunked
Server: Jetty(6.1.26.cloudera.4)

{
  "items" : [ {
    "name" : "cluster",
    "displayName" : "alanbluwol",
    "version" : "CDH5",
    "fullVersion" : "5.9.2",
    "maintenanceMode" : false,
    "maintenanceOwners" : [ ],
    "clusterUrl" : "http://ip-172-31-1-85.ap-southeast-2.compute.internal:7180/cmf/clusterRedirect/cluster",
    "hostsUrl" : "http://ip-172-31-1-85.ap-southeast-2.compute.internal:7180/cmf/clusterRedirect/cluster/hosts",
    "entityStatus" : "GOOD_HEALTH"
  } ]
}

[root@ip-172-31-1-85 ~]# curl -X POST -u "alanbluwol:cloudera" -i http://52.64.211.37:7180/api/v12/clusters/alanbluwol/services/hive/commands/stop
HTTP/1.1 200 OK
Expires: Thu, 01-Jan-1970 00:00:00 GMT
Set-Cookie: CLOUDERA_MANAGER_SESSIONID=cygwo7vc1ay6zbji6lr9w2rd;Path=/;HttpOnly
Content-Type: application/json
Date: Wed, 03 May 2017 03:41:39 GMT
Transfer-Encoding: chunked
Server: Jetty(6.1.26.cloudera.4)

{
  "id" : 800,
  "name" : "Stop",
  "startTime" : "2017-05-03T03:41:39.875Z",
  "active" : true,
  "serviceRef" : {
    "clusterName" : "cluster",
    "serviceName" : "hive"
  }
}

[root@ip-172-31-1-85 ~]# curl -X GET -u "alanbluwol:cloudera" -i http://52.64.211.37:7180/api/v12/clusters/alanbluwol/services/hive
HTTP/1.1 200 OK
Expires: Thu, 01-Jan-1970 00:00:00 GMT
Set-Cookie: CLOUDERA_MANAGER_SESSIONID=c6o0trmbnpif84946lhd1fw2;Path=/;HttpOnly
Content-Type: application/json
Date: Wed, 03 May 2017 03:42:23 GMT
Transfer-Encoding: chunked
Server: Jetty(6.1.26.cloudera.4)

{
  "name" : "hive",
  "type" : "HIVE",
  "clusterRef" : {
    "clusterName" : "cluster"
  },
  "serviceUrl" : "http://ip-172-31-1-85.ap-southeast-2.compute.internal:7180/cmf/serviceRedirect/hive",
  "roleInstancesUrl" : "http://ip-172-31-1-85.ap-southeast-2.compute.internal:7180/cmf/serviceRedirect/hive/instances",
  "serviceState" : "STOPPED",
  "healthSummary" : "DISABLED",
  "healthChecks" : [ {
    "name" : "HIVE_HIVEMETASTORES_HEALTHY",
    "summary" : "DISABLED",
    "suppressed" : false
  }, {
    "name" : "HIVE_HIVESERVER2S_HEALTHY",
    "summary" : "DISABLED",
    "suppressed" : false
  } ],
  "configStalenessStatus" : "FRESH",
  "clientConfigStalenessStatus" : "FRESH",
  "maintenanceMode" : false,
  "maintenanceOwners" : [ ],
  "displayName" : "Hive",
  "entityStatus" : "STOPPED"
}

[root@ip-172-31-1-85 ~]# curl -X POST -u "alanbluwol:cloudera" -i http://52.64.211.37:7180/api/v12/clusters/alanbluwol/services/hive/commands/start
HTTP/1.1 200 OK
Expires: Thu, 01-Jan-1970 00:00:00 GMT
Set-Cookie: CLOUDERA_MANAGER_SESSIONID=vbn0u2zllchi1425mm5jydcfe;Path=/;HttpOnly
Content-Type: application/json
Date: Wed, 03 May 2017 03:43:31 GMT
Transfer-Encoding: chunked
Server: Jetty(6.1.26.cloudera.4)

{
  "id" : 804,
  "name" : "Start",
  "startTime" : "2017-05-03T03:43:31.684Z",
  "active" : true,
  "serviceRef" : {
    "clusterName" : "cluster",
    "serviceName" : "hive"
  }
}

[root@ip-172-31-1-85 ~]# curl -X GET -u "alanbluwol:cloudera" -i http://52.64.211.37:7180/api/v12/clusters/alanbluwol/services/hive
HTTP/1.1 200 OK
Expires: Thu, 01-Jan-1970 00:00:00 GMT
Set-Cookie: CLOUDERA_MANAGER_SESSIONID=motalwsc4i881m2gaz6kygg3x;Path=/;HttpOnly
Content-Type: application/json
Date: Wed, 03 May 2017 03:43:49 GMT
Transfer-Encoding: chunked
Server: Jetty(6.1.26.cloudera.4)

{
  "name" : "hive",
  "type" : "HIVE",
  "clusterRef" : {
    "clusterName" : "cluster"
  },
  "serviceUrl" : "http://ip-172-31-1-85.ap-southeast-2.compute.internal:7180/cmf/serviceRedirect/hive",
  "roleInstancesUrl" : "http://ip-172-31-1-85.ap-southeast-2.compute.internal:7180/cmf/serviceRedirect/hive/instances",
  "serviceState" : "STARTING",
  "healthSummary" : "DISABLED",
  "healthChecks" : [ {
    "name" : "HIVE_HIVEMETASTORES_HEALTHY",
    "summary" : "DISABLED",
    "suppressed" : false
  }, {
    "name" : "HIVE_HIVESERVER2S_HEALTHY",
    "summary" : "DISABLED",
    "suppressed" : false
  } ],
  "configStalenessStatus" : "FRESH",
  "clientConfigStalenessStatus" : "FRESH",
  "maintenanceMode" : false,
  "maintenanceOwners" : [ ],
  "displayName" : "Hive",
  "entityStatus" : "STARTING"
}

[root@ip-172-31-1-85 ~]# curl -X GET -u "alanbluwol:cloudera" -i http://52.64.211.37:7180/api/v12/clusters/alanbluwol/services/hive
HTTP/1.1 200 OK
Expires: Thu, 01-Jan-1970 00:00:00 GMT
Set-Cookie: CLOUDERA_MANAGER_SESSIONID=104ncnsoe76ja1sz5udnv2pc3n;Path=/;HttpOnly
Content-Type: application/json
Date: Wed, 03 May 2017 03:44:06 GMT
Transfer-Encoding: chunked
Server: Jetty(6.1.26.cloudera.4)

{
  "name" : "hive",
  "type" : "HIVE",
  "clusterRef" : {
    "clusterName" : "cluster"
  },
  "serviceUrl" : "http://ip-172-31-1-85.ap-southeast-2.compute.internal:7180/cmf/serviceRedirect/hive",
  "roleInstancesUrl" : "http://ip-172-31-1-85.ap-southeast-2.compute.internal:7180/cmf/serviceRedirect/hive/instances",
  "serviceState" : "STARTED",
  "healthSummary" : "GOOD",
  "healthChecks" : [ {
    "name" : "HIVE_HIVEMETASTORES_HEALTHY",
    "summary" : "GOOD",
    "suppressed" : false
  }, {
    "name" : "HIVE_HIVESERVER2S_HEALTHY",
    "summary" : "GOOD",
    "suppressed" : false
  } ],
  "configStalenessStatus" : "FRESH",
  "clientConfigStalenessStatus" : "FRESH",
  "maintenanceMode" : false,
  "maintenanceOwners" : [ ],
  "displayName" : "Hive",
  "entityStatus" : "GOOD_HEALTH"
}
```