# Services Enablement Boot Camp

## 4 - YARN Test

* In this lab you'll experiment with different work parameters to
determine which version of a common job runs fastest.

* Review the file `tools/YARNtest.sh`
* Run this script from your edge node
    * Check it first: there are 1-2 things wrong with it
    * Modify the script to echo the parameter values for each run
    * Incorporate the `time` command to report how long each job takes to finish
    * Change the mapper, reducer, and container memory parameters to emulate different resource demands
* Run the script: your most demanding job should try to max out the cluster
* Add the following to your `resources/labs` directory
    * Your modified version of the script as `3_YARNtest.sh.md`
    * The parameters used and times of your slowest and fastest runs in `4_YARN_results.md`
* In CM, navigate to YARN Applications
    * Select the `Charts` tab and take a screenshot.
    * Save it to `resources/labs/5_YARN_Charts.png`
* Set your Issue to `submitted` when it is finished.

### Upload YARN test script

Script

```sh
scp -i /Users/alanbluwol/.ssh/evolvelabs-sydney-1.pem YARNTest.sh ec2-user@52.64.211.37:/home/ec2-user/                                                                                                                                   100% 1786   198.7KB/s   00:00    
```

Output

```sh
AlanBluwol:tools alanbluwol$ scp -i /Users/alanbluwol/.ssh/evolvelabs-sydney-1.pem YARNTest.sh ec2-user@52.64.211.37:/home/ec2-user/
YARNTest.sh            100% 1786   198.7KB/s   00:00    
```

### Create Results Directory

Script

```sh
su - hdfs
hdfs dfs -mkdir /results
hdfs dfs -ls /
```

Output

```sh
[hdfs@node1 ~]$ hdfs dfs -mkdir /results
[hdfs@node1 ~]$ hdfs dfs -ls /
Found 6 items
drwxr-xr-x   - alanbluwol supergroup          0 2017-05-02 02:11 /alanbluwol
drwxr-xr-x   - hdfs       supergroup          0 2017-05-02 02:49 /paulcompton
drwxr-xr-x   - hdfs       supergroup          0 2017-05-02 00:13 /precious
drwxr-xr-x   - hdfs       supergroup          0 2017-05-02 21:15 /results
drwxrwxrwt   - hdfs       supergroup          0 2017-05-01 02:48 /tmp
drwxrwxrwx   - hdfs       supergroup          0 2017-05-02 02:25 /user
```

### Clear some large files

Script

```sh
hdfs dfs -rm -R -skipTrash /alanbluwol/*
hdfs dfs -rm -R -skipTrash /paulcompton/*
```

### Run YARN Test

Script - 8 Mappers, 1 Reducer, 512-1024 Memory

```sh
./YARNTest.sh
cat tera_*
```

Output

```sh
[root@ip-172-31-1-85 ~]# ./YARNTest.sh 
Testing loop started on Tue May 2 21:20:24 EDT 2017
Number of Mappers: 8
Number of Reducers: 1
Container Memory: 512
Mapper JVM heap: 409
Reducer JVM heap: 409
real	0m55.939s
user	0m55.940s
sys	0m3.578s

real	4m11.938s
user	4m1.580s
sys	0m17.731s
Deleted /results/tg-10GB-8-1-512
Deleted /results/ts-10GB-8-1-512
Container Memory: 1024
Mapper JVM heap: 819
Reducer JVM heap: 819
real	0m57.147s
user	0m55.930s
sys	0m3.817s

real	2m51.045s
user	2m48.739s
sys	0m10.268s
Deleted /results/tg-10GB-8-1-1024
Deleted /results/ts-10GB-8-1-1024
Testing loop ended on Tue May 2 21:29:28 EDT 2017
```

Script - 4 Mappers, 1 Reducer, 512-1024 Memory

```sh
./YARNTest.sh
cat tera_*
```

Output

```sh
[root@ip-172-31-1-85 ~]# ./YARNTest.sh 
Testing loop started on Tue May 2 21:34:44 EDT 2017
Number of Mappers: 4
Number of Containers: 1
Container Memory: 512
Mapper JVM heap: 409
Reducer JVM heap: 409

real	0m56.139s
user	0m54.962s
sys	0m3.715s

real	2m23.638s
user	2m28.352s
sys	0m7.604s
Deleted /results/tg-10GB-4-1-512
Deleted /results/ts-10GB-4-1-512
Container Memory: 1024
Mapper JVM heap: 819
Reducer JVM heap: 819

real	0m56.959s
user	0m56.097s
sys	0m3.804s

real	2m26.036s
user	2m25.004s
sys	0m7.774s
Deleted /results/tg-10GB-4-1-1024
Deleted /results/ts-10GB-4-1-1024
Testing loop ended on Tue May 2 21:41:33 EDT 2017
```