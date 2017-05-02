# Services Enablement Boot Camp

# Lab 1: Terasort Tests

### Create End User

Script

```sh
useradd -u 2200 -g "hdfs" alanbluwol

cat /etc/passwd
cat /etc/group
```

Output

```sh
[root@node1 conf]# cat /etc/passwd | grep alanbluwol
alanbluwol:x:2200:989::/home/alanbluwol:/bin/bash
[root@node1 conf]# cat /etc/group | grep hdfs
hdfs:x:989:
```

### Create Folders

Script

```sh
sudo -i
sudo -u hdfs hdfs dfs -chmod 777 /
sudo -u hdfs hdfs dfs -chmod 777 /user

su - alanbluwol
hdfs dfs -mkdir /alanbluwol
hdfs dfs -ls /
```

Output

```sh
[alanbluwol@node1 ~]$ hdfs dfs -mkdir /alanbluwol
[alanbluwol@node1 ~]$ hdfs dfs -ls /
Found 5 items
drwxr-xr-x   - alanbluwol supergroup          0 2017-05-01 21:39 /alanbluwol
drwxrwxrwt   - hdfs       supergroup          0 2017-05-01 02:48 /tmp
drwxr-xr-x   - hdfs       supergroup          0 2017-05-01 19:11 /user
```

### Run Teragen

* Create a 10 GB file using teragen
	* Set the number of mappers to four
	* Limit the block size to 32 MB
	* Land the result under your user's home directory
	* Use the time command to report the job's duration

Script
```sh
HADOOP_PATH=/opt/cloudera/parcels/CDH/lib/hadoop-0.20-mapreduce
time hadoop jar $HADOOP_PATH/hadoop-examples.jar teragen -D mapred.map.tasks=4 -D dfs.block.size=33554432 100000000 /alanbluwol/test
hdfs dfs -du -s -h /alanbluwol/test
```

Our configuration shows we've requested 4 workers.

	> 17/05/01 22:10:18 INFO mapreduce.JobSubmitter: number of splits:4

In the original run without these parameters, only 2 workers were present.
Performance was noticeably faster - 2m 6s

Output

```sh
[root@node1 ~]# su - alanbluwol
Last login: Mon May  1 22:06:51 EDT 2017 on pts/1
[alanbluwol@node1 ~]$ HADOOP_PATH=/opt/cloudera/parcels/CDH/lib/hadoop-0.20-mapreduce
[alanbluwol@node1 ~]$ time hadoop jar $HADOOP_PATH/hadoop-examples.jar teragen -D mapred.map.tasks=4 -D dfs.block.size=33554432 100000000 /alanbluwol/test
17/05/01 22:10:18 INFO client.RMProxy: Connecting to ResourceManager at node1.alanbluwol/172.31.5.116:8032
17/05/01 22:10:18 INFO terasort.TeraSort: Generating 100000000 using 4
17/05/01 22:10:18 INFO mapreduce.JobSubmitter: number of splits:4
17/05/01 22:10:18 INFO Configuration.deprecation: mapred.map.tasks is deprecated. Instead, use mapreduce.job.maps
17/05/01 22:10:18 INFO Configuration.deprecation: dfs.block.size is deprecated. Instead, use dfs.blocksize
17/05/01 22:10:18 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1493622768059_0007
17/05/01 22:10:19 INFO impl.YarnClientImpl: Submitted application application_1493622768059_0007
17/05/01 22:10:19 INFO mapreduce.Job: The url to track the job: http://node1.alanbluwol:8088/proxy/application_1493622768059_0007/
17/05/01 22:10:19 INFO mapreduce.Job: Running job: job_1493622768059_0007
17/05/01 22:10:25 INFO mapreduce.Job: Job job_1493622768059_0007 running in uber mode : false
17/05/01 22:10:25 INFO mapreduce.Job:  map 0% reduce 0%
17/05/01 22:10:36 INFO mapreduce.Job:  map 9% reduce 0%
17/05/01 22:10:39 INFO mapreduce.Job:  map 11% reduce 0%
17/05/01 22:10:42 INFO mapreduce.Job:  map 14% reduce 0%
17/05/01 22:10:45 INFO mapreduce.Job:  map 16% reduce 0%
17/05/01 22:10:48 INFO mapreduce.Job:  map 20% reduce 0%
17/05/01 22:10:51 INFO mapreduce.Job:  map 23% reduce 0%
17/05/01 22:10:54 INFO mapreduce.Job:  map 25% reduce 0%
17/05/01 22:10:57 INFO mapreduce.Job:  map 28% reduce 0%
17/05/01 22:11:06 INFO mapreduce.Job:  map 31% reduce 0%
17/05/01 22:11:07 INFO mapreduce.Job:  map 33% reduce 0%
17/05/01 22:11:09 INFO mapreduce.Job:  map 35% reduce 0%
17/05/01 22:11:10 INFO mapreduce.Job:  map 37% reduce 0%
17/05/01 22:11:12 INFO mapreduce.Job:  map 38% reduce 0%
17/05/01 22:11:13 INFO mapreduce.Job:  map 40% reduce 0%
17/05/01 22:11:15 INFO mapreduce.Job:  map 41% reduce 0%
17/05/01 22:11:16 INFO mapreduce.Job:  map 43% reduce 0%
17/05/01 22:11:19 INFO mapreduce.Job:  map 44% reduce 0%
17/05/01 22:11:21 INFO mapreduce.Job:  map 45% reduce 0%
17/05/01 22:11:22 INFO mapreduce.Job:  map 46% reduce 0%
17/05/01 22:11:27 INFO mapreduce.Job:  map 49% reduce 0%
17/05/01 22:11:28 INFO mapreduce.Job:  map 52% reduce 0%
17/05/01 22:11:30 INFO mapreduce.Job:  map 53% reduce 0%
17/05/01 22:11:31 INFO mapreduce.Job:  map 55% reduce 0%
17/05/01 22:11:33 INFO mapreduce.Job:  map 56% reduce 0%
17/05/01 22:11:34 INFO mapreduce.Job:  map 58% reduce 0%
17/05/01 22:11:37 INFO mapreduce.Job:  map 59% reduce 0%
17/05/01 22:11:40 INFO mapreduce.Job:  map 60% reduce 0%
17/05/01 22:11:42 INFO mapreduce.Job:  map 63% reduce 0%
17/05/01 22:11:43 INFO mapreduce.Job:  map 65% reduce 0%
17/05/01 22:11:45 INFO mapreduce.Job:  map 67% reduce 0%
17/05/01 22:11:47 INFO mapreduce.Job:  map 68% reduce 0%
17/05/01 22:11:49 INFO mapreduce.Job:  map 70% reduce 0%
17/05/01 22:11:50 INFO mapreduce.Job:  map 71% reduce 0%
17/05/01 22:11:52 INFO mapreduce.Job:  map 72% reduce 0%
17/05/01 22:11:53 INFO mapreduce.Job:  map 74% reduce 0%
17/05/01 22:11:55 INFO mapreduce.Job:  map 75% reduce 0%
17/05/01 22:11:56 INFO mapreduce.Job:  map 76% reduce 0%
17/05/01 22:11:58 INFO mapreduce.Job:  map 77% reduce 0%
17/05/01 22:11:59 INFO mapreduce.Job:  map 78% reduce 0%
17/05/01 22:12:01 INFO mapreduce.Job:  map 80% reduce 0%
17/05/01 22:12:02 INFO mapreduce.Job:  map 82% reduce 0%
17/05/01 22:12:10 INFO mapreduce.Job:  map 86% reduce 0%
17/05/01 22:12:11 INFO mapreduce.Job:  map 90% reduce 0%
17/05/01 22:12:13 INFO mapreduce.Job:  map 91% reduce 0%
17/05/01 22:12:14 INFO mapreduce.Job:  map 92% reduce 0%
17/05/01 22:12:20 INFO mapreduce.Job:  map 94% reduce 0%
17/05/01 22:12:21 INFO mapreduce.Job:  map 95% reduce 0%
17/05/01 22:12:22 INFO mapreduce.Job:  map 100% reduce 0%
17/05/01 22:12:22 INFO mapreduce.Job: Job job_1493622768059_0007 completed successfully
17/05/01 22:12:22 INFO mapreduce.Job: Counters: 31
	File System Counters
		FILE: Number of bytes read=0
		FILE: Number of bytes written=492802
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=344
		HDFS: Number of bytes written=10000000000
		HDFS: Number of read operations=16
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=8
	Job Counters 
		Launched map tasks=4
		Other local map tasks=4
		Total time spent by all maps in occupied slots (ms)=456414
		Total time spent by all reduces in occupied slots (ms)=0
		Total time spent by all map tasks (ms)=456414
		Total vcore-seconds taken by all map tasks=456414
		Total megabyte-seconds taken by all map tasks=467367936
	Map-Reduce Framework
		Map input records=100000000
		Map output records=100000000
		Input split bytes=344
		Spilled Records=0
		Failed Shuffles=0
		Merged Map outputs=0
		GC time elapsed (ms)=1512
		CPU time spent (ms)=166830
		Physical memory (bytes) snapshot=784359424
		Virtual memory (bytes) snapshot=6329577472
		Total committed heap usage (bytes)=1027604480
	org.apache.hadoop.examples.terasort.TeraGen$Counters
		CHECKSUM=214760662691937609
	File Input Format Counters 
		Bytes Read=0
	File Output Format Counters 
		Bytes Written=10000000000

real	2m6.441s
user	0m5.294s
sys	0m0.249s

[alanbluwol@node1 ~]$ hdfs dfs -du -s -h /alanbluwol/test
9.3 G  27.9 G  /alanbluwol/test
```

Notes

	> check on master: http://52.65.79.152:8088/

### Run Terasort

Script

```sh
time hadoop jar $HADOOP_PATH/hadoop-examples.jar terasort /alanbluwol/test /alanbluwol/sorted
hdfs dfs -ls /alanbluwol/sorted

hdfs dfs -rm -R /alanbluwol/sorted
hadoop fs -expunge

/# On Nodes Inspect
iostat -t 1 100
vmstat -t 1 100

```

Output

```sh
[alanbluwol@node1 ~]$ time hadoop jar $HADOOP_PATH/hadoop-examples.jar terasort /alanbluwol/test /alanbluwol/sorted
17/05/01 22:14:37 INFO terasort.TeraSort: starting
17/05/01 22:14:39 INFO input.FileInputFormat: Total input paths to process : 4
Spent 225ms computing base-splits.
Spent 5ms computing TeraScheduler splits.
Computing input splits took 231ms
Sampling 10 splits of 300
Making 6 from 100000 sampled records
Computing parititions took 833ms
Spent 1066ms computing partitions.
17/05/01 22:14:40 INFO client.RMProxy: Connecting to ResourceManager at node1.alanbluwol/172.31.5.116:8032
17/05/01 22:14:40 INFO mapreduce.JobSubmitter: number of splits:300
17/05/01 22:14:40 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1493622768059_0008
17/05/01 22:14:40 INFO impl.YarnClientImpl: Submitted application application_1493622768059_0008
17/05/01 22:14:40 INFO mapreduce.Job: The url to track the job: http://node1.alanbluwol:8088/proxy/application_1493622768059_0008/
17/05/01 22:14:40 INFO mapreduce.Job: Running job: job_1493622768059_0008
17/05/01 22:14:47 INFO mapreduce.Job: Job job_1493622768059_0008 running in uber mode : false
17/05/01 22:14:47 INFO mapreduce.Job:  map 0% reduce 0%
17/05/01 22:14:57 INFO mapreduce.Job:  map 1% reduce 0%
17/05/01 22:14:59 INFO mapreduce.Job:  map 2% reduce 0%
17/05/01 22:15:00 INFO mapreduce.Job:  map 4% reduce 0%
17/05/01 22:15:07 INFO mapreduce.Job:  map 5% reduce 0%
17/05/01 22:15:11 INFO mapreduce.Job:  map 7% reduce 0%
17/05/01 22:15:13 INFO mapreduce.Job:  map 8% reduce 0%
17/05/01 22:15:21 INFO mapreduce.Job:  map 9% reduce 0%
17/05/01 22:15:23 INFO mapreduce.Job:  map 11% reduce 0%
17/05/01 22:15:24 INFO mapreduce.Job:  map 12% reduce 0%
17/05/01 22:15:34 INFO mapreduce.Job:  map 13% reduce 0%
17/05/01 22:15:35 INFO mapreduce.Job:  map 15% reduce 0%
17/05/01 22:15:36 INFO mapreduce.Job:  map 16% reduce 0%
17/05/01 22:15:43 INFO mapreduce.Job:  map 17% reduce 0%
17/05/01 22:15:46 INFO mapreduce.Job:  map 18% reduce 0%
17/05/01 22:15:48 INFO mapreduce.Job:  map 20% reduce 0%
17/05/01 22:15:54 INFO mapreduce.Job:  map 21% reduce 0%
17/05/01 22:15:57 INFO mapreduce.Job:  map 22% reduce 0%
17/05/01 22:16:00 INFO mapreduce.Job:  map 24% reduce 0%
17/05/01 22:16:06 INFO mapreduce.Job:  map 25% reduce 0%
17/05/01 22:16:08 INFO mapreduce.Job:  map 26% reduce 0%
17/05/01 22:16:12 INFO mapreduce.Job:  map 28% reduce 0%
17/05/01 22:16:19 INFO mapreduce.Job:  map 29% reduce 0%
17/05/01 22:16:20 INFO mapreduce.Job:  map 30% reduce 0%
17/05/01 22:16:24 INFO mapreduce.Job:  map 31% reduce 0%
17/05/01 22:16:25 INFO mapreduce.Job:  map 32% reduce 0%
17/05/01 22:16:29 INFO mapreduce.Job:  map 33% reduce 0%
17/05/01 22:16:31 INFO mapreduce.Job:  map 34% reduce 0%
17/05/01 22:16:36 INFO mapreduce.Job:  map 35% reduce 0%
17/05/01 22:16:37 INFO mapreduce.Job:  map 36% reduce 0%
17/05/01 22:16:40 INFO mapreduce.Job:  map 37% reduce 0%
17/05/01 22:16:42 INFO mapreduce.Job:  map 38% reduce 0%
17/05/01 22:16:47 INFO mapreduce.Job:  map 39% reduce 0%
17/05/01 22:16:48 INFO mapreduce.Job:  map 40% reduce 0%
17/05/01 22:16:52 INFO mapreduce.Job:  map 41% reduce 0%
17/05/01 22:16:55 INFO mapreduce.Job:  map 42% reduce 0%
17/05/01 22:17:00 INFO mapreduce.Job:  map 43% reduce 0%
17/05/01 22:17:01 INFO mapreduce.Job:  map 44% reduce 0%
17/05/01 22:17:04 INFO mapreduce.Job:  map 46% reduce 0%
17/05/01 22:17:13 INFO mapreduce.Job:  map 47% reduce 0%
17/05/01 22:17:14 INFO mapreduce.Job:  map 48% reduce 0%
17/05/01 22:17:15 INFO mapreduce.Job:  map 50% reduce 0%
17/05/01 22:17:23 INFO mapreduce.Job:  map 51% reduce 0%
17/05/01 22:17:25 INFO mapreduce.Job:  map 52% reduce 0%
17/05/01 22:17:26 INFO mapreduce.Job:  map 53% reduce 0%
17/05/01 22:17:32 INFO mapreduce.Job:  map 54% reduce 0%
17/05/01 22:17:33 INFO mapreduce.Job:  map 55% reduce 0%
17/05/01 22:17:37 INFO mapreduce.Job:  map 57% reduce 0%
17/05/01 22:17:41 INFO mapreduce.Job:  map 58% reduce 0%
17/05/01 22:17:48 INFO mapreduce.Job:  map 59% reduce 0%
17/05/01 22:17:49 INFO mapreduce.Job:  map 61% reduce 0%
17/05/01 22:17:51 INFO mapreduce.Job:  map 62% reduce 0%
17/05/01 22:17:59 INFO mapreduce.Job:  map 64% reduce 0%
17/05/01 22:18:02 INFO mapreduce.Job:  map 65% reduce 0%
17/05/01 22:18:05 INFO mapreduce.Job:  map 66% reduce 0%
17/05/01 22:18:09 INFO mapreduce.Job:  map 67% reduce 0%
17/05/01 22:18:12 INFO mapreduce.Job:  map 68% reduce 0%
17/05/01 22:18:14 INFO mapreduce.Job:  map 69% reduce 0%
17/05/01 22:18:17 INFO mapreduce.Job:  map 70% reduce 0%
17/05/01 22:18:22 INFO mapreduce.Job:  map 71% reduce 0%
17/05/01 22:18:25 INFO mapreduce.Job:  map 72% reduce 0%
17/05/01 22:18:26 INFO mapreduce.Job:  map 73% reduce 0%
17/05/01 22:18:27 INFO mapreduce.Job:  map 74% reduce 0%
17/05/01 22:18:32 INFO mapreduce.Job:  map 75% reduce 0%
17/05/01 22:18:35 INFO mapreduce.Job:  map 76% reduce 0%
17/05/01 22:18:38 INFO mapreduce.Job:  map 78% reduce 0%
17/05/01 22:18:45 INFO mapreduce.Job:  map 80% reduce 0%
17/05/01 22:18:50 INFO mapreduce.Job:  map 81% reduce 0%
17/05/01 22:18:54 INFO mapreduce.Job:  map 82% reduce 0%
17/05/01 22:18:55 INFO mapreduce.Job:  map 83% reduce 0%
17/05/01 22:18:56 INFO mapreduce.Job:  map 84% reduce 0%
17/05/01 22:19:06 INFO mapreduce.Job:  map 84% reduce 5%
17/05/01 22:19:07 INFO mapreduce.Job:  map 85% reduce 9%
17/05/01 22:19:08 INFO mapreduce.Job:  map 86% reduce 9%
17/05/01 22:19:09 INFO mapreduce.Job:  map 86% reduce 11%
17/05/01 22:19:10 INFO mapreduce.Job:  map 86% reduce 13%
17/05/01 22:19:12 INFO mapreduce.Job:  map 86% reduce 14%
17/05/01 22:19:13 INFO mapreduce.Job:  map 86% reduce 15%
17/05/01 22:19:15 INFO mapreduce.Job:  map 86% reduce 18%
17/05/01 22:19:16 INFO mapreduce.Job:  map 86% reduce 19%
17/05/01 22:19:18 INFO mapreduce.Job:  map 87% reduce 20%
17/05/01 22:19:19 INFO mapreduce.Job:  map 88% reduce 22%
17/05/01 22:19:21 INFO mapreduce.Job:  map 88% reduce 24%
17/05/01 22:19:25 INFO mapreduce.Job:  map 88% reduce 25%
17/05/01 22:19:27 INFO mapreduce.Job:  map 89% reduce 25%
17/05/01 22:19:29 INFO mapreduce.Job:  map 90% reduce 25%
17/05/01 22:19:32 INFO mapreduce.Job:  map 91% reduce 25%
17/05/01 22:19:40 INFO mapreduce.Job:  map 92% reduce 25%
17/05/01 22:19:41 INFO mapreduce.Job:  map 93% reduce 25%
17/05/01 22:19:42 INFO mapreduce.Job:  map 93% reduce 26%
17/05/01 22:19:47 INFO mapreduce.Job:  map 94% reduce 26%
17/05/01 22:19:51 INFO mapreduce.Job:  map 95% reduce 26%
17/05/01 22:19:52 INFO mapreduce.Job:  map 96% reduce 26%
17/05/01 22:19:54 INFO mapreduce.Job:  map 96% reduce 27%
17/05/01 22:19:59 INFO mapreduce.Job:  map 97% reduce 27%
17/05/01 22:20:03 INFO mapreduce.Job:  map 98% reduce 27%
17/05/01 22:20:05 INFO mapreduce.Job:  map 99% reduce 27%
17/05/01 22:20:13 INFO mapreduce.Job:  map 100% reduce 27%
17/05/01 22:20:14 INFO mapreduce.Job:  map 100% reduce 28%
17/05/01 22:20:15 INFO mapreduce.Job:  map 100% reduce 34%
17/05/01 22:20:16 INFO mapreduce.Job:  map 100% reduce 44%
17/05/01 22:20:17 INFO mapreduce.Job:  map 100% reduce 47%
17/05/01 22:20:18 INFO mapreduce.Job:  map 100% reduce 51%
17/05/01 22:20:19 INFO mapreduce.Job:  map 100% reduce 58%
17/05/01 22:20:20 INFO mapreduce.Job:  map 100% reduce 60%
17/05/01 22:20:21 INFO mapreduce.Job:  map 100% reduce 61%
17/05/01 22:20:22 INFO mapreduce.Job:  map 100% reduce 62%
17/05/01 22:20:23 INFO mapreduce.Job:  map 100% reduce 63%
17/05/01 22:20:24 INFO mapreduce.Job:  map 100% reduce 64%
17/05/01 22:20:25 INFO mapreduce.Job:  map 100% reduce 66%
17/05/01 22:20:26 INFO mapreduce.Job:  map 100% reduce 67%
17/05/01 22:20:28 INFO mapreduce.Job:  map 100% reduce 69%
17/05/01 22:20:30 INFO mapreduce.Job:  map 100% reduce 75%
17/05/01 22:20:31 INFO mapreduce.Job:  map 100% reduce 76%
17/05/01 22:20:32 INFO mapreduce.Job:  map 100% reduce 77%
17/05/01 22:20:33 INFO mapreduce.Job:  map 100% reduce 79%
17/05/01 22:20:34 INFO mapreduce.Job:  map 100% reduce 80%
17/05/01 22:20:36 INFO mapreduce.Job:  map 100% reduce 81%
17/05/01 22:20:37 INFO mapreduce.Job:  map 100% reduce 83%
17/05/01 22:20:39 INFO mapreduce.Job:  map 100% reduce 84%
17/05/01 22:20:40 INFO mapreduce.Job:  map 100% reduce 86%
17/05/01 22:20:42 INFO mapreduce.Job:  map 100% reduce 87%
17/05/01 22:20:43 INFO mapreduce.Job:  map 100% reduce 89%
17/05/01 22:20:46 INFO mapreduce.Job:  map 100% reduce 90%
17/05/01 22:20:47 INFO mapreduce.Job:  map 100% reduce 93%
17/05/01 22:20:49 INFO mapreduce.Job:  map 100% reduce 95%
17/05/01 22:20:50 INFO mapreduce.Job:  map 100% reduce 96%
17/05/01 22:20:52 INFO mapreduce.Job:  map 100% reduce 97%
17/05/01 22:20:55 INFO mapreduce.Job:  map 100% reduce 99%
17/05/01 22:20:58 INFO mapreduce.Job:  map 100% reduce 100%
17/05/01 22:20:59 INFO mapreduce.Job: Job job_1493622768059_0008 completed successfully
17/05/01 22:20:59 INFO mapreduce.Job: Counters: 49
	File System Counters
		FILE: Number of bytes read=4446008674
		FILE: Number of bytes written=8826938237
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=10000036600
		HDFS: Number of bytes written=10000000000
		HDFS: Number of read operations=918
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=12
	Job Counters 
		Launched map tasks=300
		Launched reduce tasks=6
		Data-local map tasks=300
		Total time spent by all maps in occupied slots (ms)=2803205
		Total time spent by all reduces in occupied slots (ms)=634058
		Total time spent by all map tasks (ms)=2803205
		Total time spent by all reduce tasks (ms)=634058
		Total vcore-seconds taken by all map tasks=2803205
		Total vcore-seconds taken by all reduce tasks=634058
		Total megabyte-seconds taken by all map tasks=2870481920
		Total megabyte-seconds taken by all reduce tasks=649275392
	Map-Reduce Framework
		Map input records=100000000
		Map output records=100000000
		Map output bytes=10200000000
		Map output materialized bytes=4342784689
		Input split bytes=36600
		Combine input records=0
		Combine output records=0
		Reduce input groups=100000000
		Reduce shuffle bytes=4342784689
		Reduce input records=100000000
		Reduce output records=100000000
		Spilled Records=200000000
		Shuffled Maps =1800
		Failed Shuffles=0
		Merged Map outputs=1800
		GC time elapsed (ms)=42925
		CPU time spent (ms)=1413840
		Physical memory (bytes) snapshot=151282737152
		Virtual memory (bytes) snapshot=482473689088
		Total committed heap usage (bytes)=177985814528
	Shuffle Errors
		BAD_ID=0
		CONNECTION=0
		IO_ERROR=0
		WRONG_LENGTH=0
		WRONG_MAP=0
		WRONG_REDUCE=0
	File Input Format Counters 
		Bytes Read=10000000000
	File Output Format Counters 
		Bytes Written=10000000000
17/05/01 22:20:59 INFO terasort.TeraSort: done

real	6m22.395s
user	0m8.380s
sys	0m0.314s
[alanbluwol@node1 ~]$ hdfs dfs -ls /alanbluwol/sorted
Found 8 items
-rw-r--r--   1 alanbluwol supergroup          0 2017-05-01 22:20 /alanbluwol/sorted/_SUCCESS
-rw-r--r--  10 alanbluwol supergroup         55 2017-05-01 22:14 /alanbluwol/sorted/_partition.lst
-rw-r--r--   1 alanbluwol supergroup 1657210500 2017-05-01 22:20 /alanbluwol/sorted/part-r-00000
-rw-r--r--   1 alanbluwol supergroup 1669051200 2017-05-01 22:20 /alanbluwol/sorted/part-r-00001
-rw-r--r--   1 alanbluwol supergroup 1654375500 2017-05-01 22:20 /alanbluwol/sorted/part-r-00002
-rw-r--r--   1 alanbluwol supergroup 1668843100 2017-05-01 22:20 /alanbluwol/sorted/part-r-00003
-rw-r--r--   1 alanbluwol supergroup 1681409800 2017-05-01 22:20 /alanbluwol/sorted/part-r-00004
-rw-r--r--   1 alanbluwol supergroup 1669109900 2017-05-01 22:20 /alanbluwol/sorted/part-r-00005
```
