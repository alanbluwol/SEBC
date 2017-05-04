# Services Enablement Boot Camp

# 0 - Setup

* List the cloud provider you are using (AWS, GCE, Azure, other)

	> AWS

* List the nodes you are using by IP address and name

```sh
challenge-cloudera-manager    13.55.126.110  172.31.2.184
challenge-cloudera-master     13.54.133.149  172.31.5.118
challenge-cloudera-worker-01  52.63.73.20    172.31.9.102
challenge-cloudera-worker-02  52.62.178.228  172.31.15.9
challenge-cloudera-worker-03  52.63.115.248  172.31.15.19
```

* List the Linux release you are using

	> Red Hat Enterprise Linux Server release 7.2 (Maipo)

* Demonstrate the disk capacity available on each node is >= 30 GB

	> Note: xvdb will be mounted to /opt as part of the challenge

```sh
[root@ip-172-31-2-184 ~]# df
Filesystem     1K-blocks    Used Available Use% Mounted on
/dev/xvda2      10473452 1686672   8786780  17% /
devtmpfs         8115408       0   8115408   0% /dev
tmpfs            8002596       0   8002596   0% /dev/shm
tmpfs            8002596   16640   7985956   1% /run
tmpfs            8002596       0   8002596   0% /sys/fs/cgroup
tmpfs            1600520       0   1600520   0% /run/user/1000
/dev/xvdb       30832636   45128  30787508   1% /data01
```

* List the command and output for yum repolist enabled

```sh
[root@ip-172-31-2-184 ~]# yum repolist enabled
Loaded plugins: amazon-id, rhui-lb, search-disabled-repos
repo id                                                                        repo name                                                                                     status
cloudera-manager                                                               Cloudera Manager                                                                                   7
rhui-REGION-client-config-server-7/x86_64                                      Red Hat Update Infrastructure 2.0 Client Configuration Server 7                                    6
rhui-REGION-rhel-server-releases/7Server/x86_64                                Red Hat Enterprise Linux Server 7 (RPMs)                                                      14,277
rhui-REGION-rhel-server-rh-common/7Server/x86_64                               Red Hat Enterprise Linux Server 7 RH Common (RPMs)                                               228
repolist: 14,518
```
* Add the following Linux accounts to all nodes

```sh
[root@ip-172-31-2-184 ~]# groupadd kiwis
[root@ip-172-31-2-184 ~]# groupadd aussies
[root@ip-172-31-2-184 ~]# useradd -u 2300 -g "aussies" cate
[root@ip-172-31-2-184 ~]# useradd -u 2900 -g "kiwis" jemaine
[root@ip-172-31-2-184 ~]# cat /etc/passwd | grep cate
cate:x:2300:1002::/home/cate:/bin/bash
[root@ip-172-31-2-184 ~]# cat /etc/passwd | grep jemaine
jemaine:x:2900:1001::/home/jemaine:/bin/bash
[root@ip-172-31-2-184 ~]# cat /etc/group | grep aussies
aussies:x:1002:
[root@ip-172-31-2-184 ~]# cat /etc/group | grep kiwis
kiwis:x:1001:
```

