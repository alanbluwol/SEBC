# Services Enablement Boot Camp

# Lab 2: Test HDFS Snapshot

## Create Directory

Script - Local

```sh
AlanBluwol:SEBC alanbluwol$ scp -i ~/.ssh/evolvelabs-sydney-1.pem ~/Downloads/SEBC-Sydney.zip ec2-user@52.64.169.185:/home/ec2-user/
SEBC-Sydney.zip      100%  463KB   4.0MB/s   00:00      
```

Script - Master

```sh
sudo -i
chown hdfs /home/ec2-user/SEBC-Sydney.zip
mv /home/ec2-user/SEBC-Sydney.zip /tmp/
su - hdfs
hdfs dfs -mkdir /precious
hdfs dfs -ls /

hadoop fs -put /tmp/SEBC-Sydney.zip /precious/
hdfs dfs -ls /precious
```

Output

```sh
[hdfs@node1 ~]$ hdfs dfs -ls /
Found 4 items
drwxr-xr-x   - alanbluwol supergroup          0 2017-05-01 22:14 /alanbluwol
drwxr-xr-x   - hdfs       supergroup          0 2017-05-01 23:38 /precious
drwxrwxrwt   - hdfs       supergroup          0 2017-05-01 02:48 /tmp
drwxrwxrwx   - hdfs       supergroup          0 2017-05-01 22:10 /user

[hdfs@node1 ~]$ hadoop fs -put /tmp/SEBC-Sydney.zip /precious/
[hdfs@node1 ~]$ hdfs dfs -ls /precious
Found 1 items
-rw-r--r--   3 hdfs supergroup     474169 2017-05-01 23:43 /precious/SEBC-Sydney.zip
```

## Make Snapshottable & Take Snapshot

Refer to PNG images uploaded

	> 2_3_enable snapshot
	> 2_2_create snapshot

## Delete Folder & File

Script

```sh
hdfs dfs -rm -R /precious
hdfs dfs -ls /
rm -rf /tmp/SEBC-Sydney.zip
ls /tmp/SEBC*
```

Output

```sh
[hdfs@node1 ~]$ hdfs dfs -rm -R /precious/SEBC-Sydney.zip
17/05/01 23:57:16 INFO fs.TrashPolicyDefault: Moved: 'hdfs://node1.alanbluwol:8020/precious/SEBC-Sydney.zip' to trash at: hdfs://node1.alanbluwol:8020/user/hdfs/.Trash/Current/precious/SEBC-Sydney.zip
[hdfs@node1 ~]$ hdfs dfs -ls /precious/
[hdfs@node1 ~]$ rm -rf /tmp/SEBC-Sydney.zip
[hdfs@node1 ~]$ ls /tmp/SEBC*
ls: cannot access /tmp/SEBC*: No such file or directory
```

## Test Restored Snapshot

Script

```sh
hdfs dfs -ls /precious
```

Output

```sh
[hdfs@node1 ~]$ hdfs dfs -ls /precious/
Found 1 items
-rw-r--r--   3 hdfs supergroup     474169 2017-05-02 00:01 /precious/SEBC-Sydney.zip
```