# Services Enablement Boot Camp

# 5 - Kerberise Pi

```sh
[root@ip-172-31-5-118 ~]# su - jemaine

[jemaine@node1 ~]$ kinit
Password for jemaine@ALANBLUWOL.AU: 

[jemaine@node1 ~]$ klist
Ticket cache: FILE:/tmp/krb5cc_2900
Default principal: jemaine@ALANBLUWOL.AU

Valid starting       Expires              Service principal
05/04/2017 21:41:32  05/05/2017 21:41:32  krbtgt/ALANBLUWOL.AU@ALANBLUWOL.AU
	renew until 05/11/2017 21:41:32
	
[jemaine@node1 ~]$ HADOOP_PATH=/opt/cloudera/parcels/CDH/lib/hadoop-0.20-mapreduce

[jemaine@node1 ~]$ time hadoop jar /opt/cloudera/parcels/CDH/lib/hadoop-0.20-mapreduce/hadoop-examples.jar pi 10 10
Number of Maps  = 10
Samples per Map = 10
Wrote input for Map #0
Wrote input for Map #1
Wrote input for Map #2
Wrote input for Map #3
Wrote input for Map #4
Wrote input for Map #5
Wrote input for Map #6
Wrote input for Map #7
Wrote input for Map #8
Wrote input for Map #9
Starting Job
17/05/04 21:41:49 INFO client.RMProxy: Connecting to ResourceManager at node1.alanbluwol/172.31.5.118:8032
17/05/04 21:41:49 INFO hdfs.DFSClient: Created token for jemaine: HDFS_DELEGATION_TOKEN owner=jemaine@ALANBLUWOL.AU, renewer=yarn, realUser=, issueDate=1493948509273, maxDate=1494553309273, sequenceNumber=2, masterKeyId=2 on 172.31.5.118:8020
17/05/04 21:41:49 INFO security.TokenCache: Got dt for hdfs://node1.alanbluwol:8020; Kind: HDFS_DELEGATION_TOKEN, Service: 172.31.5.118:8020, Ident: (token for jemaine: HDFS_DELEGATION_TOKEN owner=jemaine@ALANBLUWOL.AU, renewer=yarn, realUser=, issueDate=1493948509273, maxDate=1494553309273, sequenceNumber=2, masterKeyId=2)
17/05/04 21:41:49 INFO input.FileInputFormat: Total input paths to process : 10
17/05/04 21:41:49 INFO mapreduce.JobSubmitter: number of splits:10
17/05/04 21:41:49 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1493947619596_0002
17/05/04 21:41:49 INFO mapreduce.JobSubmitter: Kind: HDFS_DELEGATION_TOKEN, Service: 172.31.5.118:8020, Ident: (token for jemaine: HDFS_DELEGATION_TOKEN owner=jemaine@ALANBLUWOL.AU, renewer=yarn, realUser=, issueDate=1493948509273, maxDate=1494553309273, sequenceNumber=2, masterKeyId=2)
17/05/04 21:41:50 INFO impl.YarnClientImpl: Submitted application application_1493947619596_0002
17/05/04 21:41:50 INFO mapreduce.Job: The url to track the job: http://node1.alanbluwol:8088/proxy/application_1493947619596_0002/
17/05/04 21:41:50 INFO mapreduce.Job: Running job: job_1493947619596_0002
17/05/04 21:41:58 INFO mapreduce.Job: Job job_1493947619596_0002 running in uber mode : false
17/05/04 21:41:58 INFO mapreduce.Job:  map 0% reduce 0%
17/05/04 21:42:03 INFO mapreduce.Job:  map 10% reduce 0%
17/05/04 21:42:06 INFO mapreduce.Job:  map 20% reduce 0%
17/05/04 21:42:07 INFO mapreduce.Job:  map 30% reduce 0%
17/05/04 21:42:08 INFO mapreduce.Job:  map 40% reduce 0%
17/05/04 21:42:10 INFO mapreduce.Job:  map 80% reduce 0%
17/05/04 21:42:13 INFO mapreduce.Job:  map 100% reduce 0%
17/05/04 21:42:17 INFO mapreduce.Job:  map 100% reduce 100%
17/05/04 21:42:17 INFO mapreduce.Job: Job job_1493947619596_0002 completed successfully
17/05/04 21:42:17 INFO mapreduce.Job: Counters: 49
	File System Counters
		FILE: Number of bytes read=80
		FILE: Number of bytes written=1383658
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=2740
		HDFS: Number of bytes written=215
		HDFS: Number of read operations=43
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=3
	Job Counters 
		Launched map tasks=10
		Launched reduce tasks=1
		Data-local map tasks=10
		Total time spent by all maps in occupied slots (ms)=64044
		Total time spent by all reduces in occupied slots (ms)=3208
		Total time spent by all map tasks (ms)=64044
		Total time spent by all reduce tasks (ms)=3208
		Total vcore-seconds taken by all map tasks=64044
		Total vcore-seconds taken by all reduce tasks=3208
		Total megabyte-seconds taken by all map tasks=65581056
		Total megabyte-seconds taken by all reduce tasks=3284992
	Map-Reduce Framework
		Map input records=10
		Map output records=20
		Map output bytes=180
		Map output materialized bytes=339
		Input split bytes=1560
		Combine input records=0
		Combine output records=0
		Reduce input groups=2
		Reduce shuffle bytes=339
		Reduce input records=20
		Reduce output records=0
		Spilled Records=40
		Shuffled Maps =10
		Failed Shuffles=0
		Merged Map outputs=10
		GC time elapsed (ms)=411
		CPU time spent (ms)=7050
		Physical memory (bytes) snapshot=4872785920
		Virtual memory (bytes) snapshot=17429975040
		Total committed heap usage (bytes)=5711069184
	Shuffle Errors
		BAD_ID=0
		CONNECTION=0
		IO_ERROR=0
		WRONG_LENGTH=0
		WRONG_MAP=0
		WRONG_REDUCE=0
	File Input Format Counters 
		Bytes Read=1180
	File Output Format Counters 
		Bytes Written=97
Job Finished in 28.496 seconds
Estimated value of Pi is 3.20000000000000000000

real	0m31.472s
user	0m5.673s
sys	    0m0.233s
```

