# Services Enablement Boot Camp

# Lab 1: CDH Install

## 0 - Instantiate Hosts

### Instance Type

Red Hat Enterprise Linux 7.2
ami-91cdf0f2

### Security Group

* Authorize 
	* TCP ports 22, 7180, 7182, 7183, 7432, 8088
	* ICMP Echo Reply
	* TCP all ports to VPC Default Security Group
	* TCP all ports to VPC IP Range

## 1 - Prepare Hosts

```sh
sudo -i
```

### Memory Configurtion & Transparent Hugepage

Script

```sh
echo 1 > /proc/sys/vm/swappiness
echo "vm.swapiness = 1" >> /etc/sysctl.conf

echo never > /sys/kernel/mm/transparent_hugepage/defrag
echo never > /sys/kernel/mm/transparent_hugepage/enabled

echo "echo never > /sys/kernel/mm/transparent_hugepage/defrag" >> /etc/rc.d/rc.local
echo "echo never > /sys/kernel/mm/transparent_hugepage/enabled" >> /etc/rc.d/rc.local

grep AnonHugePages /proc/meminfo 
egrep 'trans|thp' /proc/vmstat
grep -i HugePages_Total /proc/meminfo 
cat /proc/sys/vm/nr_hugepages
sysctl vm.nr_hugepages

```

Output

```sh
[root@ip-172-31-1-85 ~]# grep AnonHugePages /proc/meminfo 
AnonHugePages:      6144 kB
[root@ip-172-31-1-85 ~]# egrep 'trans|thp' /proc/vmstat
nr_anon_transparent_hugepages 3
thp_fault_alloc 40
thp_fault_fallback 0
thp_collapse_alloc 1
thp_collapse_alloc_failed 0
thp_split 0
thp_zero_page_alloc 0
thp_zero_page_alloc_failed 0
[root@ip-172-31-1-85 ~]# grep -i HugePages_Total /proc/meminfo 
HugePages_Total:       0
[root@ip-172-31-1-85 ~]# cat /proc/sys/vm/nr_hugepages
0
[root@ip-172-31-1-85 ~]# sysctl vm.nr_hugepages
vm.nr_hugepages = 0
```

### Mount Data Drive

Script

```sh
lsblk
mkfs -t ext3 -m 0 /dev/xvdb
echo "/dev/xvdb /data01 ext3 defaults,noatime 0" >> /etc/fstab
mkdir /data01
mount /data01
mkdir /data01/opt
mv /opt/* /data01/opt
rmdir /opt
ln -s /data01/opt /opt
df
```

Ouput

```sh
[root@ip-172-31-1-85 ~]# lsblk
NAME    MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
xvda    202:0    0  20G  0 disk 
├─xvda1 202:1    0   1M  0 part 
└─xvda2 202:2    0  20G  0 part /
xvdb    202:16   0  20G  0 disk 

[root@ip-172-31-1-85 ~]# mkfs -t ext3 -m 0 /dev/xvdb
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
1310720 inodes, 5242880 blocks
0 blocks (0.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=4294967296
160 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks: 
	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208, 
	4096000

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done   

[root@ip-172-31-1-85 ~]# df
Filesystem     1K-blocks   Used Available Use% Mounted on
/dev/xvda2      20959212 948728  20010484   5% /
devtmpfs         8115408      0   8115408   0% /dev
tmpfs            8002596      0   8002596   0% /dev/shm
tmpfs            8002596  16636   7985960   1% /run
tmpfs            8002596      0   8002596   0% /sys/fs/cgroup
tmpfs            1600520      0   1600520   0% /run/user/1000
/dev/xvdb       20511356  45128  20466228   1% /data01
```

### Name Service Cache Daemon

Script

```sh
yum -y install nscd
service nscd start
chkconfig nscd on
service nscd status
```

Output

```sh
[root@ip-172-31-1-85 ~]# service nscd status
Redirecting to /bin/systemctl status  nscd.service
● nscd.service - Name Service Cache Daemon
   Loaded: loaded (/usr/lib/systemd/system/nscd.service; enabled; vendor preset: disabled)
   Active: active (running) since Mon 2017-05-01 01:22:06 EDT; 70ms ago
 Main PID: 9553 (nscd)
   CGroup: /system.slice/nscd.service
           └─9553 /usr/sbin/nscd

May 01 01:22:06 ip-172-31-1-85.ap-southeast-2.compute.internal nscd[9553]: 9553 monitoring file `/etc/hosts` (4)
May 01 01:22:06 ip-172-31-1-85.ap-southeast-2.compute.internal nscd[9553]: 9553 monitoring directory `/etc` (2)
May 01 01:22:06 ip-172-31-1-85.ap-southeast-2.compute.internal nscd[9553]: 9553 monitoring file `/etc/resolv.conf` (5)
May 01 01:22:06 ip-172-31-1-85.ap-southeast-2.compute.internal nscd[9553]: 9553 monitoring directory `/etc` (2)
May 01 01:22:06 ip-172-31-1-85.ap-southeast-2.compute.internal nscd[9553]: 9553 monitoring file `/etc/services` (6)
May 01 01:22:06 ip-172-31-1-85.ap-southeast-2.compute.internal nscd[9553]: 9553 monitoring directory `/etc` (2)
May 01 01:22:06 ip-172-31-1-85.ap-southeast-2.compute.internal nscd[9553]: 9553 disabled inotify-based monitoring for file `/etc/netgroup': No such file or directory
May 01 01:22:06 ip-172-31-1-85.ap-southeast-2.compute.internal nscd[9553]: 9553 stat failed for file `/etc/netgroup'; will try again later: No such file or directory
May 01 01:22:06 ip-172-31-1-85.ap-southeast-2.compute.internal nscd[9553]: 9553 Access Vector Cache (AVC) started
May 01 01:22:06 ip-172-31-1-85.ap-southeast-2.compute.internal systemd[1]: Started Name Service Cache Daemon.
```

### Network Time Protocol

Script

```sh
yum -y install ntp
service ntpd start
chkconfig ntpd on
service ntpd status
```

Output

```sh
[root@ip-172-31-1-85 ~]# service ntpd status
Redirecting to /bin/systemctl status  ntpd.service
● ntpd.service - Network Time Service
   Loaded: loaded (/usr/lib/systemd/system/ntpd.service; enabled; vendor preset: disabled)
   Active: active (running) since Mon 2017-05-01 01:22:08 EDT; 66ms ago
 Main PID: 9654 (ntpd)
   CGroup: /system.slice/ntpd.service
           └─9654 /usr/sbin/ntpd -u ntp:ntp -g

May 01 01:22:08 ip-172-31-1-85.ap-southeast-2.compute.internal ntpd[9654]: 0.0.0.0 c01d 0d kern kernel time sync enabled
May 01 01:22:08 ip-172-31-1-85.ap-southeast-2.compute.internal ntpd[9654]: ntp_io: estimated max descriptors: 1024, initial socket boundary: 16
May 01 01:22:08 ip-172-31-1-85.ap-southeast-2.compute.internal ntpd[9654]: Listen and drop on 0 v4wildcard 0.0.0.0 UDP 123
May 01 01:22:08 ip-172-31-1-85.ap-southeast-2.compute.internal ntpd[9654]: Listen and drop on 1 v6wildcard :: UDP 123
May 01 01:22:08 ip-172-31-1-85.ap-southeast-2.compute.internal ntpd[9654]: Listen normally on 2 lo 127.0.0.1 UDP 123
May 01 01:22:08 ip-172-31-1-85.ap-southeast-2.compute.internal ntpd[9654]: Listen normally on 3 eth0 172.31.1.85 UDP 123
May 01 01:22:08 ip-172-31-1-85.ap-southeast-2.compute.internal ntpd[9654]: Listen normally on 4 lo ::1 UDP 123
May 01 01:22:08 ip-172-31-1-85.ap-southeast-2.compute.internal ntpd[9654]: Listen normally on 5 eth0 fe80::5d:6dff:fea8:cfc1 UDP 123
May 01 01:22:08 ip-172-31-1-85.ap-southeast-2.compute.internal ntpd[9654]: Listening on routing socket on fd #22 for interface updates
May 01 01:22:08 ip-172-31-1-85.ap-southeast-2.compute.internal systemd[1]: Started Network Time Service.
```

### RPC Bind & NetCat Utility

Script

```sh
yum -y install rpcbind
service rpcbind start
chkconfig rpcbind on
service rpcbind status
```

Output

```sh
[root@ip-172-31-1-85 ~]# service rpcbind status
Redirecting to /bin/systemctl status  rpcbind.service
● rpcbind.service - RPC bind service
   Loaded: loaded (/usr/lib/systemd/system/rpcbind.service; indirect; vendor preset: enabled)
   Active: active (running) since Mon 2017-05-01 01:22:09 EDT; 248ms ago
 Main PID: 9750 (rpcbind)
   CGroup: /system.slice/rpcbind.service
           └─9750 /sbin/rpcbind -w

May 01 01:22:09 ip-172-31-1-85.ap-southeast-2.compute.internal systemd[1]: Starting RPC bind service...
May 01 01:22:09 ip-172-31-1-85.ap-southeast-2.compute.internal systemd[1]: Started RPC bind service.
May 01 01:22:09 ip-172-31-1-85.ap-southeast-2.compute.internal systemd[1]: Started RPC bind service.
```

### PAM Limits

Script

```sh
echo "hdfs - nofile 32768" >> /etc/security/limits.conf
echo "mapred - nofile 32768" >> /etc/security/limits.conf
echo "hbase - nofile 32768" >> /etc/security/limits.conf
echo "hdfs - noproc 32768" >> /etc/security/limits.conf
echo "mapred - noproc 32768" >> /etc/security/limits.conf
echo "hbase - noproc 32768" >> /etc/security/limits.conf

cat /etc/security/limits.conf
```

### Host Configuration

Script

```sh
# echo "127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4" > /etc/hosts
# echo "::1         localhost localhost.localdomain localhost6 localhost6.localdomain6" >> /etc/hosts
# echo "172.31.5.116 node1.alanbluwol node1 master1 ip-172-31-5-116" >> /etc/hosts
# echo "172.31.7.189 node2.alanbluwol node2 worker1 ip-172-31-7-189" >> /etc/hosts
# echo "172.31.13.46 node3.alanbluwol node3 worker2 ip-172-31-13-46" >> /etc/hosts
# echo "172.31.15.253 node4.alanbluwol node4 worker3 ip-172-31-15-253" >> /etc/hosts

echo "172.31.2.184 manager.alanbluwol manager manager ip-172-31-2-184 " >> /etc/hosts
echo "172.31.5.118 node1.alanbluwol   node1   master  ip-172-31-5-118 " >> /etc/hosts
echo "172.31.9.102 node2.alanbluwol   node2   worker1 ip-172-31-9-102 " >> /etc/hosts
echo "172.31.15.9  node3.alanbluwol   node3   worker2 ip-172-31-15-9  " >> /etc/hosts
echo "172.31.15.19 node4.alanbluwol   node4   worker3 ip-172-31-15-19 " >> /etc/hosts

getent hosts
```

Output

```sh
[root@ip-172-31-1-85 ~]# getent hosts
127.0.0.1       localhost localhost.localdomain localhost4 localhost4.localdomain4
127.0.0.1       localhost localhost.localdomain localhost6 localhost6.localdomain6
172.31.5.116    node1.alanbluwol node1 master1 ip-172-31-5-116
172.31.7.189    node2.alanbluwol node2 worker1 ip-172-31-7-189
172.31.13.46    node3.alanbluwol node3 worker2 ip-172-31-13-46
172.31.15.253   node4.alanbluwol node4 worker3 ip-172-31-15-253
```

### Security-Enhanced Linux Disabled

Script

```sh
sed -i 's/enforcing/disabled/g' /etc/selinux/config
setenforce 0
getenforce

cat /selinux/enforce
cat /etc/selinux/config
```

Output

```sh
[root@ip-172-31-1-85 ~]# getenforce
Permissive

[root@ip-172-31-1-85 ~]# cat /etc/selinux/config
/# This file controls the state of SELinux on the system.
/# SELINUX= can take one of these three values:
/#     enforcing - SELinux security policy is enforced.
/#     permissive - SELinux prints warnings instead of enforcing.
/#     disabled - No SELinux policy is loaded.
SELINUX=enforcing
/# SELINUXTYPE= can take one of three two values:
/#     targeted - Targeted processes are protected,
/#     minimum - Modification of targeted policy. Only selected processes are protected. 
/#     mls - Multi Level Security protection.
SELINUXTYPE=targeted
```

### Networking Configuration (On each node)

Script

```sh
HOST_NAME=node1.alanbluwol

hostname $HOST_NAME

echo "DHCP_HOSTNAME=$HOST_NAME" >> /etc/sysconfig/network-scripts/ifcfg-eth0
echo "HOSTNAME=$HOST_NAME" >> /etc/sysconfig/network

hostname
cat /etc/sysconfig/network-scripts/ifcfg-eth0
cat /etc/sysconfig/network
```
Output

```sh
[root@ip-172-31-5-116 ~]# hostname
node1.alanbluwol
[root@ip-172-31-5-116 ~]# cat /etc/sysconfig/network-scripts/ifcfg-eth0
DEVICE="eth0"
BOOTPROTO="dhcp"
ONBOOT="yes"
TYPE="Ethernet"
USERCTL="yes"
PEERDNS="yes"
IPV6INIT="no"
DHCP_HOSTNAME=node1.alanbluwol
[root@ip-172-31-5-116 ~]# cat /etc/sysconfig/network
NETWORKING=yes
NOZEROCONF=yes
HOSTNAME=node1.alanbluwol
```

### Install Perl, WGET, Python

Script

```sh
yum -y --skip-broken update
yum -y install perl wget
```

Notes

	> Optional:
	> wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
	> rpm -ivh epel-release-latest-7.noarch.rpm
	> yum -y install epel-release python-pip
	> pip install --upgrade pip
	> pip install psycopg2

### MariaDB / MySQL

Script

```sh
yum -y install mariadb-server
service mariadb stop

mv /var/lib/mysql/ib_logfile0 ~/
mv /var/lib/mysql/ib_logfile1 ~/

chkconfig mariadb on
service mariadb start
service mariadb status

/usr/bin/mysql_secure_installation
```

Notes

	> Options:
	> Password: cloudera
	> n Set password protection for the server
	> y Revoke permissions for anonymous users
	> n Permit remote privileged login
	> y Remove test databases
	> y Refresh privileges in memory

Output

```sh
[root@ip-172-31-1-85 log]# service mariadb status
Redirecting to /bin/systemctl status  mariadb.service
● mariadb.service - MariaDB database server
   Loaded: loaded (/usr/lib/systemd/system/mariadb.service; enabled; vendor preset: disabled)
   Active: active (running) since Mon 2017-05-01 01:50:41 EDT; 1min 40s ago
  Process: 20995 ExecStartPost=/usr/libexec/mariadb-wait-ready $MAINPID (code=exited, status=0/SUCCESS)
  Process: 20966 ExecStartPre=/usr/libexec/mariadb-prepare-db-dir %n (code=exited, status=0/SUCCESS)
 Main PID: 20994 (mysqld_safe)
   CGroup: /system.slice/mariadb.service
           ├─20994 /bin/sh /usr/bin/mysqld_safe --basedir=/usr
           └─21414 /usr/libexec/mysqld --basedir=/usr --datadir=/var/lib/mysql --plugin-dir=/usr/lib64/mysql/plugin --log-error=/var/lib/mysql/ip-172-31-1-85.ap-southe...

May 01 01:50:39 ip-172-31-1-85.ap-southeast-2.compute.internal systemd[1]: Starting MariaDB database server...
May 01 01:50:39 ip-172-31-1-85.ap-southeast-2.compute.internal mysqld_safe[20994]: 170501 01:50:39 mysqld_safe Logging to '/var/lib/mysql/ip-172-31-1-85.ap-south....err'.
May 01 01:50:39 ip-172-31-1-85.ap-southeast-2.compute.internal mysqld_safe[20994]: 170501 01:50:39 mysqld_safe Starting mysqld daemon with databases from /var/lib/mysql
May 01 01:50:41 ip-172-31-1-85.ap-southeast-2.compute.internal systemd[1]: Started MariaDB database server.
Hint: Some lines were ellipsized, use -l to show in full.
```

### MySQL Connector

Script

```sh
wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.41.tar.gz
tar -zxvf mysql-connector-java-5.1.41.tar.gz
mkdir -p /usr/share/java/
cp mysql-connector-java-5.1.41/mysql-connector-java-5.1.41-bin.jar /usr/share/java/mysql-connector-java.jar
```

### Oracle JDK

Script

```sh
yum-config-manager --add-repo http://archive.cloudera.com/cm5/redhat/7/x86_64/cm/cloudera-manager.repo
yum -y install oracle-j2sdk1.7
```

### Passwordless SSH - Cloudera Manager & Cloudera Master

#### On Manager & Primary Node

Script

```sh
ssh-keygen -t rsa
cat ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub
```

Notes

	> Options: enter, enter, enter
	> Save output to local pem file to upload to Cloudera Manager Install
	> Copy the key

#### On Other Nodes

Script

```sh
echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDcAPxi7OVurlZI9kfM4FIOuKnvXUgBpchjfXf9ZSP+nHLytSZTdkGONj4Fk/V1Bh9RQkuGkkTYnA6HE74BxrHwRBGFn4MlPFPnPNSGNJgx5JOTmhyOw9JWFbIAsPrypqp3DxEJvZPCOnmrc/POxJARZtRmfxhvnbYkmGp6lV3fSumnbzDbjSvMH9/IjBU2PGwRiITKyPe3Ok+4ATf+ika+phSAFE0dKtt4BdtJavUq0nVMXhULbPhbxxxeN49rhz2OqEnpryw/RrtLEbpRos8whDk7NXdfI5Oson1hxYd1IolSBYGqh+rkHCjm+fdO60fu2yPGbFopn6mx1IVIb/4l root@ip-172-31-1-85.ap-southeast-2.compute.internal" >> ~/.ssh/authorized_keys
echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCtP+L9vb2RbUEU2lSvTPjN3nWWZGdkGkhUaJopEoN6KPLz6D1zyFjNYezsAU8JH3QJfdKN6RP5I3qtHS0ldrqndKAkU/eC7VeRrHTm1TSVI3z7x7RU1yZCwd3RoS1CiuqV5GhKa3WFHjyl3ex6Ak5Xqy6pRuLcdTqpRwv6dvWAITuUsXpk2sVVXochQGvkd1EguvsZ7SGlI6DLeghmlHzdRgsqhfKQ993Zb+75YAP+wRU+Wyof/keA3AlHnYiYJPoLtDWUk/qT39MmgDEvUP3kgUWXVPZN5HTWYqJtTmPuiUjZNWEz4g27K+Tfw6CZi3tO3og40MMcLz4XHT+org49 root@node1.alanbluwol" >> ~/.ssh/authorized_keys

vi ~/.ssh/authorized_keys
```

Notes

	> SSH to other hosts
	> Redhat: 157x - to remove root access
	> Centos: 155x - to remove root access
	> escape i - to insert
	> paste public key
	> escape wq!

Output

```sh
[root@ip-172-31-1-85 log]# ssh root@node1
The authenticity of host 'node1 (172.31.5.116)' can't be established.
ECDSA key fingerprint is 4c:11:9c:ea:83:5e:01:ed:c2:48:62:f2:60:11:e3:12.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'node1,172.31.5.116' (ECDSA) to the list of known hosts.
[root@node1 ~]# exit
logout
Connection to node1 closed.
[root@ip-172-31-1-85 log]# ssh root@node2
The authenticity of host 'node2 (172.31.7.189)' can't be established.
ECDSA key fingerprint is e1:7b:8d:eb:89:b3:26:24:db:5c:e0:61:48:cb:3e:07.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'node2,172.31.7.189' (ECDSA) to the list of known hosts.
[root@node2 ~]# exit
logout
Connection to node2 closed.
[root@ip-172-31-1-85 log]# ssh root@node3
The authenticity of host 'node3 (172.31.13.46)' can't be established.
ECDSA key fingerprint is fa:45:30:b5:ac:3e:94:40:c0:dd:0c:ac:da:23:60:b8.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'node3,172.31.13.46' (ECDSA) to the list of known hosts.
[root@node3 ~]# exit
logout
Connection to node3 closed.
[root@ip-172-31-1-85 ~]# ssh root@node4
The authenticity of host 'node4 (172.31.15.253)' can't be established.
ECDSA key fingerprint is be:36:14:85:d1:84:35:56:12:da:8d:dd:f5:09:cb:ea.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'node4,172.31.15.253' (ECDSA) to the list of known hosts.
[root@node4 ~]# exit
logout
Connection to node4 closed.
```

## 2 - Prepare Database

### Primary Node

Script

```sh
cp /etc/my.cnf ~/bk.my.cnf

cat << 'EOF' > /etc/my.cnf
[mysqld]
server_id=198
log-bin=mysqld-bin
bind-address=0.0.0.0
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
default-storage-engine=innodb
sql_mode=STRICT_ALL_TABLES
key_buffer_size = 32M
max_allowed_packet = 32M
thread_stack = 256K
thread_cache_size = 64
query_cache_limit = 8M
query_cache_size = 64M
query_cache_type = 1
max_connections = 550
binlog_format = row
read_buffer_size = 2M
read_rnd_buffer_size = 16M
sort_buffer_size = 8M
join_buffer_size = 8M
symbolic-links=0
innodb_file_per_table = 1
innodb_flush_log_at_trx_commit = 2
innodb_log_buffer_size = 64M
innodb_buffer_pool_size = 4G
innodb_thread_concurrency = 8
innodb_flush_method = O_DIRECT

[mysqld_safe]
EOF

service mariadb restart

mysql -u root -pcloudera
```

Script

```sql
SET GLOBAL server_id=198;
grant replication slave on *.* to 'root'@'node1.alanbluwol' identified by 'cloudera';
grant replication slave on *.* to 'root'@'node2.alanbluwol' identified by 'cloudera';
set global binlog_format = 'ROW';
flush tables with read lock;
unlock tables;
show variables like 'server_id';
show variables like 'binlog_format';
SHOW MASTER STATUS \G
create database scm; -- Cloudera Manager
create database hive; -- Hive Metastore
create database hue; -- Hue Metastore
create database rman; -- Reports Manager
create database oozie; -- Oozie database
create database sentry; -- Sentry database
create user cloudera identified by 'cloudera';
grant all on *.* to 'cloudera'@'%' identified by 'cloudera';
exit
```

Output

```sh
MariaDB [(none)]> SET GLOBAL server_id=198;
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> grant replication slave on *.* to 'root'@'node1.alanbluwol' identified by 'cloudera';
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> grant replication slave on *.* to 'root'@'node2.alanbluwol' identified by 'cloudera';
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> set global binlog_format = 'ROW';
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> flush tables with read lock;
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> unlock tables;
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> show variables like 'server_id';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| server_id     | 198   |
+---------------+-------+
1 row in set (0.00 sec)

MariaDB [(none)]> show variables like 'binlog_format';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| binlog_format | ROW   |
+---------------+-------+
1 row in set (0.01 sec)

MariaDB [(none)]> SHOW MASTER STATUS \G
Empty set (0.01 sec)

MariaDB [(none)]> create database scm; -- Cloudera Manager
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> create database HMS; -- Hive Metastore
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> create database huey; -- Hue Metastore
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> create database repman; -- Reports Manager
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> create database works; -- Oozie database
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> create database sentry; -- Sentry database
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> create user cloudera identified by 'cloudera';
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> grant all on *.* to 'cloudera'@'%' identified by 'cloudera';
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> exit
Bye
```

Script

```sh
mysql --version
mysql -uroot -pcloudera -e "SELECT * FROM information_schema.user_privileges WHERE privilege_type = 'usage';"
mysql -uroot -pcloudera -e "SELECT user, host FROM mysql.user;"
mysql -uroot -pcloudera -e "SHOW DATABASES;"
```

Output

```sh
[root@ip-172-31-5-116 ~]# mysql --version
mysql  Ver 15.1 Distrib 5.5.52-MariaDB, for Linux (x86_64) using readline 5.1
[root@ip-172-31-5-116 ~]# mysql -uroot -pcloudera -e "SELECT * FROM information_schema.user_privileges WHERE privilege_type = 'usage';"
[root@ip-172-31-5-116 ~]# mysql -uroot -pcloudera -e "SELECT user, host FROM mysql.user;"
+------+------------------+
| user | host             |
+------+------------------+
| root | 127.0.0.1        |
| root | ::1              |
| root | localhost        |
| root | node1.alanbluwol |
+------+------------------+
[root@ip-172-31-5-116 ~]# mysql -uroot -pcloudera -e "SHOW DATABASES;"
+--------------------+
| Database           |
+--------------------+
| information_schema |
| HMS                |
| huey               |
| mysql              |
| performance_schema |
| repman             |
| scm                |
| sentry             |
| works              |
+--------------------+
```

### Secondary Node

Script

```sh
cp /etc/my.cnf ~/bk.my.cnf

cat << 'EOF' > /etc/my.cnf
[mysqld]
server_id=199
log-bin=mysqld-bin
bind-address=0.0.0.0
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
default-storage-engine=innodb
sql_mode=STRICT_ALL_TABLES
key_buffer_size = 32M
max_allowed_packet = 32M
thread_stack = 256K
thread_cache_size = 64
query_cache_limit = 8M
query_cache_size = 64M
query_cache_type = 1
max_connections = 550
binlog_format = row
read_buffer_size = 2M
read_rnd_buffer_size = 16M
sort_buffer_size = 8M
join_buffer_size = 8M
symbolic-links=0
innodb_file_per_table = 1
innodb_flush_log_at_trx_commit = 2
innodb_log_buffer_size = 64M
innodb_buffer_pool_size = 4G
innodb_thread_concurrency = 8
innodb_flush_method = O_DIRECT

[mysqld_safe]
EOF

service mariadb restart

mysql -u root -pcloudera
```

Script

```sql
SET GLOBAL server_id=199;
change master to MASTER_HOST='node1.alanbluwol', MASTER_USER='root', MASTER_PASSWORD='cloudera', MASTER_LOG_FILE='mysqld-bin.000001', MASTER_LOG_POS=0;
START SLAVE;
SHOW SLAVE STATUS \G
exit
```

Output

```sh
MariaDB [(none)]> show variables like 'server_id';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| server_id     | 199   |
+---------------+-------+
1 row in set (0.00 sec)

MariaDB [(none)]> change master to MASTER_HOST='node1.alanbluwol', MASTER_USER='root', MASTER_PASSWORD='cloudera', MASTER_LOG_FILE='mysqld-bin.000001', MASTER_LOG_POS=0;
Query OK, 0 rows affected (0.02 sec)

MariaDB [(none)]> START SLAVE;
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> SHOW SLAVE STATUS \G
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: node1.alanbluwol
                  Master_User: root
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysqld-bin.000001
          Read_Master_Log_Pos: 245
               Relay_Log_File: node2-relay-bin.000003
                Relay_Log_Pos: 530
        Relay_Master_Log_File: mysqld-bin.000001
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB: 
          Replicate_Ignore_DB: 
           Replicate_Do_Table: 
       Replicate_Ignore_Table: 
      Replicate_Wild_Do_Table: 
  Replicate_Wild_Ignore_Table: 
                   Last_Errno: 0
                   Last_Error: 
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 245
              Relay_Log_Space: 824
              Until_Condition: None
               Until_Log_File: 
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File: 
           Master_SSL_CA_Path: 
              Master_SSL_Cert: 
            Master_SSL_Cipher: 
               Master_SSL_Key: 
        Seconds_Behind_Master: 0
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 0
                Last_IO_Error: 
               Last_SQL_Errno: 0
               Last_SQL_Error: 
  Replicate_Ignore_Server_Ids: 
             Master_Server_Id: 198
1 row in set (0.00 sec)
```

## 3 - Install Cloudera Manager

### Cloudera Manager

Script

```sh
vi /etc/yum.repos.d/cloudera-manager.repo 
/# baseurl=https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/5.9.2/

yum -y install cloudera-manager-daemons cloudera-manager-server

/usr/share/cmf/schema/scm_prepare_database.sh -h node1.alanbluwol mysql scm cloudera cloudera
cat /etc/cloudera-scm-server/db.properties
```

Output

```sh
[root@ip-172-31-1-85 log]# /usr/share/cmf/schema/scm_prepare_database.sh -h node1.alanbluwol mysql scm cloudera cloudera
JAVA_HOME=/usr/java/jdk1.7.0_67-cloudera
Verifying that we can write to /etc/cloudera-scm-server
Creating SCM configuration file in /etc/cloudera-scm-server
Executing:  /usr/java/jdk1.7.0_67-cloudera/bin/java -cp /usr/share/java/mysql-connector-java.jar:/usr/share/java/oracle-connector-java.jar:/usr/share/cmf/schema/../lib/* com.cloudera.enterprise.dbutil.DbCommandExecutor /etc/cloudera-scm-server/db.properties com.cloudera.cmf.db.
[                          main] DbCommandExecutor              INFO  Successfully connected to database.
All done, your SCM database is configured correctly!
[root@ip-172-31-1-85 log]# cat /etc/cloudera-scm-server/db.properties
/# Auto-generated by scm_prepare_database.sh on Mon May  1 02:22:53 EDT 2017
/#
/# For information describing how to configure the Cloudera Manager Server
/# to connect to databases, see the "Cloudera Manager Installation Guide."
/#
com.cloudera.cmf.db.type=mysql
com.cloudera.cmf.db.host=node1.alanbluwol
com.cloudera.cmf.db.name=scm
com.cloudera.cmf.db.user=cloudera
com.cloudera.cmf.db.password=cloudera
com.cloudera.cmf.db.setupType=EXTERNAL
```

## 4 - Start Cloudera Manager

Script

```sh
service cloudera-scm-server start
chkconfig cloudera-scm-server on
tail -f /var/log/cloudera-scm-server/cloudera-scm-server.log

head -1 /var/log/cloudera-scm-server/cloudera-scm-server.log
cat /var/log/cloudera-scm-server/cloudera-scm-server.log | grep "Started Jetty server"
```

Output

```sh
[root@ip-172-31-1-85 log]# service cloudera-scm-server start
Starting cloudera-scm-server (via systemctl):              [  OK  ]
```

### On all nodes re-run

```sh
echo 1 > /proc/sys/vm/swappiness
echo "vm.swapiness = 1" >> /etc/sysctl.conf
```

Notes

	> Goto: http://52.64.211.37:7180/
	> User: admin
	> Password: cloudera
	> Redhat: 172.31.5.116, 172.31.7.189, 172.31.13.46, 172.31.15.253

## 5 - Configure the Cluster

* Do not apply Single User Mode
* Use only Cloudera's standard repositories
* Ignore all wizard steps marked "(Optional)" in the docs
* Install the Data Hub Edition
* Install CDH using parcels
* Rename your cluster to your GitHub name
* Enable the Coreset of CDH services (HDFS, Yarn, Hive, Oozie, Zookeeper, Spark, Sentry - Hue has a MySQL issue in this version)
* Deploy three ZooKeeper instances
* Once you have a healthy cluster, take a screenshot of your home page
	* Name the file installation/3_cm_installed.png
* Mark your Issue 'submitted' if you won't attempt the Bonus lab

## 6 - HDFS Testing


### Create End User

Script - Labs

```sh
useradd -u 2200 -g "hdfs" alanbluwol

cat /etc/passwd
cat /etc/group
```

Script - Challenges

```sh
yum repolist enabled

groupadd group1
groupadd group2
useradd -u 2200 -g "group1" user1
useradd -u 2300 -g "group2" user2

cat /etc/passwd | grep user
cat /etc/group | grep group

su - hdfs
hdfs dfs -mkdir /user/user1
hdfs dfs -mkdir /user/user2
hdfs dfs -chown user1:group1 /user/user1
hdfs dfs -chown user2:group2 /user/user2
hdfs dfs -ls /user/
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
time hadoop jar $HADOOP_PATH/hadoop-test-mr1.jar TestDFSIO -write -nrFiles 10 -fileSize 100
hdfs dfs -du -s -h /benchmarks/TestDFSIO
hdfs dfs -ls /benchmarks/TestDFSIO
time hadoop jar $HADOOP_PATH/hadoop-test-mr1.jar TestDFSIO -clean

time hadoop jar $HADOOP_PATH/hadoop-examples.jar teragen -D mapred.map.tasks=4 -D dfs.block.size=33554432 100000000 /alanbluwol/test
hdfs dfs -du -s -h /alanbluwol/test
```

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

/# On Master
iostat -t 1 100
vmstat -t 1 100


/# On Worker
iostat

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
hdfs dfs -rm -R /precious/SEBC-Sydney.zip
hdfs dfs -ls /precious/
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

## Restore from Snapshot

Refer to PNG images uploaded

	> 2_snapshot_list

## Test Restored Snapshot

Script - Master

```sh
hdfs dfs -ls /precious
```

Output

```sh
[hdfs@node1 ~]$ hdfs dfs -ls /precious/
Found 1 items
-rw-r--r--   3 hdfs supergroup     474169 2017-05-02 00:01 /precious/SEBC-Sydney.zip
```



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

Output
```sh
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

# Lab 3: High Availability

## Create Directory

Script

```sh
mkdir /data01/opt/cloudera/jn
chmod 777 /data01/opt/cloudera/jn
```

	> Add JournalNode Role to three hosts
	> Restart HDFS
	

## Add Journal Nodes

Done via Cloudera Manager

## Enable High Availability

Via Cloudera Manager

See screenshot PNG

## Post Configuration Steps

* The following manual steps must be performed after completing this wizard:
	* Configure the HDFS Web Interface Role of Hue service(s) Hue to be an HTTPFS role instead of a NameNode. Documentation
	* For each of the Hive service(s) Hive, stop the Hive service, back up the Hive Metastore Database to a persistent store, run the service command "Update Hive Metastore NameNodes", then restart the Hive services.

## Clean up HDFS Trash

Script

```sh
hadoop fs -rm -R /user/hdfs/.Trash/Current/*
hadoop fs -rm -R /user/hdfs/.Trash/170501220157/*
```

## 0 - YARN Test

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


## 0 - CM Monitoring Lab

### Questions

* What is ubertask optimization?

	> A YARN configuration parameter
	> By default this is disabled
	> Whether to enable ubertask optimization, which runs "sufficiently small" jobs sequentially within a single JVM. "Small" is defined by the mapreduce.job.ubertask.maxmaps, mapreduce.job.ubertask.maxreduces, and mapreduce.job.ubertask.maxbytes settings.

* Where in CM is the Kerberos Security Realm value displayed?

	> Administration > Settings > Kerberos
	> Default: HADOOP.COM

* Which CDH service(s) host a property for enabling Kerberos authentication?

	> Zookeeper
	> YARN
	> HDFS

* How do you upgrade the CM agents?

	> Upgrade the Cloudera Manager Agent. You can use an upgrade wizard that is invoked when you connect to the Admin Console or manually install the Cloudera Manager Agent packages.

* Give the tsquery statement used to chart Hue's CPU utilization?

```sql
select cpu_system_rate + cpu_user_rate 
where category=ROLE 
and serviceName=$SERVICENAME
-- $SERVICENAME = hue  $CLUSTERID = 1
```

* Name all the roles that make up the Hive service

	> HMS  Hive Metastore Server × 1
	> WHCS  WebHCat Server
	> HS2  HiveServer2 × 1
	> G  Gateway × 4

* What steps must be completed before integrating Cloudera Manager with Kerberos?

	> Prerequisites - These instructions assume you know how to install and configure Kerberos, you already have a working Kerberos key distribution center (KDC) and realm setup, and that you've installed the following Kerberos client packages on all cluster hosts and hosts that will be used to access the cluster, depending on the OS in use.


## 0 - CM Security


### Add Manager to all Node Hosts file

Script

```sh
sudo -i
echo "172.31.1.85  manager.alanbluwol node5 manager ip-172-31-1-85" >> /etc/hosts
getent hosts
```

Output

```sh
[root@node4 ~]# getent hosts
127.0.0.1       localhost localhost.localdomain localhost4 localhost4.localdomain4
127.0.0.1       localhost localhost.localdomain localhost6 localhost6.localdomain6
172.31.5.116    node1.alanbluwol node1 master1 ip-172-31-1-226
172.31.7.189    node2.alanbluwol node2 worker1 ip-172-31-6-72
172.31.13.46    node3.alanbluwol node3 worker2 ip-172-31-10-115
172.31.15.253   node4.alanbluwol node4 worker3 ip-172-31-11-75
172.31.1.85     manager.alanbluwol node5 manager ip-172-31-1-85
```

### Stop Cloudera Cluster Services

In Cloudera Manager, stop all services

### Install Kerberos 

Script - Cloudera Manager

```sh
yum -y install krb5-server krb5-libs krb5-auth-dialog krb5-workstation
```

Issue

	> No package krb5-auth-dialog available.

Output

```sh
Running transaction
  Installing : words-3.0-22.el7.noarch                      1/5 
  Installing : libevent-2.0.21-4.el7.x86_64                 2/5 
  Installing : libverto-libevent-0.2.5-4.el7.x86_64         3/5 
  Installing : krb5-server-1.14.1-27.el7_3.x86_64           4/5 
  Installing : krb5-workstation-1.14.1-27.el7_3.x86_64      5/5 
```

Script - Cloudera Master & Workers

```sh
yum -y install krb5-workstation krb5-libs krb5-auth-dialog
```

### Configure Kerberos

Script - Server

```sh
vi /var/kerberos/krb5kdc/kdc.conf
```

Outputs - Server

```sh
[root@ip-172-31-1-85 ~]# cat /var/kerberos/krb5kdc/kdc.conf
[kdcdefaults]
 kdc_ports = 88
 kdc_tcp_ports = 88

[realms]
 CLOUDERA.COM = {
  #master_key_type = aes256-cts
  acl_file = /var/kerberos/krb5kdc/kadm5.acl
  dict_file = /usr/share/dict/words
  admin_keytab = /var/kerberos/krb5kdc/kadm5.keytab
  supported_enctypes = aes256-cts:normal aes128-cts:normal des3-hmac-sha1:normal arcfour-hmac:normal camellia256-cts:normal camellia128-cts:normal des-hmac-sha1:normal des-cbc-md5:normal des-cbc-crc:normal
  max_life = 1d
  max_renewable_life = 7d 
}
```

Script - Server & Clients

```sh
cat << 'EOF' > /etc/krb5.conf
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
 default_realm = CLOUDERA.COM
 default_ccache_name = KEYRING:persistent:%{uid}
 udp_preference_limit = 1
 default_tgs_enctypes = arcfour-hmac
 default_tkt_enctypes = arcfour-hmac 

[realms]
 CLOUDERA.COM = {
  kdc = manager.alanbluwol
  admin_server = manager.alanbluwol
 }

[domain_realm]
 .cloudera.com = CLOUDERA.COM
 cloudera.com = CLOUDERA.COM
EOF
cat /etc/krb5.conf
```

Outputs - Clients

```sh
[root@node1 ~]# cat /etc/krb5.conf
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
 default_realm = CLOUDERA.COM
 default_ccache_name = KEYRING:persistent:%{uid}
 udp_preference_limit = 1
 default_tgs_enctypes = arcfour-hmac
 default_tkt_enctypes = arcfour-hmac 

[realms]
 CLOUDERA.COM = {
  kdc = manager.alanbluwol
  admin_server = manager.alanbluwol
 }

[domain_realm]
 .cloudera.com = CLOUDERA.COM
 cloudera.com = CLOUDERA.COM
```

### Create Kerberos Database

Script - Server

```sh
/usr/sbin/kdb5_util create -s
# kdb5_util destroy # destroy created database in case of error
```

Outputs

```sh
[root@manager ~]# /usr/sbin/kdb5_util create -s
Loading random data
Initializing database '/var/kerberos/krb5kdc/principal' for realm 'CLOUDERA.COM',
master key name 'K/M@CLOUDERA.COM'
You will be prompted for the database Master Password.
It is important that you NOT FORGET this password.
Enter KDC database master key: 
Re-enter KDC database master key to verify: 
```

### Add Principal

Script - Server

```sh
kadmin.local
addprinc cloudera-scm@CLOUDERA.COM
```

Outputs

```sh
kadmin.local:  addprinc cloudera-scm@CLOUDERA.COM
WARNING: no policy specified for cloudera-scm@CLOUDERA.COM; defaulting to no policy
Enter password for principal "cloudera-scm@CLOUDERA.COM": 
Re-enter password for principal "cloudera-scm@CLOUDERA.COM": 
Principal "cloudera-scm@CLOUDERA.COM" created.
kadmin.local:  exit
```

### Configure ACL

Script - Server

```sh
vi /var/kerberos/krb5kdc/kadm5.acl 
kadmin.local
addpol admin
addpol users
addpol hosts
exit
```

Outputs

```sh
[root@manager ~]# cat /var/kerberos/krb5kdc/kadm5.acl 
*/admin@CLOUDERA.COM	*
cloudera-scm@CLOUDERA.COM admilc

[root@manager ~]# kadmin.local
Authenticating as principal root/admin@CLOUDERA.COM with password.
kadmin.local:  addpol admin
kadmin.local:  addpol users
kadmin.local:  addpol hosts
kadmin.local:  exit
```

### Start Kerberos

Script - Server

```sh
service krb5kdc start
service krb5kdc status
service kadmin start
service kadmin status
```

Outputs

```sh
[root@manager ~]# service krb5kdc status
Redirecting to /bin/systemctl status  krb5kdc.service
● krb5kdc.service - Kerberos 5 KDC
   Loaded: loaded (/usr/lib/systemd/system/krb5kdc.service; disabled; vendor preset: disabled)
   Active: active (running) since Wed 2017-05-03 02:14:18 EDT; 21s ago
  Process: 15114 ExecStart=/usr/sbin/krb5kdc -P /var/run/krb5kdc.pid $KRB5KDC_ARGS (code=exited, status=0/SUCCESS)
 Main PID: 15115 (krb5kdc)
   CGroup: /system.slice/krb5kdc.service
           └─15115 /usr/sbin/krb5kdc -P /var/run/krb5kdc.pid

May 03 02:14:18 manager.alanbluwol systemd[1]: Starting Kerberos 5 KDC...
May 03 02:14:18 manager.alanbluwol systemd[1]: PID file /var/run/krb5kdc.pid not readable (yet?) after start.
May 03 02:14:18 manager.alanbluwol systemd[1]: Started Kerberos 5 KDC.
[root@manager ~]# service kadmin status
Redirecting to /bin/systemctl status  kadmin.service
● kadmin.service - Kerberos 5 Password-changing and Administration
   Loaded: loaded (/usr/lib/systemd/system/kadmin.service; disabled; vendor preset: disabled)
   Active: active (running) since Wed 2017-05-03 02:14:25 EDT; 17s ago
  Process: 15147 ExecStart=/usr/sbin/_kadmind -P /var/run/kadmind.pid $KADMIND_ARGS (code=exited, status=0/SUCCESS)
 Main PID: 15148 (kadmind)
   CGroup: /system.slice/kadmin.service
           └─15148 /usr/sbin/kadmind -P /var/run/kadmind.pid

May 03 02:14:25 manager.alanbluwol systemd[1]: Starting Kerberos 5 Password-changing and Administration...
May 03 02:14:25 manager.alanbluwol systemd[1]: Started Kerberos 5 Password-changing and Administration.
```

### Enable Kerberos Cloudera Manager

Now go to Cloudera Manager UI: 
Administration > Security > Enable Kerberos

* Check all the boxes
* KDC Type: MIT KDC
* KDC Server Host: manager.alanbluwol
* Kerberos Security Realm: CLOUDERA.COM
* Kerberos Encryption Types: arcfour-hmac
* Maximum Renewable Life for Principals: 7 days
* Manage krb5.conf through Cloudera Manager: Check the box
* Kerberos Ticket Lifetime: 1 days
* Kerberos Renewable Lifetime: 7 days
* Forwardable Tickets: check the box
* Provide credentials of cloudera-scm principal, which we created before
	* Username: cloudera-scm@CLOUDERA.COM
	* Password: cloudera
* Kerberos Principals for each Service
	* HDFS: hdfs
	* Hive: hive
	* Hue: hue
	* Oozie: oozie
	* YARN: yarn
	* ZooKeeper: zookeeper
* Check the box to restart the cluster, this will deploy kerberos configuration across cluster

Outputs - Cloudera Manager Kerberos Wizard

```sh
Enable Kerberos for alanbluwol
Import KDC Account Manager Credentials Command
Status: Finished Start Time: May 3, 2:20:11 AM Duration: 5.03s
Successfully imported KDC Account Manager credentials.

Enable Kerberos for alanbluwol
Congratulations!
You have enabled Kerberos for all your cluster(s).
Cluster	Status
alanbluwol	Successfully enabled Kerberos.
```

Cloudera Manager warns of insecure interfaces to WebUIs - clearly present a vulnerability in a now secured cluster - fair enough

### Configure Service Security

Hue Service 
-> Add 
-> Instances 
-> Assign the Kerberos Ticket Renewer role instance to the same host as the Hue server

Hive Service 
-> Configuration 
-> Service-wide 
-> Advanced 
-> Hive Service Configuration Safety Valve for hive-site.xml

```xml
<property>
  <name>hive.server2.authentication</name>
  <value>KERBEROS</value>
</property>
<property>
  <name>hive.server2.authentication.kerberos.principal</name>
  <value>hive/_HOST@CLOUDERA.COM</value>
</property>
<property>
 <name>hive.stats.ndv.error</name>
 <value>5.0</value>
</property>
```

### Test Hadoop Access

Script - Kerberos Server

```sh
sudo -u hdfs hadoop fs -ls

kadmin.local
addprinc hdfs@CLOUDERA.COM
exit

sudo -u hdfs hadoop fs -ls
```

Output - Kerberos Server

```sh
[root@manager ~]# sudo -u hdfs hadoop fs -ls 
Warning: fs.defaultFS is not set when running "ls" command.
ls: cannot open directory file:/root: Permission denied

[root@manager ~]# kadmin.local
Authenticating as principal root/admin@CLOUDERA.COM with password.
kadmin.local:  addprinc hdfs@CLOUDERA.COM
WARNING: no policy specified for hdfs@CLOUDERA.COM; defaulting to no policy
Enter password for principal "hdfs@CLOUDERA.COM": cloudera
Re-enter password for principal "hdfs@CLOUDERA.COM": cloudera
Principal "hdfs@CLOUDERA.COM" created.
kadmin.local:  exit

[hdfs@manager ~]$ hadoop fs -ls /
Warning: fs.defaultFS is not set when running "ls" command.
Found 22 items
dr-xr-xr-x   - root root      24576 2017-05-03 01:33 /bin
dr-xr-xr-x   - root root       4096 2017-05-01 01:43 /boot
drwxr-xr-x   - root root          6 2016-10-25 16:10 /data
drwxr-xr-x   - root root       4096 2017-05-01 01:21 /data01
drwxr-xr-x   - root root       2720 2016-03-10 06:05 /dev
drwxr-xr-x   - root root       8192 2017-05-03 02:22 /etc
drwxr-xr-x   - root root         21 2016-03-10 06:05 /home
dr-xr-xr-x   - root root       4096 2017-05-02 21:08 /lib
dr-xr-xr-x   - root root      24576 2017-05-03 01:33 /lib64
drwxr-xr-x   - root root          6 2016-03-10 06:05 /media
drwxr-xr-x   - root root          6 2016-03-10 06:05 /mnt
drwxr-xr-x   - root root       4096 2017-05-01 02:21 /opt
dr-xr-xr-x   - root root          0 2016-03-10 06:05 /proc
drwxr-xr-x   - root root          6 2017-05-02 21:41 /results
dr-xr-x---   - root root       4096 2017-05-02 21:43 /root
drwxr-xr-x   - root root        860 2017-05-03 02:14 /run
dr-xr-xr-x   - root root      12288 2017-05-03 01:33 /sbin
drwxr-xr-x   - root root          6 2016-03-10 06:05 /srv
dr-xr-xr-x   - root root          0 2016-03-10 06:05 /sys
drwxrwxrwt   - root root       4096 2017-05-03 02:55 /tmp
drwxr-xr-x   - root root       4096 2012-01-20 16:00 /usr
drwxr-xr-x   - root root       4096 2017-05-02 21:06 /var
```

Script - Client (Master Node)

```sh
su - hdfs
hadoop fs -ls
kinit
klist
hadoop fs -ls
```

Output

```sh
[hdfs@node1 ~]$ hadoop fs -ls
[Whole bunch of Java Exceptions...]
ls: Failed on local exception: java.io.IOException: javax.security.sasl.SaslException: 
GSS initiate failed [Caused by GSSException: No valid credentials provided 
(Mechanism level: Failed to find any Kerberos tgt)]; 
Host Details : local host is: "node1.alanbluwol/172.31.5.116"; 
destination host is: "node1.alanbluwol":8020; 

[hdfs@node1 ~]$ kinit
Password for hdfs@CLOUDERA.COM: 

[hdfs@node1 ~]$ klist
Ticket cache: FILE:/tmp/krb5cc_993
Default principal: hdfs@CLOUDERA.COM

Valid starting       Expires              Service principal
05/03/2017 02:52:24  05/04/2017 02:52:24  krbtgt/CLOUDERA.COM@CLOUDERA.COM
	renew until 05/10/2017 02:52:24

[hdfs@node1 ~]$ hadoop fs -ls /
Found 6 items
drwxr-xr-x   - alanbluwol supergroup          0 2017-05-02 21:18 /alanbluwol
drwxr-xr-x   - hdfs       supergroup          0 2017-05-02 21:18 /paulcompton
drwxr-xr-x   - hdfs       supergroup          0 2017-05-02 00:13 /precious
drwxr-xr-x   - hdfs       supergroup          0 2017-05-02 21:15 /results
drwxrwxrwt   - hdfs       supergroup          0 2017-05-01 02:48 /tmp
drwxrwxrwx   - hdfs       supergroup          0 2017-05-02 02:25 /user
```

### Create Kerberos User

Script - On all Cluster Nodes

```sh
sudo -i

# For new alanbluwol users
cat /etc/passwd | grep alanbluwol
useradd alanbluwol -u 1001
passwd alanbluwol
cat /etc/group | grep alanbluwol
cat /etc/passwd | grep alanbluwol

# For the existing alanbluwol created in anticipation...
groupadd -g 1001 alanbluwol
cat /etc/group | grep alanbluwol
usermod -u 1001 alanbluwol 
usermod -g 1001 alanbluwol
passwd alanbluwol

su - hdfs
kinit
hadoop fs -mkdir /user/alanbluwol
hadoop fs -chown alanbluwol:alanbluwol /user/alanbluwol
```

Output - On all Cluster Nodes

```sh
[root@manager ~]# useradd alanbluwol -u 1001
[root@node1 ~]# passwd alanbluwol
Changing password for user alanbluwol.
New password: 
Retype new password: 
passwd: all authentication tokens updated successfully.
[root@node1 ~]# cat /etc/group | grep alanbluwol
alanbluwol:x:1001:
[root@node1 ~]# cat /etc/passwd | grep alanbluwol
alanbluwol:x:1001:1001::/home/alanbluwol:/bin/bash

[hdfs@node1 ~]$ hadoop fs -mkdir /user/alanbluwol
mkdir: `/user/alanbluwol': File exists
[hdfs@node1 ~]$ hadoop fs -chown alanbluwol:alanbluwol /user/alanbluwol
```

Script - Kerberos Server

```sh
kadmin.local
addprinc alanbluwol@CLOUDERA.COM
exit
```

Output - Kerberos Server

```sh
kadmin.local:  addprinc alanbluwol@CLOUDERA.COM
WARNING: no policy specified for alanbluwol@CLOUDERA.COM; defaulting to no policy
Enter password for principal "alanbluwol@CLOUDERA.COM": 
Re-enter password for principal "alanbluwol@CLOUDERA.COM": 
Principal "alanbluwol@CLOUDERA.COM" created.
kadmin.local:  exit
```

Script - Cloudera Master

```sh
sudo -i
su - alanbluwol
hadoop fs -ls /user/alanbluwol
kinit
hadoop fs -ls /user/alanbluwol
```

Output - Cloudera Master

```sh
[root@node1 ~]# su - alanbluwol
Last login: Mon May  1 22:10:11 EDT 2017 on pts/1
[alanbluwol@node1 ~]$ hadoop fs -ls /user/alanbluwol
ls: Failed on local exception: java.io.IOException: javax.security.sasl.SaslException: 
GSS initiate failed [Caused by GSSException: 
No valid credentials provided (Mechanism level: 
Failed to find any Kerberos tgt)]; 
Host Details : local host is: "node1.alanbluwol/172.31.5.116"; 
destination host is: "node1.alanbluwol":8020; 

[alanbluwol@node1 ~]$ kinit
Password for alanbluwol@CLOUDERA.COM: 
[alanbluwol@node1 ~]$ hadoop fs -ls /user/alanbluwol
Found 1 items
drwx------   - alanbluwol supergroup          0 2017-05-01 22:20 /user/alanbluwol/.staging
```

### Final Checks

```sh
kadmin.local
getprincs
exit

cat /etc/krb5.conf
cat /var/kerberos/krb5kdc/kdc.conf
cat /var/kerberos/krb5kdc/kadm5.acl
```

Output

```sh
[root@manager ~]# kadmin.local
Authenticating as principal root/admin@CLOUDERA.COM with password.
kadmin.local:  getprincs
HTTP/node1.alanbluwol@CLOUDERA.COM
HTTP/node2.alanbluwol@CLOUDERA.COM
HTTP/node3.alanbluwol@CLOUDERA.COM
HTTP/node4.alanbluwol@CLOUDERA.COM
K/M@CLOUDERA.COM
alanbluwol@CLOUDERA.COM
cloudera-scm@CLOUDERA.COM
hdfs/node1.alanbluwol@CLOUDERA.COM
hdfs/node2.alanbluwol@CLOUDERA.COM
hdfs/node3.alanbluwol@CLOUDERA.COM
hdfs/node4.alanbluwol@CLOUDERA.COM
hdfs@CLOUDERA.COM
hive/node1.alanbluwol@CLOUDERA.COM
httpfs/node1.alanbluwol@CLOUDERA.COM
hue/node1.alanbluwol@CLOUDERA.COM
kadmin/admin@CLOUDERA.COM
kadmin/changepw@CLOUDERA.COM
kadmin/manager.alanbluwol@CLOUDERA.COM
kiprop/manager.alanbluwol@CLOUDERA.COM
krbtgt/CLOUDERA.COM@CLOUDERA.COM
mapred/node1.alanbluwol@CLOUDERA.COM
oozie/node1.alanbluwol@CLOUDERA.COM
yarn/node1.alanbluwol@CLOUDERA.COM
yarn/node2.alanbluwol@CLOUDERA.COM
yarn/node3.alanbluwol@CLOUDERA.COM
yarn/node4.alanbluwol@CLOUDERA.COM
zookeeper/node1.alanbluwol@CLOUDERA.COM
zookeeper/node2.alanbluwol@CLOUDERA.COM
zookeeper/node3.alanbluwol@CLOUDERA.COM
```

# Sentry Lab

## Install Sentry Service

### 

Script

```sh
sudo -i
su - hdfs
hdfs dfs -chmod -R 771 /user/hive/warehouse
hdfs dfs -chown -R hive:hive /user/hive/warehouse
```

Output

```sh
[ec2-user@node1 ~]$ sudo -i
[root@node1 ~]# su - hdfs
Last login: Wed May  3 18:56:03 EDT 2017 on pts/0
[hdfs@node1 ~]$ hdfs dfs -chmod -R 771 /user/hive/warehouse
[hdfs@node1 ~]$ hdfs dfs -chown -R hive:hive /user/hive/warehouse

```

### Disable Impersionation

* Go to the Hive service.
* Click the Configuration tab.
* Select Scope > HiveServer2.
* Select Category > Main.
* Uncheck the HiveServer2 Enable Impersonation checkbox.
* Click Save Changes to commit the changes.

* Go to the MapReduce service.
* Click the Configuration tab.
* Select Scope > NodeManager.
* Select Category > Security.
* Set the Minimum User ID for Job Submission property to zero (the default is 1000).
* Click Save Changes to commit the changes.
* Repeat steps 1-6 for every TaskTracker role group for the MapReduce service that is associated with Hive.
* Restart the MapReduce service.

* Go to the YARN service.
* Click the Configuration tab.
* Select Scope > NodeManager.
* Select Category > Security.
* Ensure the Allowed System Users property includes the hive user. If not, add hive.
* Click Save Changes to commit the changes.
* Repeat steps 1-6 for every NodeManager role group for the YARN service that is associated with Hive.
* Restart the YARN service.

### Add Sentry Service

* Do this in Cloudera Manager > Add Service

### Enabling the Sentry Service for Hive

* Go to the Hive service.
* Click the Configuration tab.
* Select Scope > Hive (Service-Wide).
* Select Category > Main.
* Locate the Sentry Service property and select Sentry.
* Click Save Changes to commit the changes.
* Restart the Hive service.

### Enabling the Sentry Service for Hue

* Go to the Hue service.
* Click the Configuration tab.
* Select Scope > Hue (Service-Wide).
* Select Category > Main.
* Locate the Sentry Service property and select Sentry.
* Click Save Changes to commit the changes.
* Restart Hue.

### Add the Hive and Hue Groups to Sentry's Admin Groups

* Go to the Sentry service.
* Click the Configuration tab.
* Select Scope > Sentry (Service-Wide).
* Select Category > Main.
* Locate the Admin Groups property and add the hive, impala and hue groups to the list. 
	* If an end user is in one of these admin groups, that user has administrative privileges on the Sentry Server.
* Click Save Changes to commit the changes.

### Configure Sentry to recognize this account as an administrator

* Add your test user's primary group to the sentry.service.admin.group list in CM
* Restart Sentry, Hue, Hive, and Impala services

### Verify user privileges

Script

```sh
sudo -i
su - alanbluwol
kinit
beeline
!connect jdbc:hive2://node1.alanbluwol:10000/default;principal=hive/node1.alanbluwol@CLOUDERA.COM
```

Output

```sh
[root@node1 ~]# su - alanbluwol
Last login: Wed May  3 03:19:27 EDT 2017 on pts/1
[alanbluwol@node1 ~]$ kinit
Password for alanbluwol@CLOUDERA.COM: 
[alanbluwol@node1 ~]$ beeline
Beeline version 1.1.0-cdh5.9.2 by Apache Hive
beeline> !connect jdbc:hive2://node1.alanbluwol:10000/default;principal=hive/node1.alanbluwol@CLOUDERA.COM
scan complete in 2ms
Connecting to jdbc:hive2://node1.alanbluwol:10000/default;principal=hive/node1.alanbluwol@CLOUDERA.COM
Connected to: Apache Hive (version 1.1.0-cdh5.9.2)
Driver: Hive JDBC (version 1.1.0-cdh5.9.2)
Transaction isolation: TRANSACTION_REPEATABLE_READ
0: jdbc:hive2://node1.alanbluwol:10000/defaul> 
0: jdbc:hive2://node1.alanbluwol:10000/defaul> SHOW TABLES;
INFO  : Compiling command(queryId=hive_20170503201111_48b20724-ed8d-4396-b628-4d3f29d595ad): SHOW TABLES
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:[FieldSchema(name:tab_name, type:string, comment:from deserializer)], properties:null)
INFO  : Completed compiling command(queryId=hive_20170503201111_48b20724-ed8d-4396-b628-4d3f29d595ad); Time taken: 0.623 seconds
INFO  : Executing command(queryId=hive_20170503201111_48b20724-ed8d-4396-b628-4d3f29d595ad): SHOW TABLES
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503201111_48b20724-ed8d-4396-b628-4d3f29d595ad); Time taken: 0.242 seconds
INFO  : OK
+-----------+--+
| tab_name  |
+-----------+--+
+-----------+--+
No rows selected (2.217 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> 
```

### Create a Sentry role with full authorization

Script

```sh
beeline
!connect jdbc:hive2://node1.alanbluwol:10000/default;principal=hive/node1.alanbluwol@CLOUDERA.COM

CREATE ROLE sentry_admin;
GRANT ALL ON SERVER server1 TO ROLE sentry_admin;
GRANT ROLE sentry_admin TO GROUP alanbluwol;
SHOW TABLES;
```

Output

```sh
0: jdbc:hive2://node1.alanbluwol:10000/defaul> CREATE ROLE sentry_admin;
INFO  : Compiling command(queryId=hive_20170503201616_e28e0fa9-4230-4d70-970d-2e5d288c232d): CREATE ROLE sentry_admin
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:null, properties:null)
INFO  : Completed compiling command(queryId=hive_20170503201616_e28e0fa9-4230-4d70-970d-2e5d288c232d); Time taken: 0.079 seconds
INFO  : Executing command(queryId=hive_20170503201616_e28e0fa9-4230-4d70-970d-2e5d288c232d): CREATE ROLE sentry_admin
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503201616_e28e0fa9-4230-4d70-970d-2e5d288c232d); Time taken: 0.687 seconds
INFO  : OK
No rows affected (0.782 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> GRANT ALL ON SERVER server1 TO ROLE sentry_admin;
INFO  : Compiling command(queryId=hive_20170503201717_941be416-c340-40dd-9cde-b65a14de85c6): GRANT ALL ON SERVER server1 TO ROLE sentry_admin
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:null, properties:null)
INFO  : Completed compiling command(queryId=hive_20170503201717_941be416-c340-40dd-9cde-b65a14de85c6); Time taken: 0.088 seconds
INFO  : Executing command(queryId=hive_20170503201717_941be416-c340-40dd-9cde-b65a14de85c6): GRANT ALL ON SERVER server1 TO ROLE sentry_admin
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503201717_941be416-c340-40dd-9cde-b65a14de85c6); Time taken: 0.077 seconds
INFO  : OK
No rows affected (0.176 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> GRANT ROLE sentry_admin TO GROUP alanbluwol;
INFO  : Compiling command(queryId=hive_20170503201717_32b18526-c2a0-45a1-8c93-45adb78e286a): GRANT ROLE sentry_admin TO GROUP alanbluwol
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:null, properties:null)
INFO  : Completed compiling command(queryId=hive_20170503201717_32b18526-c2a0-45a1-8c93-45adb78e286a); Time taken: 0.057 seconds
INFO  : Executing command(queryId=hive_20170503201717_32b18526-c2a0-45a1-8c93-45adb78e286a): GRANT ROLE sentry_admin TO GROUP alanbluwol
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503201717_32b18526-c2a0-45a1-8c93-45adb78e286a); Time taken: 0.084 seconds
INFO  : OK
No rows affected (0.153 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> SHOW TABLES;
INFO  : Compiling command(queryId=hive_20170503201717_fa29bcfb-c4ef-4a71-9818-624b0236a9fe): SHOW TABLES
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:[FieldSchema(name:tab_name, type:string, comment:from deserializer)], properties:null)
INFO  : Completed compiling command(queryId=hive_20170503201717_fa29bcfb-c4ef-4a71-9818-624b0236a9fe); Time taken: 0.059 seconds
INFO  : Executing command(queryId=hive_20170503201717_fa29bcfb-c4ef-4a71-9818-624b0236a9fe): SHOW TABLES
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503201717_fa29bcfb-c4ef-4a71-9818-624b0236a9fe); Time taken: 0.121 seconds
INFO  : OK
+-------------+--+
|  tab_name   |
+-------------+--+
| alanbluwol  |
+-------------+--+
1 row selected (0.233 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> 
```

### Create additional test users

Add new users to all cluster nodes

Script

```sh
sudo -i
groupadd selector
groupadd inserters
useradd -u 1100 -g selector george
useradd -u 1200 -g inserters ferdinand

kadmin.local
add_principal george
add_principal ferdinand
```

Output

```sh
[ec2-user@manager ~]$ sudo -i
[root@manager ~]# groupadd selector
[root@manager ~]# groupadd inserters
[root@manager ~]# useradd -u 1100 -g selector george
[root@manager ~]# useradd -u 1200 -g inserters ferdinand
[root@manager ~]# kadmin.local
Authenticating as principal root/admin@CLOUDERA.COM with password.
kadmin.local:  add_principal george
WARNING: no policy specified for george@CLOUDERA.COM; defaulting to no policy
Enter password for principal "george@CLOUDERA.COM": 
Re-enter password for principal "george@CLOUDERA.COM": 
Principal "george@CLOUDERA.COM" created.
kadmin.local:  add_principal ferdinand
WARNING: no policy specified for ferdinand@CLOUDERA.COM; defaulting to no policy
Enter password for principal "ferdinand@CLOUDERA.COM": 
Re-enter password for principal "ferdinand@CLOUDERA.COM": 
Principal "ferdinand@CLOUDERA.COM" created.
kadmin.local:  exit
```

### Create test roles

Script

* Connect
* Create test roles
* Grant read privilege for all tables to reads
* Grant read privilege for default.sample07 only to 'writes':

```sh
su - alanbluwol
beeline
!connect jdbc:hive2://node1.alanbluwol:10000/default;principal=hive/node1.alanbluwol@CLOUDERA.COM

CREATE ROLE reads;
CREATE ROLE writes;

GRANT SELECT ON DATABASE default TO ROLE reads;
GRANT ROLE reads TO GROUP selector;

REVOKE ALL ON DATABASE default FROM ROLE writes;
GRANT SELECT ON default.sample_07 TO ROLE writes;
GRANT ROLE writes TO GROUP inserters;
```

Output

```sh
[root@node1 ~]# su - alanbluwol
Last login: Wed May  3 20:10:47 EDT 2017 on pts/1
[alanbluwol@node1 ~]$ beeline
Beeline version 1.1.0-cdh5.9.2 by Apache Hive
beeline> !connect jdbc:hive2://node1.alanbluwol:10000/default;principal=hive/node1.alanbluwol@CLOUDERA.COM
scan complete in 1ms
Connecting to jdbc:hive2://node1.alanbluwol:10000/default;principal=hive/node1.alanbluwol@CLOUDERA.COM
Connected to: Apache Hive (version 1.1.0-cdh5.9.2)
Driver: Hive JDBC (version 1.1.0-cdh5.9.2)
Transaction isolation: TRANSACTION_REPEATABLE_READ
0: jdbc:hive2://node1.alanbluwol:10000/defaul> CREATE ROLE reads;
INFO  : Compiling command(queryId=hive_20170503202424_335d1b22-d383-4078-8cb0-a6bc9d8d3427): CREATE ROLE reads
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:null, properties:null)
INFO  : Completed compiling command(queryId=hive_20170503202424_335d1b22-d383-4078-8cb0-a6bc9d8d3427); Time taken: 0.062 seconds
INFO  : Executing command(queryId=hive_20170503202424_335d1b22-d383-4078-8cb0-a6bc9d8d3427): CREATE ROLE reads
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503202424_335d1b22-d383-4078-8cb0-a6bc9d8d3427); Time taken: 0.038 seconds
INFO  : OK
No rows affected (0.155 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> CREATE ROLE writes;
INFO  : Compiling command(queryId=hive_20170503202424_9d53c2e4-aa97-45df-b487-db3b0d0bdd52): CREATE ROLE writes
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:null, properties:null)
INFO  : Completed compiling command(queryId=hive_20170503202424_9d53c2e4-aa97-45df-b487-db3b0d0bdd52); Time taken: 0.054 seconds
INFO  : Executing command(queryId=hive_20170503202424_9d53c2e4-aa97-45df-b487-db3b0d0bdd52): CREATE ROLE writes
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503202424_9d53c2e4-aa97-45df-b487-db3b0d0bdd52); Time taken: 0.022 seconds
INFO  : OK
No rows affected (0.087 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> GRANT SELECT ON DATABASE default TO ROLE reads;
INFO  : Compiling command(queryId=hive_20170503202424_6d57c3bd-2b4b-4583-a7b4-fd566149766e): GRANT SELECT ON DATABASE default TO ROLE reads
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:null, properties:null)
INFO  : Completed compiling command(queryId=hive_20170503202424_6d57c3bd-2b4b-4583-a7b4-fd566149766e); Time taken: 0.062 seconds
INFO  : Executing command(queryId=hive_20170503202424_6d57c3bd-2b4b-4583-a7b4-fd566149766e): GRANT SELECT ON DATABASE default TO ROLE reads
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503202424_6d57c3bd-2b4b-4583-a7b4-fd566149766e); Time taken: 0.038 seconds
INFO  : OK
No rows affected (0.112 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> GRANT ROLE reads TO GROUP selector;
INFO  : Compiling command(queryId=hive_20170503202424_c6585f2e-9cae-436c-aed7-9054cdf71e67): GRANT ROLE reads TO GROUP selector
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:null, properties:null)
INFO  : Completed compiling command(queryId=hive_20170503202424_c6585f2e-9cae-436c-aed7-9054cdf71e67); Time taken: 0.074 seconds
INFO  : Executing command(queryId=hive_20170503202424_c6585f2e-9cae-436c-aed7-9054cdf71e67): GRANT ROLE reads TO GROUP selector
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503202424_c6585f2e-9cae-436c-aed7-9054cdf71e67); Time taken: 0.025 seconds
INFO  : OK
No rows affected (0.109 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> REVOKE ALL ON DATABASE default FROM ROLE writes;
INFO  : Compiling command(queryId=hive_20170503202626_65a56b6f-ee16-4f9f-9d1c-362bb0cb5de3): REVOKE ALL ON DATABASE default FROM ROLE writes
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:null, properties:null)
INFO  : Completed compiling command(queryId=hive_20170503202626_65a56b6f-ee16-4f9f-9d1c-362bb0cb5de3); Time taken: 0.055 seconds
INFO  : Executing command(queryId=hive_20170503202626_65a56b6f-ee16-4f9f-9d1c-362bb0cb5de3): REVOKE ALL ON DATABASE default FROM ROLE writes
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503202626_65a56b6f-ee16-4f9f-9d1c-362bb0cb5de3); Time taken: 0.081 seconds
INFO  : OK
No rows affected (0.147 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> GRANT SELECT ON default.sample_07 TO ROLE writes;
INFO  : Compiling command(queryId=hive_20170503202626_61820299-1717-4f63-9047-8449dc3b6d7c): GRANT SELECT ON default.sample_07 TO ROLE writes
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:null, properties:null)
INFO  : Completed compiling command(queryId=hive_20170503202626_61820299-1717-4f63-9047-8449dc3b6d7c); Time taken: 0.054 seconds
INFO  : Executing command(queryId=hive_20170503202626_61820299-1717-4f63-9047-8449dc3b6d7c): GRANT SELECT ON default.sample_07 TO ROLE writes
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503202626_61820299-1717-4f63-9047-8449dc3b6d7c); Time taken: 0.033 seconds
INFO  : OK
No rows affected (0.099 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> GRANT ROLE writes TO GROUP inserters;
INFO  : Compiling command(queryId=hive_20170503202626_8985705b-5aa8-490f-a54f-27277a9796f4): GRANT ROLE writes TO GROUP inserters
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:null, properties:null)
INFO  : Completed compiling command(queryId=hive_20170503202626_8985705b-5aa8-490f-a54f-27277a9796f4); Time taken: 0.053 seconds
INFO  : Executing command(queryId=hive_20170503202626_8985705b-5aa8-490f-a54f-27277a9796f4): GRANT ROLE writes TO GROUP inserters
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503202626_8985705b-5aa8-490f-a54f-27277a9796f4); Time taken: 0.024 seconds
INFO  : OK
No rows affected (0.087 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> 
```

### Test Users

Script

* George can read tables
* Ferdinand cannot read tables

```sh
exit
su - george
kinit
klist
beeline
!connect jdbc:hive2://node1.alanbluwol:10000/default;principal=hive/node1.alanbluwol@CLOUDERA.COM
SHOW TABLES;

exit
su - ferdinand
kinit
klist
beeline
!connect jdbc:hive2://node1.alanbluwol:10000/default;principal=hive/node1.alanbluwol@CLOUDERA.COM
SHOW TABLES;
```

Output

```sh
[alanbluwol@node1 ~]$ exit
logout
[root@node1 ~]# su - george
[george@node1 ~]$ kinit
Password for george@CLOUDERA.COM: 
[george@node1 ~]$ klist
Ticket cache: FILE:/tmp/krb5cc_1100
Default principal: george@CLOUDERA.COM

Valid starting       Expires              Service principal
05/03/2017 20:28:16  05/04/2017 20:28:16  krbtgt/CLOUDERA.COM@CLOUDERA.COM
	renew until 05/10/2017 20:28:16
[george@node1 ~]$ beeline
Beeline version 1.1.0-cdh5.9.2 by Apache Hive
beeline> !connect jdbc:hive2://node1.alanbluwol:10000/default;principal=hive/node1.alanbluwol@CLOUDERA.COM
scan complete in 1ms
Connecting to jdbc:hive2://node1.alanbluwol:10000/default;principal=hive/node1.alanbluwol@CLOUDERA.COM
Connected to: Apache Hive (version 1.1.0-cdh5.9.2)
Driver: Hive JDBC (version 1.1.0-cdh5.9.2)
Transaction isolation: TRANSACTION_REPEATABLE_READ
0: jdbc:hive2://node1.alanbluwol:10000/defaul> SHOW TABLES;
INFO  : Compiling command(queryId=hive_20170503202828_9261f6d7-d9b2-4ffe-a084-e6326d512f4f): SHOW TABLES
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:[FieldSchema(name:tab_name, type:string, comment:from deserializer)], properties:null)
INFO  : Completed compiling command(queryId=hive_20170503202828_9261f6d7-d9b2-4ffe-a084-e6326d512f4f); Time taken: 0.06 seconds
INFO  : Executing command(queryId=hive_20170503202828_9261f6d7-d9b2-4ffe-a084-e6326d512f4f): SHOW TABLES
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503202828_9261f6d7-d9b2-4ffe-a084-e6326d512f4f); Time taken: 0.121 seconds
INFO  : OK
+-------------+--+
|  tab_name   |
+-------------+--+
| alanbluwol  |
+-------------+--+
1 row selected (0.271 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> 


[george@node1 ~]$ exit
logout
[root@node1 ~]# su - ferdinand
[ferdinand@node1 ~]$ kinit
Password for ferdinand@CLOUDERA.COM: 
[ferdinand@node1 ~]$ klist
Ticket cache: FILE:/tmp/krb5cc_1200
Default principal: ferdinand@CLOUDERA.COM

Valid starting       Expires              Service principal
05/03/2017 20:29:53  05/04/2017 20:29:53  krbtgt/CLOUDERA.COM@CLOUDERA.COM
	renew until 05/10/2017 20:29:53
[ferdinand@node1 ~]$ beeline
Beeline version 1.1.0-cdh5.9.2 by Apache Hive
beeline> !connect jdbc:hive2://node1.alanbluwol:10000/default;principal=hive/node1.alanbluwol@CLOUDERA.COM
scan complete in 2ms
Connecting to jdbc:hive2://node1.alanbluwol:10000/default;principal=hive/node1.alanbluwol@CLOUDERA.COM
Connected to: Apache Hive (version 1.1.0-cdh5.9.2)
Driver: Hive JDBC (version 1.1.0-cdh5.9.2)
Transaction isolation: TRANSACTION_REPEATABLE_READ
0: jdbc:hive2://node1.alanbluwol:10000/defaul> SHOW TABLES;
INFO  : Compiling command(queryId=hive_20170503203030_9263758d-7c8c-40fd-ade2-6eceb056a5b6): SHOW TABLES
INFO  : Semantic Analysis Completed
INFO  : Returning Hive schema: Schema(fieldSchemas:[FieldSchema(name:tab_name, type:string, comment:from deserializer)], properties:null)
INFO  : Completed compiling command(queryId=hive_20170503203030_9263758d-7c8c-40fd-ade2-6eceb056a5b6); Time taken: 0.061 seconds
INFO  : Executing command(queryId=hive_20170503203030_9263758d-7c8c-40fd-ade2-6eceb056a5b6): SHOW TABLES
INFO  : Starting task [Stage-0:DDL] in serial mode
INFO  : Completed executing command(queryId=hive_20170503203030_9263758d-7c8c-40fd-ade2-6eceb056a5b6); Time taken: 0.124 seconds
INFO  : OK
+-----------+--+
| tab_name  |
+-----------+--+
+-----------+--+
No rows selected (0.269 seconds)
0: jdbc:hive2://node1.alanbluwol:10000/defaul> 
```

# 0 - Linux Users

### Authenticate using Linux users/groups

Script

* Verify hadoop group is available
* Create password script, so that its not in hue.ini configuration file
* Test script
* Change hue.ini to point to new script for passwords
* Run User Admin Sync with Unix

```sh
sudo -i
getent group | grep hadoop
mkdir /var/lib/hue
echo << 'EOF' > /var/lib/hue/hue_passwords.sh
#!/bin/bash
 
SERVICE=$1

if [[ ${SERVICE} == "sec-1-secret_key" ]]
then
echo "cloudera"
fi

if [[ ${SERVICE} == "sec-5-password" ]]
then
echo "cloudera"
fi
EOF
chmod +x /var/lib/hue/hue_passwords.sh

# Test Script
/var/lib/hue/hue_passwords.sh sec-1-secret_key

export HUE_CONF_DIR="/var/run/cloudera-scm-agent/process/`ls -alrt /var/run/cloudera-scm-agent/process | grep HUE | tail -1 | awk '{print $9}'`"

cd $HUE_CONF_DIR
vi hue.ini
# Replace the command for passwords from/to
# secret_key_script=/run/cloudera-scm-agent/process/318-hue-HUE_SERVER/altscript.sh sec-1-secret_key
# password_script=/run/cloudera-scm-agent/process/318-hue-HUE_SERVER/altscript.sh sec-5-password
# secret_key_script=/var/lib/hue/hue_passwords.sh sec-1-secret_key
# password_script=/var/lib/hue/hue_passwords.sh sec-5-password

/data01/opt/cloudera/parcels/CDH-5.9.2-1.cdh5.9.2.p0.3/lib/hue/build/env/bin/hue useradmin_sync_with_unix
/opt/cloudera/parcels/CDH/lib/hue/build/env/bin/hue changepassword alanbluwol
/opt/cloudera/parcels/CDH/lib/hue/build/env/bin/hue changepassword george
```

Output

```sh
[ec2-user@node1 ~]$ cat /etc/group | grep hadoop
hadoop:x:991:hdfs,mapred,yarn

[root@node1 318-hue-HUE_SERVER]# /data01/opt/cloudera/parcels/CDH-5.9.2-1.cdh5.9.2.p0.3/lib/hue/build/env/bin/hue useradmin_sync_with_unix

[root@node1 318-hue-HUE_SERVER]# /opt/cloudera/parcels/CDH/lib/hue/build/env/bin/hue --version
1.6.10

[root@node1 ~]# /opt/cloudera/parcels/CDH/lib/hue/build/env/bin/hue changepassword george
Changing password for user 'george'
Password: 
Password (again): 
Password changed successfully for user 'george'
```

# 1 - Hue & Sentry Integration

### Preparation

Script

* Update hue.ini

```sh


[libsentry]
  # Hostname or IP of server.
  hostname=localhost
  # Port the sentry service is running on.
  port=8038
  # Sentry configuration directory, where sentry-site.xml is located.
  sentry_conf_dir=/etc/sentry/conf


```

Output

```sh

```


###

Script

```sh

```

Output

```sh

```


###

Script

```sh

```

Output

```sh

```


###

Script

```sh

```

Output

```sh

```


###

Script

```sh

```

Output

```sh

```


###

Script

```sh

```

Output

```sh

```


###

Script

```sh

```

Output

```sh

```

###

Script

```sh

```

Output

```sh

```


```sh

```





Verification Tools

kinit -- valid command
klist -- ticket issued
kvno -- version discrepancies across nodes

# All nodes
yum -y install krb5-workstation

# Server Node
yum -y install krb5-server

# Check 
Security Inspector Control - enable Unlimited Encryption Strength
OR
removing aes256-cts:normal from the supported_enctypes field of the kdc.conf or krb5.conf file.
create a new Kerberos database and then restart both the KDC and the kadmin server.

kadmin
addprinc test

kinit test

klist -e 



/var/kerberos/krb5kdc/...


### Check current user

[root@ip-172-31-1-85 ~]# id
uid=0(root) gid=0(root) groups=0(root) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
[root@ip-172-31-1-85 ~]# id -Gn
root







### Other

Script

```sh
yarn application -list

/# Docs: http://cloudera.github.io/cm_api/apidocs/v14/index.html

curl -X GET -u "admin:cloudera" -i http://52.64.211.37:7180/api/v14/tools/echo?message=hello
curl -X GET -u "admin:cloudera" -i http://52.64.211.37:7180/api/v14/clusters
curl -X GET -u "admin:cloudera" -i http://52.64.211.37:7180/api/v14/clusters/alanbluwol/services
curl -X GET -u "admin:cloudera" -i http://52.64.211.37:7180/api/v14/hosts
```

Output

```sh
17/05/01 19:49:20 INFO client.RMProxy: Connecting to ResourceManager at node1.alanbluwol/172.31.5.116:8032
Total number of applications (application-types: [] and states: [SUBMITTED, ACCEPTED, RUNNING]):0
                Application-Id	    Application-Name	    Application-Type	      User	     Queue	             State	       Final-State	       Progress	                       Tracking-URL

AlanBluwol:SEBC alanbluwol$ curl -X GET -u "admin:cloudera" -i http://52.64.211.37:7180/api/v14/tools/echo?message=hello
HTTP/1.1 200 OK
Expires: Thu, 01-Jan-1970 00:00:00 GMT
Set-Cookie: CLOUDERA_MANAGER_SESSIONID=896v4hncnmzu1gc59th2y1lv8;Path=/;HttpOnly
Content-Type: application/json
Date: Mon, 01 May 2017 23:35:28 GMT
Transfer-Encoding: chunked
Server: Jetty(6.1.26.cloudera.4)

{
  "message" : "hello"
}

AlanBluwol:SEBC alanbluwol$ curl -X GET -u "admin:cloudera" -i http://52.64.211.37:7180/api/v14/clusters
HTTP/1.1 200 OK
Expires: Thu, 01-Jan-1970 00:00:00 GMT
Set-Cookie: CLOUDERA_MANAGER_SESSIONID=h0y0eq7xizhzd78yifcelu1s;Path=/;HttpOnly
Content-Type: application/json
Date: Mon, 01 May 2017 23:36:26 GMT
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

AlanBluwol:SEBC alanbluwol$ curl -X GET -u "admin:cloudera" -i http://52.64.211.37:7180/api/v14/clusters/alanbluwol/services
HTTP/1.1 200 OK
Expires: Thu, 01-Jan-1970 00:00:00 GMT
Set-Cookie: CLOUDERA_MANAGER_SESSIONID=1f91clxfo5cb089l7pnpxymp1;Path=/;HttpOnly
Content-Type: application/json
Date: Mon, 01 May 2017 23:37:19 GMT
Transfer-Encoding: chunked
Server: Jetty(6.1.26.cloudera.4)

{
  "items" : [ {
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
  }, {
    "name" : "zookeeper",
    "type" : "ZOOKEEPER",
    "clusterRef" : {
      "clusterName" : "cluster"
    },
    "serviceUrl" : "http://ip-172-31-1-85.ap-southeast-2.compute.internal:7180/cmf/serviceRedirect/zookeeper",
    "roleInstancesUrl" : "http://ip-172-31-1-85.ap-southeast-2.compute.internal:7180/cmf/serviceRedirect/zookeeper/instances",
    "serviceState" : "STARTED",
    "healthSummary" : "GOOD",
    "healthChecks" : [ {
      "name" : "ZOOKEEPER_CANARY_HEALTH",
      "summary" : "GOOD",
      "suppressed" : false
    }, {
      "name" : "ZOOKEEPER_SERVERS_HEALTHY",
      "summary" : "GOOD",
      "suppressed" : false
    } ],
    "configStalenessStatus" : "FRESH",
    "clientConfigStalenessStatus" : "FRESH",
    "maintenanceMode" : false,
    "maintenanceOwners" : [ ],
    "displayName" : "ZooKeeper",
    "entityStatus" : "GOOD_HEALTH"
  }, {
    "name" : "hue",
    "type" : "HUE",
    "clusterRef" : {
      "clusterName" : "cluster"
    },
    "serviceUrl" : "http://ip-172-31-1-85.ap-southeast-2.compute.internal:7180/cmf/serviceRedirect/hue",
    "roleInstancesUrl" : "http://ip-172-31-1-85.ap-southeast-2.compute.internal:7180/cmf/serviceRedirect/hue/instances",
    "serviceState" : "STARTED",
    "healthSummary" : "GOOD",
    "healthChecks" : [ {
      "name" : "HUE_HUE_SERVERS_HEALTHY",
      "summary" : "GOOD",
      "suppressed" : false
    } ],
    "configStalenessStatus" : "FRESH",
    "clientConfigStalenessStatus" : "FRESH",
    "maintenanceMode" : false,
    "maintenanceOwners" : [ ],
    "displayName" : "Hue",
    "entityStatus" : "GOOD_HEALTH"
  }, {
    "name" : "oozie",
    "type" : "OOZIE",
    "clusterRef" : {
      "clusterName" : "cluster"
    },
    "serviceUrl" : "http://ip-172-31-1-85.ap-southeast-2.compute.internal:7180/cmf/serviceRedirect/oozie",
    "roleInstancesUrl" : "http://ip-172-31-1-85.ap-southeast-2.compute.internal:7180/cmf/serviceRedirect/oozie/instances",
    "serviceState" : "STARTED",
    "healthSummary" : "GOOD",
    "healthChecks" : [ {
      "name" : "OOZIE_OOZIE_SERVERS_HEALTHY",
      "summary" : "GOOD",
      "suppressed" : false
    } ],
    "configStalenessStatus" : "FRESH",
    "clientConfigStalenessStatus" : "FRESH",
    "maintenanceMode" : false,
    "maintenanceOwners" : [ ],
    "displayName" : "Oozie",
    "entityStatus" : "GOOD_HEALTH"
  }, {
    "name" : "yarn",
    "type" : "YARN",
    "clusterRef" : {
      "clusterName" : "cluster"
    },
    "serviceUrl" : "http://ip-172-31-1-85.ap-southeast-2.compute.internal:7180/cmf/serviceRedirect/yarn",
    "roleInstancesUrl" : "http://ip-172-31-1-85.ap-southeast-2.compute.internal:7180/cmf/serviceRedirect/yarn/instances",
    "serviceState" : "STARTED",
    "healthSummary" : "GOOD",
    "healthChecks" : [ {
      "name" : "YARN_JOBHISTORY_HEALTH",
      "summary" : "GOOD",
      "suppressed" : false
    }, {
      "name" : "YARN_NODE_MANAGERS_HEALTHY",
      "summary" : "GOOD",
      "suppressed" : false
    }, {
      "name" : "YARN_RESOURCEMANAGERS_HEALTH",
      "summary" : "GOOD",
      "suppressed" : false
    }, {
      "name" : "YARN_USAGE_AGGREGATION_HEALTH",
      "summary" : "DISABLED",
      "suppressed" : false
    } ],
    "configStalenessStatus" : "FRESH",
    "clientConfigStalenessStatus" : "FRESH",
    "maintenanceMode" : false,
    "maintenanceOwners" : [ ],
    "displayName" : "YARN (MR2 Included)",
    "entityStatus" : "GOOD_HEALTH"
  }, {
    "name" : "hdfs",
    "type" : "HDFS",
    "clusterRef" : {
      "clusterName" : "cluster"
    },
    "serviceUrl" : "http://ip-172-31-1-85.ap-southeast-2.compute.internal:7180/cmf/serviceRedirect/hdfs",
    "roleInstancesUrl" : "http://ip-172-31-1-85.ap-southeast-2.compute.internal:7180/cmf/serviceRedirect/hdfs/instances",
    "serviceState" : "STARTED",
    "healthSummary" : "GOOD",
    "healthChecks" : [ {
      "name" : "HDFS_BLOCKS_WITH_CORRUPT_REPLICAS",
      "summary" : "GOOD",
      "suppressed" : false
    }, {
      "name" : "HDFS_CANARY_HEALTH",
      "summary" : "GOOD",
      "suppressed" : false
    }, {
      "name" : "HDFS_DATA_NODES_HEALTHY",
      "summary" : "GOOD",
      "suppressed" : false
    }, {
      "name" : "HDFS_FREE_SPACE_REMAINING",
      "summary" : "GOOD",
      "suppressed" : false
    }, {
      "name" : "HDFS_HA_NAMENODE_HEALTH",
      "summary" : "GOOD",
      "suppressed" : false
    }, {
      "name" : "HDFS_MISSING_BLOCKS",
      "summary" : "GOOD",
      "suppressed" : false
    }, {
      "name" : "HDFS_UNDER_REPLICATED_BLOCKS",
      "summary" : "GOOD",
      "suppressed" : false
    } ],
    "configStalenessStatus" : "FRESH",
    "clientConfigStalenessStatus" : "FRESH",
    "maintenanceMode" : false,
    "maintenanceOwners" : [ ],
    "displayName" : "HDFS",
    "entityStatus" : "GOOD_HEALTH"
  } ]
}

AlanBluwol:SEBC alanbluwol$ curl -X GET -u "admin:cloudera" -i http://52.64.211.37:7180/api/v14/hosts
HTTP/1.1 200 OK
Expires: Thu, 01-Jan-1970 00:00:00 GMT
Set-Cookie: CLOUDERA_MANAGER_SESSIONID=g526hs5vht1gd3g1k1xw9uyw;Path=/;HttpOnly
Content-Type: application/json
Date: Mon, 01 May 2017 23:38:11 GMT
Transfer-Encoding: chunked
Server: Jetty(6.1.26.cloudera.4)

{
  "items" : [ {
    "hostId" : "i-0c598fea474f1c40c",
    "ipAddress" : "172.31.5.116",
    "hostname" : "node1.alanbluwol",
    "rackId" : "/default",
    "hostUrl" : "http://ip-172-31-1-85.ap-southeast-2.compute.internal:7180/cmf/hostRedirect/i-0c598fea474f1c40c",
    "maintenanceMode" : false,
    "maintenanceOwners" : [ ],
    "commissionState" : "COMMISSIONED",
    "numCores" : 4,
    "numPhysicalCores" : 2,
    "totalPhysMemBytes" : 16389316608
  }, {
    "hostId" : "i-086a43bd697f8a95d",
    "ipAddress" : "172.31.7.189",
    "hostname" : "node2.alanbluwol",
    "rackId" : "/default",
    "hostUrl" : "http://ip-172-31-1-85.ap-southeast-2.compute.internal:7180/cmf/hostRedirect/i-086a43bd697f8a95d",
    "maintenanceMode" : false,
    "maintenanceOwners" : [ ],
    "commissionState" : "COMMISSIONED",
    "numCores" : 4,
    "numPhysicalCores" : 2,
    "totalPhysMemBytes" : 16389316608
  }, {
    "hostId" : "i-08e2e5b156afa2c87",
    "ipAddress" : "172.31.13.46",
    "hostname" : "node3.alanbluwol",
    "rackId" : "/default",
    "hostUrl" : "http://ip-172-31-1-85.ap-southeast-2.compute.internal:7180/cmf/hostRedirect/i-08e2e5b156afa2c87",
    "maintenanceMode" : false,
    "maintenanceOwners" : [ ],
    "commissionState" : "COMMISSIONED",
    "numCores" : 4,
    "numPhysicalCores" : 2,
    "totalPhysMemBytes" : 16389316608
  }, {
    "hostId" : "i-0e0a065748a9cd385",
    "ipAddress" : "172.31.15.253",
    "hostname" : "node4.alanbluwol",
    "rackId" : "/default",
    "hostUrl" : "http://ip-172-31-1-85.ap-southeast-2.compute.internal:7180/cmf/hostRedirect/i-0e0a065748a9cd385",
    "maintenanceMode" : false,
    "maintenanceOwners" : [ ],
    "commissionState" : "COMMISSIONED",
    "numCores" : 4,
    "numPhysicalCores" : 2,
    "totalPhysMemBytes" : 16389316608
  } ]
}

```



Access key ID: AKIAIHZKQXTIK2XEKFXQ
Secret access key: 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


## X - System Checks

### System Check Point

```sh
sudo -i

cat /etc/redhat-release

OS | Red Hat Enterprise Linux Server release 7.3 (Maipo)
AMI | ami-39ac915a
Region | Asia Pacific (Sydney) ap-southeast-2a
FQDN | ec2-13-55-168-139.ap-southeast-2.compute.amazonaws.com

uptime
hadoop checknative
ls /etc/yum.repos.d
```

### Create Local Users & Groups

```sh
groupadd group1
groupadd group2
useradd -u 2200 -g "group1" user1
useradd -u 2300 -g "group2" user2

cat /etc/passwd
cat /etc/group
```

### Create HDFS Users, Groups & Folders

```sh
su - hdfs
hdfs dfs -mkdir /user/user1
hdfs dfs -mkdir /user/user2
hdfs dfs -chown user1:group1 /user/user1
hdfs dfs -chown user2:group2 /user/user2
```

### System Check Point

```sh
mysql -uroot -pcloudera -e "SELECT SERVICE_TYPE FROM scm.SERVICES\G"
hadoop checknative
hdfs dfs -ls /user
```

### Run Teragen as User 1

```sh
HADOOP_PATH=/opt/cloudera/parcels/CDH/lib/hadoop-0.20-mapreduce
hadoop jar $HADOOP_PATH/hadoop-examples.jar teragen -D dfs.blocksize=16777216 51200000 /user/user1/tgen

hdfs dfs -ls /user/user1/tgen
```
