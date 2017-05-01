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
echo "127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4" > /etc/hosts
echo "::1         localhost localhost.localdomain localhost6 localhost6.localdomain6" >> /etc/hosts
echo "172.31.5.116 node1.alanbluwol node1 master1 ip-172-31-5-116" >> /etc/hosts
echo "172.31.7.189 node2.alanbluwol node2 worker1 ip-172-31-7-189" >> /etc/hosts
echo "172.31.13.46 node3.alanbluwol node3 worker2 ip-172-31-13-46" >> /etc/hosts
echo "172.31.15.253 node4.alanbluwol node4 worker3 ip-172-31-15-253" >> /etc/hosts

getent hosts
```

Output

```sh
[root@ip-172-31-1-85 ~]# getent hosts
127.0.0.1       localhost localhost.localdomain localhost4 localhost4.localdomain4
127.0.0.1       localhost localhost.localdomain localhost6 localhost6.localdomain6
172.31.5.116    node1.alanbluwol node1 master1 ip-172-31-1-226
172.31.7.189    node2.alanbluwol node2 worker1 ip-172-31-6-72
172.31.13.46    node3.alanbluwol node3 worker2 ip-172-31-10-115
172.31.15.253    node4.alanbluwol node4 worker3 ip-172-31-15-253
```

### Security-Enhanced Linux Disabled

Script

```sh
sed -i 's/enabled/disabled/g' /etc/selinux/config
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
yum-config-manager --add-repo https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/cloudera-manager.repo
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
create database HMS; -- Hive Metastore
create database huey; -- Hue Metastore
create database repman; -- Reports Manager
create database works; -- Oozie database
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