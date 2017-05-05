# Services Enablement Boot Camp

# 4 - HDFS Testing

```sh
[cate@node1 ~]$ time hadoop jar $HADOOP_PATH/hadoop-examples.jar teragen -D mapreduce.map.memory.mb=512 -D mapred.map.tasks=8 -D dfs.block.size=16777216 655360 /user/cate/tgen
17/05/04 20:37:18 INFO client.RMProxy: Connecting to ResourceManager at node1.alanbluwol/172.31.5.118:8032
17/05/04 20:37:19 INFO terasort.TeraGen: Generating 655360 using 8
17/05/04 20:37:19 INFO mapreduce.JobSubmitter: number of splits:8
17/05/04 20:37:19 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
17/05/04 20:37:19 INFO Configuration.deprecation: dfs.block.size is deprecated. Instead, use dfs.blocksize
17/05/04 20:37:19 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1493942623114_0001
17/05/04 20:37:20 INFO impl.YarnClientImpl: Submitted application application_1493942623114_0001
17/05/04 20:37:20 INFO mapreduce.Job: The url to track the job: http://node1.alanbluwol:8088/proxy/application_1493942623114_0001/
17/05/04 20:37:20 INFO mapreduce.Job: Running job: job_1493942623114_0001
17/05/04 20:37:27 INFO mapreduce.Job: Job job_1493942623114_0001 running in uber mode : false
17/05/04 20:37:27 INFO mapreduce.Job:  map 0% reduce 0%
17/05/04 20:37:34 INFO mapreduce.Job:  map 13% reduce 0%
17/05/04 20:37:35 INFO mapreduce.Job:  map 75% reduce 0%
17/05/04 20:37:41 INFO mapreduce.Job:  map 88% reduce 0%
17/05/04 20:37:42 INFO mapreduce.Job:  map 100% reduce 0%
17/05/04 20:37:42 INFO mapreduce.Job: Job job_1493942623114_0001 completed successfully
17/05/04 20:37:42 INFO mapreduce.Job: Counters: 31
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=994408
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=677
		HDFS: Number of bytes written=65536000
		HDFS: Number of read operations=32
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=16
	Job Counters 
		Launched map tasks=8
		Other local map tasks=8
		Total time spent by all maps in occupied slots (ms)=46141
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=46141
		Total vcore-seconds taken by all map tasks=46141
		Total megabyte-seconds taken by all map tasks=47248384
	Map-Reduce Framework
		Map input records=655360
		Map output records=655360
		Input split bytes=677
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=729
		CPU time spent (ms)=17230
		Physical memory (bytes) snapshot=1548390400
		Virtual memory (bytes) snapshot=9064787968
		Total committed heap usage (bytes)=2428502016
	org.apache.hadoop.examples.terasort.TeraGen$Counters
		CHECKSUM=1408100361519672
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=65536000

real	0m26.227s
user	0m5.431s
sys	    0m0.236s
[cate@node1 ~]$ hdfs dfs -ls /user/cate/tgen
Found 9 items
-rw-r--r--   3 cate aussies          0 2017-05-04 20:37 /user/cate/tgen/_SUCCESS
-rw-r--r--   3 cate aussies    8192000 2017-05-04 20:37 /user/cate/tgen/part-m-00000
-rw-r--r--   3 cate aussies    8192000 2017-05-04 20:37 /user/cate/tgen/part-m-00001
-rw-r--r--   3 cate aussies    8192000 2017-05-04 20:37 /user/cate/tgen/part-m-00002
-rw-r--r--   3 cate aussies    8192000 2017-05-04 20:37 /user/cate/tgen/part-m-00003
-rw-r--r--   3 cate aussies    8192000 2017-05-04 20:37 /user/cate/tgen/part-m-00004
-rw-r--r--   3 cate aussies    8192000 2017-05-04 20:37 /user/cate/tgen/part-m-00005
-rw-r--r--   3 cate aussies    8192000 2017-05-04 20:37 /user/cate/tgen/part-m-00006
-rw-r--r--   3 cate aussies    8192000 2017-05-04 20:37 /user/cate/tgen/part-m-00007
```
