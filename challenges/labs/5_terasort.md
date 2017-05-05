# Services Enablement Boot Camp

# 5 - Kerberise TeraSort

```sh
[root@ip-172-31-5-118 ~]# su - cate

Last login: Thu May  4 20:31:32 EDT 2017 on pts/0

[cate@node1 ~]$ kinit
Password for cate@ALANBLUWOL.AU: 

[cate@node1 ~]$ klist
Ticket cache: FILE:/tmp/krb5cc_2300
Default principal: cate@ALANBLUWOL.AU

Valid starting       Expires              Service principal
05/04/2017 21:38:13  05/05/2017 21:38:13  krbtgt/ALANBLUWOL.AU@ALANBLUWOL.AU
	renew until 05/11/2017 21:38:13
	
[cate@node1 ~]$ HADOOP_PATH=/opt/cloudera/parcels/CDH/lib/hadoop-0.20-mapreduce

[cate@node1 ~]$ time hadoop jar $HADOOP_PATH/hadoop-examples.jar terasort /user/cate/tgen /user/cate/tsort640m
17/05/04 21:38:33 INFO terasort.TeraSort: starting
17/05/04 21:38:34 INFO hdfs.DFSClient: Created token for cate: HDFS_DELEGATION_TOKEN owner=cate@ALANBLUWOL.AU, renewer=yarn, realUser=, issueDate=1493948314746, maxDate=1494553114746, sequenceNumber=1, masterKeyId=2 on 172.31.5.118:8020
17/05/04 21:38:34 INFO security.TokenCache: Got dt for hdfs://node1.alanbluwol:8020; Kind: HDFS_DELEGATION_TOKEN, Service: 172.31.5.118:8020, Ident: (token for cate: HDFS_DELEGATION_TOKEN owner=cate@ALANBLUWOL.AU, renewer=yarn, realUser=, issueDate=1493948314746, maxDate=1494553114746, sequenceNumber=1, masterKeyId=2)
17/05/04 21:38:34 INFO input.FileInputFormat: Total input paths to process : 8
Spent 257ms computing base-splits.
Spent 2ms computing TeraScheduler splits.
Computing input splits took 259ms
Sampling 8 splits of 8
Making 6 from 100000 sampled records
Computing parititions took 890ms
Spent 1152ms computing partitions.
17/05/04 21:38:35 INFO client.RMProxy: Connecting to ResourceManager at node1.alanbluwol/172.31.5.118:8032
17/05/04 21:38:36 INFO mapreduce.JobSubmitter: number of splits:8
17/05/04 21:38:36 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1493947619596_0001
17/05/04 21:38:36 INFO mapreduce.JobSubmitter: Kind: HDFS_DELEGATION_TOKEN, Service: 172.31.5.118:8020, Ident: (token for cate: HDFS_DELEGATION_TOKEN owner=cate@ALANBLUWOL.AU, renewer=yarn, realUser=, issueDate=1493948314746, maxDate=1494553114746, sequenceNumber=1, masterKeyId=2)
17/05/04 21:38:37 INFO impl.YarnClientImpl: Submitted application application_1493947619596_0001
17/05/04 21:38:37 INFO mapreduce.Job: The url to track the job: http://node1.alanbluwol:8088/proxy/application_1493947619596_0001/
17/05/04 21:38:37 INFO mapreduce.Job: Running job: job_1493947619596_0001
17/05/04 21:38:46 INFO mapreduce.Job: Job job_1493947619596_0001 running in uber mode : false
17/05/04 21:38:46 INFO mapreduce.Job:  map 0% reduce 0%
17/05/04 21:38:52 INFO mapreduce.Job:  map 13% reduce 0%
17/05/04 21:38:57 INFO mapreduce.Job:  map 25% reduce 0%
17/05/04 21:38:58 INFO mapreduce.Job:  map 50% reduce 0%
17/05/04 21:39:01 INFO mapreduce.Job:  map 88% reduce 0%
17/05/04 21:39:02 INFO mapreduce.Job:  map 100% reduce 0%
17/05/04 21:39:08 INFO mapreduce.Job:  map 100% reduce 17%
17/05/04 21:39:10 INFO mapreduce.Job:  map 100% reduce 50%
17/05/04 21:39:12 INFO mapreduce.Job:  map 100% reduce 83%
17/05/04 21:39:13 INFO mapreduce.Job:  map 100% reduce 100%
17/05/04 21:39:13 INFO mapreduce.Job: Job job_1493947619596_0001 completed successfully
17/05/04 21:39:13 INFO mapreduce.Job: Counters: 49
	File System Counters
		FILE: Number of bytes read=28392200
		FILE: Number of bytes written=58523239
		FILE: Number of read operations=0
		FILE: Number of large read operations=0
		FILE: Number of write operations=0
		HDFS: Number of bytes read=65536968
		HDFS: Number of bytes written=65536000
		HDFS: Number of read operations=42
		HDFS: Number of large read operations=0
		HDFS: Number of write operations=12
	Job Counters 
		Launched map tasks=8
		Launched reduce tasks=6
		Data-local map tasks=8
		Total time spent by all maps in occupied slots (ms)=69390
		Total time spent by all reduces in occupied slots (ms)=39493
		Total time spent by all map tasks (ms)=69390
		Total time spent by all reduce tasks (ms)=39493
		Total vcore-seconds taken by all map tasks=69390
		Total vcore-seconds taken by all reduce tasks=39493
		Total megabyte-seconds taken by all map tasks=71055360
		Total megabyte-seconds taken by all reduce tasks=40440832
	Map-Reduce Framework
		Map input records=655360
		Map output records=655360
		Map output bytes=66846720
		Map output materialized bytes=28360507
		Input split bytes=968
		Combine input records=0
		Combine output records=0
		Reduce input groups=655360
		Reduce shuffle bytes=28360507
		Reduce input records=655360
		Reduce output records=655360
		Spilled Records=1310720
		Shuffled Maps =48
		Failed Shuffles=0
		Merged Map outputs=48
		GC time elapsed (ms)=1068
		CPU time spent (ms)=39810
		Physical memory (bytes) snapshot=5024018432
		Virtual memory (bytes) snapshot=22199418880
		Total committed heap usage (bytes)=6489636864
	Shuffle Errors
		BAD_ID=0
		CONNECTION=0
		IO_ERROR=0
		WRONG_LENGTH=0
		WRONG_MAP=0
		WRONG_REDUCE=0
	File Input Format Counters 
		Bytes Read=65536000
	File Output Format Counters 
		Bytes Written=65536000
17/05/04 21:39:13 INFO terasort.TeraSort: done

real	0m41.437s
user	0m7.626s
sys	    0m0.316s

[cate@node1 ~]$ hdfs dfs -ls  /user/cate/tsort640m
Found 8 items
-rw-r--r--   1 cate aussies          0 2017-05-04 21:39 /user/cate/tsort640m/_SUCCESS
-rw-r--r--  10 cate aussies         55 2017-05-04 21:38 /user/cate/tsort640m/_partition.lst
-rw-r--r--   1 cate aussies   10987700 2017-05-04 21:39 /user/cate/tsort640m/part-r-00000
-rw-r--r--   1 cate aussies   10863500 2017-05-04 21:39 /user/cate/tsort640m/part-r-00001
-rw-r--r--   1 cate aussies   10822300 2017-05-04 21:39 /user/cate/tsort640m/part-r-00002
-rw-r--r--   1 cate aussies   10941400 2017-05-04 21:39 /user/cate/tsort640m/part-r-00003
-rw-r--r--   1 cate aussies   10916500 2017-05-04 21:39 /user/cate/tsort640m/part-r-00004
-rw-r--r--   1 cate aussies   11004600 2017-05-04 21:39 /user/cate/tsort640m/part-r-00005
```

Supporting Scripts

```sh
[root@manager ~]# yum -y install krb5-server krb5-libs krb5-auth-dialog krb5-workstation
Loaded plugins: amazon-id, rhui-lb, search-disabled-repos
No package krb5-auth-dialog available.
Resolving Dependencies
--> Running transaction check
---> Package krb5-libs.x86_64 0:1.13.2-12.el7_2 will be updated
--> Processing Dependency: libkadm5clnt_mit.so.8()(64bit) for package: krb5-server-1.14.1-27.el7_3.x86_64
--> Processing Dependency: libkadm5clnt_mit.so.8()(64bit) for package: krb5-workstation-1.14.1-27.el7_3.x86_64
--> Processing Dependency: libkadm5clnt_mit.so.8(kadm5clnt_mit_8_MIT)(64bit) for package: krb5-server-1.14.1-27.el7_3.x86_64
--> Processing Dependency: libkadm5clnt_mit.so.8(kadm5clnt_mit_8_MIT)(64bit) for package: krb5-workstation-1.14.1-27.el7_3.x86_64
---> Package krb5-libs.x86_64 0:1.14.1-27.el7_3 will be an update
---> Package krb5-server.x86_64 0:1.14.1-27.el7_3 will be installed
rhui-REGION-rhel-server-releases/7Server/x86_64/filelists_db                                                                                                |  23 MB  00:00:00     
--> Processing Dependency: /usr/share/dict/words for package: krb5-server-1.14.1-27.el7_3.x86_64
cloudera-manager/filelists                                                                                                                                  | 122 kB  00:00:00     
rhui-REGION-client-config-server-7/x86_64/filelists_db                                                                                                      | 2.3 kB  00:00:00     
rhui-REGION-rhel-server-rh-common/7Server/x86_64/filelists_db                                                                                               |  94 kB  00:00:00     
--> Processing Dependency: libverto-module-base for package: krb5-server-1.14.1-27.el7_3.x86_64
---> Package krb5-workstation.x86_64 0:1.14.1-27.el7_3 will be installed
--> Running transaction check
---> Package libkadm5.x86_64 0:1.14.1-27.el7_3 will be installed
---> Package libverto-libevent.x86_64 0:0.2.5-4.el7 will be installed
--> Processing Dependency: libevent-2.0.so.5()(64bit) for package: libverto-libevent-0.2.5-4.el7.x86_64
---> Package words.noarch 0:3.0-22.el7 will be installed
--> Running transaction check
---> Package libevent.x86_64 0:2.0.21-4.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

===================================================================================================================================================================================
 Package                                   Arch                           Version                                   Repository                                                Size
===================================================================================================================================================================================
Installing:
 krb5-server                               x86_64                         1.14.1-27.el7_3                           rhui-REGION-rhel-server-releases                         977 k
 krb5-workstation                          x86_64                         1.14.1-27.el7_3                           rhui-REGION-rhel-server-releases                         772 k
Updating:
 krb5-libs                                 x86_64                         1.14.1-27.el7_3                           rhui-REGION-rhel-server-releases                         740 k
Installing for dependencies:
 libevent                                  x86_64                         2.0.21-4.el7                              rhui-REGION-rhel-server-releases                         214 k
 libkadm5                                  x86_64                         1.14.1-27.el7_3                           rhui-REGION-rhel-server-releases                         173 k
 libverto-libevent                         x86_64                         0.2.5-4.el7                               rhui-REGION-rhel-server-releases                         8.9 k
 words                                     noarch                         3.0-22.el7                                rhui-REGION-rhel-server-releases                         1.4 M

Transaction Summary
===================================================================================================================================================================================
Install  2 Packages (+4 Dependent packages)
Upgrade  1 Package

Total download size: 4.2 M
Downloading packages:
Delta RPMs disabled because /usr/bin/applydeltarpm not installed.
(1/7): krb5-libs-1.14.1-27.el7_3.x86_64.rpm                                                                                                                 | 740 kB  00:00:00     
(2/7): krb5-workstation-1.14.1-27.el7_3.x86_64.rpm                                                                                                          | 772 kB  00:00:00     
(3/7): libverto-libevent-0.2.5-4.el7.x86_64.rpm                                                                                                             | 8.9 kB  00:00:00     
(4/7): libkadm5-1.14.1-27.el7_3.x86_64.rpm                                                                                                                  | 173 kB  00:00:00     
(5/7): words-3.0-22.el7.noarch.rpm                                                                                                                          | 1.4 MB  00:00:00     
(6/7): libevent-2.0.21-4.el7.x86_64.rpm                                                                                                                     | 214 kB  00:00:00     
(7/7): krb5-server-1.14.1-27.el7_3.x86_64.rpm                                                                                                               | 977 kB  00:00:00     
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                              5.2 MB/s | 4.2 MB  00:00:00     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Updating   : krb5-libs-1.14.1-27.el7_3.x86_64                                                                                                                                1/8 
  Installing : libkadm5-1.14.1-27.el7_3.x86_64                                                                                                                                 2/8 
  Installing : words-3.0-22.el7.noarch                                                                                                                                         3/8 
  Installing : libevent-2.0.21-4.el7.x86_64                                                                                                                                    4/8 
  Installing : libverto-libevent-0.2.5-4.el7.x86_64                                                                                                                            5/8 
  Installing : krb5-server-1.14.1-27.el7_3.x86_64                                                                                                                              6/8 
  Installing : krb5-workstation-1.14.1-27.el7_3.x86_64                                                                                                                         7/8 
  Cleanup    : krb5-libs-1.13.2-12.el7_2.x86_64                                                                                                                                8/8 
  Verifying  : libverto-libevent-0.2.5-4.el7.x86_64                                                                                                                            1/8 
  Verifying  : libkadm5-1.14.1-27.el7_3.x86_64                                                                                                                                 2/8 
  Verifying  : krb5-libs-1.14.1-27.el7_3.x86_64                                                                                                                                3/8 
  Verifying  : krb5-workstation-1.14.1-27.el7_3.x86_64                                                                                                                         4/8 
  Verifying  : libevent-2.0.21-4.el7.x86_64                                                                                                                                    5/8 
  Verifying  : words-3.0-22.el7.noarch                                                                                                                                         6/8 
  Verifying  : krb5-server-1.14.1-27.el7_3.x86_64                                                                                                                              7/8 
  Verifying  : krb5-libs-1.13.2-12.el7_2.x86_64                                                                                                                                8/8 

Installed:
  krb5-server.x86_64 0:1.14.1-27.el7_3                                                  krb5-workstation.x86_64 0:1.14.1-27.el7_3                                                 

Dependency Installed:
  libevent.x86_64 0:2.0.21-4.el7             libkadm5.x86_64 0:1.14.1-27.el7_3             libverto-libevent.x86_64 0:0.2.5-4.el7             words.noarch 0:3.0-22.el7            

Updated:
  krb5-libs.x86_64 0:1.14.1-27.el7_3                                                                                                                                               

Complete!
[root@manager ~]# cat << 'EOF' > /var/kerberos/krb5kdc/kdc.conf
> [kdcdefaults]
>  kdc_ports = 88
>  kdc_tcp_ports = 88
> 
> [realms]
>  ALANBLUWOL.AU = {
>   #master_key_type = aes256-cts
>   acl_file = /var/kerberos/krb5kdc/kadm5.acl
>   dict_file = /usr/share/dict/words
>   admin_keytab = /var/kerberos/krb5kdc/kadm5.keytab
>   supported_enctypes = aes256-cts:normal aes128-cts:normal des3-hmac-sha1:normal arcfour-hmac:normal camellia256-cts:normal camellia128-cts:normal des-hmac-sha1:normal des-cbc-md5:normal des-cbc-crc:normal
>   max_life = 1d
>   max_renewable_life = 7d 
> }
> EOF

[root@manager ~]# cat /var/kerberos/krb5kdc/kdc.conf
[kdcdefaults]
 kdc_ports = 88
 kdc_tcp_ports = 88

[realms]
 ALANBLUWOL.AU = {
  #master_key_type = aes256-cts
  acl_file = /var/kerberos/krb5kdc/kadm5.acl
  dict_file = /usr/share/dict/words
  admin_keytab = /var/kerberos/krb5kdc/kadm5.keytab
  supported_enctypes = aes256-cts:normal aes128-cts:normal des3-hmac-sha1:normal arcfour-hmac:normal camellia256-cts:normal camellia128-cts:normal des-hmac-sha1:normal des-cbc-md5:normal des-cbc-crc:normal
  max_life = 1d
  max_renewable_life = 7d 
}
[root@manager ~]# cat << 'EOF' > /etc/krb5.conf
> # Configuration snippets may be placed in this directory as well
> includedir /etc/krb5.conf.d/
> 
> [logging]
>  default = FILE:/var/log/krb5libs.log
>  kdc = FILE:/var/log/krb5kdc.log
>  admin_server = FILE:/var/log/kadmind.log
> 
> [libdefaults]
>  dns_lookup_realm = false
>  ticket_lifetime = 24h
>  renew_lifetime = 7d
>  forwardable = true
>  rdns = false
>  default_realm = ALANBLUWOL.AU
>  default_ccache_name = KEYRING:persistent:%{uid}
>  udp_preference_limit = 1
>  default_tgs_enctypes = arcfour-hmac
>  default_tkt_enctypes = arcfour-hmac 
> 
> [realms]
>  ALANBLUWOL.AU = {
>   kdc = manager.alanbluwol
>   admin_server = manager.alanbluwol
>  }
> 
> [domain_realm]
>  .alanbluwol.au = ALANBLUWOL.AU
>  alanbluwol.au = ALANBLUWOL.AU
> EOF

[root@manager ~]# cat /etc/krb5.conf
# Configuration snippets may be placed in this directory as well
includedir /etc/krb5.conf.d/

[logging]
 default = FILE:/var/log/krb5libs.log
 kdc = FILE:/var/log/krb5kdc.log
 admin_server = FILE:/var/log/kadmind.log

[libdefaults]
 dns_lookup_realm = false
 ticket_lifetime = 24h
 renew_lifetime = 7d
 forwardable = true
 rdns = false
 default_realm = ALANBLUWOL.AU
 default_ccache_name = KEYRING:persistent:%{uid}
 udp_preference_limit = 1
 default_tgs_enctypes = arcfour-hmac
 default_tkt_enctypes = arcfour-hmac 

[realms]
 ALANBLUWOL.AU = {
  kdc = manager.alanbluwol
  admin_server = manager.alanbluwol
 }

[domain_realm]
 .alanbluwol.au = ALANBLUWOL.AU
 alanbluwol.au = ALANBLUWOL.AU
 
 [root@manager ~]# /usr/sbin/kdb5_util create -s
 Loading random data
 Initializing database '/var/kerberos/krb5kdc/principal' for realm 'ALANBLUWOL.AU',
 master key name 'K/M@ALANBLUWOL.AU'
 You will be prompted for the database Master Password.
 It is important that you NOT FORGET this password.
 Enter KDC database master key: 
 Re-enter KDC database master key to verify:
 
 [root@manager ~]# kadmin.local
 kadmin.local:  addprinc cloudera-scm@ALANBLUWOL.AU
 WARNING: no policy specified for cloudera-scm@ALANBLUWOL.AU; defaulting to no policy
 Enter password for principal "cloudera-scm@ALANBLUWOL.AU": 
 Re-enter password for principal "cloudera-scm@ALANBLUWOL.AU": 
 Principal "cloudera-scm@ALANBLUWOL.AU" created.
 
 [root@manager ~]# cat << 'EOF' > /var/kerberos/krb5kdc/kadm5.acl 
 > */admin@ALANBLUWOL.AU*
 > cloudera-scm@ALANBLUWOL.AU admilc
 > EOF
 
 [root@manager ~]# cat /var/kerberos/krb5kdc/kadm5.acl 
 */admin@ALANBLUWOL.AU      *
 cloudera-scm@ALANBLUWOL.AU admilc
 
 [root@manager ~]# kadmin.local
 Authenticating as principal root/admin@ALANBLUWOL.AU with password.
 kadmin.local:  addpol admin
 kadmin.local:  addpol users
 kadmin.local:  addpol hosts
 kadmin.local:  exit
 
 [root@manager ~]# service krb5kdc start
 Redirecting to /bin/systemctl start  krb5kdc.service
 [root@manager ~]# service krb5kdc status
 Redirecting to /bin/systemctl status  krb5kdc.service
 ● krb5kdc.service - Kerberos 5 KDC
    Loaded: loaded (/usr/lib/systemd/system/krb5kdc.service; disabled; vendor preset: disabled)
    Active: active (running) since Thu 2017-05-04 20:56:53 EDT; 22ms ago
   Process: 5146 ExecStart=/usr/sbin/krb5kdc -P /var/run/krb5kdc.pid $KRB5KDC_ARGS (code=exited, status=0/SUCCESS)
  Main PID: 5147 (krb5kdc)
    CGroup: /system.slice/krb5kdc.service
            └─5147 /usr/sbin/krb5kdc -P /var/run/krb5kdc.pid

 May 04 20:56:53 manager.alanbluwol systemd[1]: Starting Kerberos 5 KDC...
 May 04 20:56:53 manager.alanbluwol systemd[1]: Started Kerberos 5 KDC.
 [root@manager ~]# 
 [root@manager ~]# service kadmin start
 Redirecting to /bin/systemctl start  kadmin.service
 [root@manager ~]# service kadmin status
 Redirecting to /bin/systemctl status  kadmin.service
 ● kadmin.service - Kerberos 5 Password-changing and Administration
    Loaded: loaded (/usr/lib/systemd/system/kadmin.service; disabled; vendor preset: disabled)
    Active: active (running) since Thu 2017-05-04 20:56:53 EDT; 1s ago
   Process: 5174 ExecStart=/usr/sbin/_kadmind -P /var/run/kadmind.pid $KADMIND_ARGS (code=exited, status=0/SUCCESS)
  Main PID: 5175 (kadmind)
    CGroup: /system.slice/kadmin.service
            └─5175 /usr/sbin/kadmind -P /var/run/kadmind.pid

 May 04 20:56:53 manager.alanbluwol systemd[1]: Starting Kerberos 5 Password-changing and Administration...
 May 04 20:56:53 manager.alanbluwol systemd[1]: Started Kerberos 5 Password-changing and Administration.

[root@manager ~]# kadmin.local
Authenticating as principal root/admin@ALANBLUWOL.AU with password.
kadmin.local:  add_principal cate
WARNING: no policy specified for cate@ALANBLUWOL.AU; defaulting to no policy
Enter password for principal "cate@ALANBLUWOL.AU": 
Re-enter password for principal "cate@ALANBLUWOL.AU": 
Principal "cate@ALANBLUWOL.AU" created.
kadmin.local:  add_principal jemaine
WARNING: no policy specified for jemaine@ALANBLUWOL.AU; defaulting to no policy
Enter password for principal "jemaine@ALANBLUWOL.AU": 
Re-enter password for principal "jemaine@ALANBLUWOL.AU": 
Principal "jemaine@ALANBLUWOL.AU" created.
kadmin.local:  exit

# Enable Kerberos
# Experienced a number of issue for running through it too quickly
# Had to destroy database, rebuild, re-run enable process... fun...

[root@manager ~]# kadmin.local
Authenticating as principal root/admin@ALANBLUWOL.AU with password.
kadmin.local:  getprincs
HTTP/node1.alanbluwol@ALANBLUWOL.AU
HTTP/node2.alanbluwol@ALANBLUWOL.AU
HTTP/node3.alanbluwol@ALANBLUWOL.AU
HTTP/node4.alanbluwol@ALANBLUWOL.AU
K/M@ALANBLUWOL.AU
cate@ALANBLUWOL.AU
cloudera-scm@ALANBLUWOL.AU
hdfs/node1.alanbluwol@ALANBLUWOL.AU
hdfs/node2.alanbluwol@ALANBLUWOL.AU
hdfs/node3.alanbluwol@ALANBLUWOL.AU
hdfs/node4.alanbluwol@ALANBLUWOL.AU
hive/node1.alanbluwol@ALANBLUWOL.AU
hue/node1.alanbluwol@ALANBLUWOL.AU
impala/node1.alanbluwol@ALANBLUWOL.AU
impala/node2.alanbluwol@ALANBLUWOL.AU
impala/node3.alanbluwol@ALANBLUWOL.AU
impala/node4.alanbluwol@ALANBLUWOL.AU
jemaine@ALANBLUWOL.AU
kadmin/admin@ALANBLUWOL.AU
kadmin/changepw@ALANBLUWOL.AU
kadmin/manager.alanbluwol@ALANBLUWOL.AU
kiprop/manager.alanbluwol@ALANBLUWOL.AU
krbtgt/ALANBLUWOL.AU@ALANBLUWOL.AU
mapred/node1.alanbluwol@ALANBLUWOL.AU
oozie/node1.alanbluwol@ALANBLUWOL.AU
yarn/node1.alanbluwol@ALANBLUWOL.AU
yarn/node2.alanbluwol@ALANBLUWOL.AU
yarn/node3.alanbluwol@ALANBLUWOL.AU
yarn/node4.alanbluwol@ALANBLUWOL.AU
zookeeper/node1.alanbluwol@ALANBLUWOL.AU
zookeeper/node2.alanbluwol@ALANBLUWOL.AU
zookeeper/node3.alanbluwol@ALANBLUWOL.AU


```