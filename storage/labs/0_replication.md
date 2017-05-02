# Services Enablement Boot Camp

# Lab 0: HDFS Replication

## Create Partner Directory

Script

```sh
su - hdfs
hdfs dfs -mkdir /paulcompton
hdfs dfs -ls /
```

Output

```sh
[hdfs@node1 ~]$ hdfs dfs -ls /
Found 5 items
drwxr-xr-x   - alanbluwol supergroup          0 2017-05-01 22:14 /alanbluwol
drwxr-xr-x   - hdfs       supergroup          0 2017-05-02 02:09 /paulcompton
drwxr-xr-x   - hdfs       supergroup          0 2017-05-02 00:13 /precious
drwxrwxrwt   - hdfs       supergroup          0 2017-05-01 02:48 /tmp
drwxrwxrwx   - hdfs       supergroup          0 2017-05-01 22:10 /user
```

## Create Share File

Script

```sh
sudo -i
su - hdfs
HADOOP_PATH=/opt/cloudera/parcels/CDH/lib/hadoop-0.20-mapreduce
time hadoop jar $HADOOP_PATH/hadoop-examples.jar teragen -D mapred.map.tasks=4 -D dfs.block.size=33554432 500000 /alanbluwol/share
hdfs dfs -du -s -h /alanbluwol/share
```

## Pull Partner Share File

Script - Attempt 1

```sh
sudo -i
su - hdfs
HADOOP_PATH=/opt/cloudera/parcels/CDH/lib/hadoop-0.20-mapreduce
time hadoop distcp hftp://54.206.96.232:50070/paulcompton/paulcompton hdfs://node1.alanbluwol/
```

Unfortunately, the No route to host error is experienced because from my cluster I cannot see the Data Nodes of my peers cluster as these are configured on internal IPs.
Ideally, the two clusters live side by side on the same network (VPC) in order for this to work.

Output - Attempt 1

```sh
[hdfs@node1 ~]$ time hadoop distcp hftp://54.206.96.232:50070/paulcompton/paulcompton hdfs://node1.alanbluwol/
17/05/02 02:51:26 INFO tools.DistCp: Input Options: DistCpOptions{atomicCommit=false, syncFolder=false, deleteMissing=false, ignoreFailures=false, overwrite=false, append=false, useDiff=false, useRdiff=false, fromSnapshot=null, toSnapshot=null, skipCRC=false, blocking=true, numListstatusThreads=0, maxMaps=20, mapBandwidth=100, sslConfigurationFile='null', copyStrategy='uniformsize', preserveStatus=[], preserveRawXattrs=false, atomicWorkPath=null, logPath=null, sourceFileListing=null, sourcePaths=[hftp://54.206.96.232:50070/paulcompton/paulcompton], targetPath=hdfs://node1.alanbluwol/, targetPathExists=true, filtersFile='null'}
17/05/02 02:51:27 INFO client.RMProxy: Connecting to ResourceManager at node1.alanbluwol/172.31.5.116:8032
17/05/02 02:51:27 INFO tools.SimpleCopyListing: Paths (files+dirs) cnt = 4; dirCnt = 1
17/05/02 02:51:27 INFO tools.SimpleCopyListing: Build file listing completed.
17/05/02 02:51:27 INFO Configuration.deprecation: io.sort.mb is deprecated. Instead, use mapreduce.task.io.sort.mb
17/05/02 02:51:27 INFO Configuration.deprecation: io.sort.factor is deprecated. Instead, use mapreduce.task.io.sort.factor
17/05/02 02:51:27 INFO tools.DistCp: Number of paths in the copy list: 4
17/05/02 02:51:27 INFO tools.DistCp: Number of paths in the copy list: 4
17/05/02 02:51:27 INFO client.RMProxy: Connecting to ResourceManager at node1.alanbluwol/172.31.5.116:8032
17/05/02 02:51:28 INFO mapreduce.JobSubmitter: number of splits:3
17/05/02 02:51:28 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1493705206460_0012
17/05/02 02:51:28 INFO impl.YarnClientImpl: Submitted application application_1493705206460_0012
17/05/02 02:51:28 INFO mapreduce.Job: The url to track the job: http://node1.alanbluwol:8088/proxy/application_1493705206460_0012/
17/05/02 02:51:28 INFO tools.DistCp: DistCp job-id: job_1493705206460_0012
17/05/02 02:51:28 INFO mapreduce.Job: Running job: job_1493705206460_0012
17/05/02 02:51:33 INFO mapreduce.Job: Job job_1493705206460_0012 running in uber mode : false
17/05/02 02:51:33 INFO mapreduce.Job:  map 0% reduce 0%
17/05/02 02:51:44 INFO mapreduce.Job:  map 33% reduce 0%
17/05/02 02:51:45 INFO mapreduce.Job:  map 100% reduce 0%
17/05/02 02:51:50 INFO mapreduce.Job: Task Id : attempt_1493705206460_0012_m_000002_0, Status : FAILED
Error: java.io.IOException: File copy failed: hftp://54.206.96.232:50070/paulcompton/paulcompton/part-m-00001 --> hdfs://node1.alanbluwol/paulcompton/part-m-00001
	at org.apache.hadoop.tools.mapred.CopyMapper.copyFileWithRetry(CopyMapper.java:284)
	at org.apache.hadoop.tools.mapred.CopyMapper.map(CopyMapper.java:252)
	at org.apache.hadoop.tools.mapred.CopyMapper.map(CopyMapper.java:50)
	at org.apache.hadoop.mapreduce.Mapper.run(Mapper.java:145)
	at org.apache.hadoop.mapred.MapTask.runNewMapper(MapTask.java:787)
	at org.apache.hadoop.mapred.MapTask.run(MapTask.java:341)
	at org.apache.hadoop.mapred.YarnChild$2.run(YarnChild.java:164)
	at java.security.AccessController.doPrivileged(Native Method)
	at javax.security.auth.Subject.doAs(Subject.java:415)
	at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1912)
	at org.apache.hadoop.mapred.YarnChild.main(YarnChild.java:158)
Caused by: java.io.IOException: Couldn't run retriable-command: Copying hftp://54.206.96.232:50070/paulcompton/paulcompton/part-m-00001 to hdfs://node1.alanbluwol/paulcompton/part-m-00001
	at org.apache.hadoop.tools.util.RetriableCommand.execute(RetriableCommand.java:101)
	at org.apache.hadoop.tools.mapred.CopyMapper.copyFileWithRetry(CopyMapper.java:280)
	... 10 more
Caused by: org.apache.hadoop.tools.mapred.RetriableFileCopyCommand$CopyReadException: java.net.NoRouteToHostException: No route to host
	at org.apache.hadoop.tools.mapred.RetriableFileCopyCommand.getInputStream(RetriableFileCopyCommand.java:305)
	at org.apache.hadoop.tools.mapred.RetriableFileCopyCommand.copyBytes(RetriableFileCopyCommand.java:250)
	at org.apache.hadoop.tools.mapred.RetriableFileCopyCommand.copyToFile(RetriableFileCopyCommand.java:184)
	at org.apache.hadoop.tools.mapred.RetriableFileCopyCommand.doCopy(RetriableFileCopyCommand.java:124)
	at org.apache.hadoop.tools.mapred.RetriableFileCopyCommand.doExecute(RetriableFileCopyCommand.java:100)
	at org.apache.hadoop.tools.util.RetriableCommand.execute(RetriableCommand.java:87)
	... 11 more
Caused by: java.net.NoRouteToHostException: No route to host
	at java.net.PlainSocketImpl.socketConnect(Native Method)
	at java.net.AbstractPlainSocketImpl.doConnect(AbstractPlainSocketImpl.java:339)
	at java.net.AbstractPlainSocketImpl.connectToAddress(AbstractPlainSocketImpl.java:200)
	at java.net.AbstractPlainSocketImpl.connect(AbstractPlainSocketImpl.java:182)
	at java.net.SocksSocketImpl.connect(SocksSocketImpl.java:392)
	at java.net.Socket.connect(Socket.java:579)
	at sun.net.NetworkClient.doConnect(NetworkClient.java:175)
	at sun.net.www.http.HttpClient.openServer(HttpClient.java:432)
	at sun.net.www.http.HttpClient.openServer(HttpClient.java:527)
	at sun.net.www.http.HttpClient.<init>(HttpClient.java:211)
	at sun.net.www.http.HttpClient.New(HttpClient.java:308)
	at sun.net.www.http.HttpClient.New(HttpClient.java:326)
	at sun.net.www.protocol.http.HttpURLConnection.getNewHttpClient(HttpURLConnection.java:996)
	at sun.net.www.protocol.http.HttpURLConnection.plainConnect(HttpURLConnection.java:932)
	at sun.net.www.protocol.http.HttpURLConnection.connect(HttpURLConnection.java:850)
	at sun.net.www.protocol.http.HttpURLConnection.followRedirect(HttpURLConnection.java:2398)
	at sun.net.www.protocol.http.HttpURLConnection.getInputStream(HttpURLConnection.java:1557)
	at java.net.HttpURLConnection.getResponseCode(HttpURLConnection.java:468)
	at org.apache.hadoop.hdfs.web.HftpFileSystem$RangeHeaderUrlOpener.connect(HftpFileSystem.java:370)
	at org.apache.hadoop.hdfs.web.ByteRangeInputStream.openInputStream(ByteRangeInputStream.java:121)
	at org.apache.hadoop.hdfs.web.ByteRangeInputStream.getInputStream(ByteRangeInputStream.java:105)
	at org.apache.hadoop.hdfs.web.ByteRangeInputStream.<init>(ByteRangeInputStream.java:90)
	at org.apache.hadoop.hdfs.web.HftpFileSystem$RangeHeaderInputStream.<init>(HftpFileSystem.java:383)
	at org.apache.hadoop.hdfs.web.HftpFileSystem$RangeHeaderInputStream.<init>(HftpFileSystem.java:388)
	at org.apache.hadoop.hdfs.web.HftpFileSystem.open(HftpFileSystem.java:404)
	at org.apache.hadoop.fs.FileSystem.open(FileSystem.java:781)
	at org.apache.hadoop.tools.mapred.RetriableFileCopyCommand.getInputStream(RetriableFileCopyCommand.java:301)
	... 16 more

^C
real	0m26.799s
user	0m6.353s
sys	0m0.260s
```

Script - Attempt 2

```sh
sudo -i
su - hdfs
HADOOP_PATH=/opt/cloudera/parcels/CDH/lib/hadoop-0.20-mapreduce
time hadoop distcp http://54.206.96.232:50070/webhdfs/v1/paulcompton/paulcompton?op=OPEN hdfs://node1.alanbluwol/
```

Output - Attempt 2

Unfortunately, using the webhdfs API causes the URI to change to the configured internal address - which cannot be accessed externally - obviously.


```sh
http://ip-172-31-5-197.ap-southeast-2.compute.internal:50075/webhdfs/v1/paulcompton/paulcompton?op=OPEN&namenoderpcaddress=nameservice1&offset=0
```
