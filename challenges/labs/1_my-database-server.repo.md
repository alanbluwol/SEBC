# Services Enablement Boot Camp

# 1 - Database Server Repo

* Repo Package

```sh
[root@ip-172-31-2-184 ~]# yum -y install mariadb-server
Loaded plugins: amazon-id, rhui-lb, search-disabled-repos
Resolving Dependencies
--> Running transaction check
---> Package mariadb-server.x86_64 1:5.5.52-1.el7 will be installed
```

Supporting Scripts

```sh
[root@ip-172-31-2-184 ~]# hostname
manager.alanbluwol
[root@ip-172-31-2-184 ~]# yum -y install mariadb-server
Loaded plugins: amazon-id, rhui-lb, search-disabled-repos
Resolving Dependencies
--> Running transaction check
---> Package mariadb-server.x86_64 1:5.5.52-1.el7 will be installed
--> Processing Dependency: mariadb(x86-64) = 1:5.5.52-1.el7 for package: 1:mariadb-server-5.5.52-1.el7.x86_64
--> Processing Dependency: mariadb-libs(x86-64) = 1:5.5.52-1.el7 for package: 1:mariadb-server-5.5.52-1.el7.x86_64
--> Processing Dependency: libaio.so.1(LIBAIO_0.1)(64bit) for package: 1:mariadb-server-5.5.52-1.el7.x86_64
--> Processing Dependency: libaio.so.1(LIBAIO_0.4)(64bit) for package: 1:mariadb-server-5.5.52-1.el7.x86_64
--> Processing Dependency: perl(DBI) for package: 1:mariadb-server-5.5.52-1.el7.x86_64
--> Processing Dependency: perl(Data::Dumper) for package: 1:mariadb-server-5.5.52-1.el7.x86_64
--> Processing Dependency: perl-DBD-MySQL for package: 1:mariadb-server-5.5.52-1.el7.x86_64
--> Processing Dependency: perl-DBI for package: 1:mariadb-server-5.5.52-1.el7.x86_64
--> Processing Dependency: libaio.so.1()(64bit) for package: 1:mariadb-server-5.5.52-1.el7.x86_64
--> Running transaction check
---> Package libaio.x86_64 0:0.3.109-13.el7 will be installed
---> Package mariadb.x86_64 1:5.5.52-1.el7 will be installed
---> Package mariadb-libs.x86_64 1:5.5.50-1.el7_2 will be updated
---> Package mariadb-libs.x86_64 1:5.5.52-1.el7 will be an update
---> Package perl-DBD-MySQL.x86_64 0:4.023-5.el7 will be installed
---> Package perl-DBI.x86_64 0:1.627-4.el7 will be installed
--> Processing Dependency: perl(RPC::PlClient) >= 0.2000 for package: perl-DBI-1.627-4.el7.x86_64
--> Processing Dependency: perl(RPC::PlServer) >= 0.2001 for package: perl-DBI-1.627-4.el7.x86_64
---> Package perl-Data-Dumper.x86_64 0:2.145-3.el7 will be installed
--> Running transaction check
---> Package perl-PlRPC.noarch 0:0.2020-14.el7 will be installed
--> Processing Dependency: perl(Net::Daemon) >= 0.13 for package: perl-PlRPC-0.2020-14.el7.noarch
--> Processing Dependency: perl(Compress::Zlib) for package: perl-PlRPC-0.2020-14.el7.noarch
--> Processing Dependency: perl(Net::Daemon::Log) for package: perl-PlRPC-0.2020-14.el7.noarch
--> Processing Dependency: perl(Net::Daemon::Test) for package: perl-PlRPC-0.2020-14.el7.noarch
--> Running transaction check
---> Package perl-IO-Compress.noarch 0:2.061-2.el7 will be installed
--> Processing Dependency: perl(Compress::Raw::Bzip2) >= 2.061 for package: perl-IO-Compress-2.061-2.el7.noarch
--> Processing Dependency: perl(Compress::Raw::Zlib) >= 2.061 for package: perl-IO-Compress-2.061-2.el7.noarch
---> Package perl-Net-Daemon.noarch 0:0.48-5.el7 will be installed
--> Running transaction check
---> Package perl-Compress-Raw-Bzip2.x86_64 0:2.061-3.el7 will be installed
---> Package perl-Compress-Raw-Zlib.x86_64 1:2.061-4.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

===================================================================================================================================================================================
 Package                                        Arch                          Version                                Repository                                               Size
===================================================================================================================================================================================
Installing:
 mariadb-server                                 x86_64                        1:5.5.52-1.el7                         rhui-REGION-rhel-server-releases                         11 M
Installing for dependencies:
 libaio                                         x86_64                        0.3.109-13.el7                         rhui-REGION-rhel-server-releases                         24 k
 mariadb                                        x86_64                        1:5.5.52-1.el7                         rhui-REGION-rhel-server-releases                        8.6 M
 perl-Compress-Raw-Bzip2                        x86_64                        2.061-3.el7                            rhui-REGION-rhel-server-releases                         32 k
 perl-Compress-Raw-Zlib                         x86_64                        1:2.061-4.el7                          rhui-REGION-rhel-server-releases                         57 k
 perl-DBD-MySQL                                 x86_64                        4.023-5.el7                            rhui-REGION-rhel-server-releases                        140 k
 perl-DBI                                       x86_64                        1.627-4.el7                            rhui-REGION-rhel-server-releases                        802 k
 perl-Data-Dumper                               x86_64                        2.145-3.el7                            rhui-REGION-rhel-server-releases                         47 k
 perl-IO-Compress                               noarch                        2.061-2.el7                            rhui-REGION-rhel-server-releases                        260 k
 perl-Net-Daemon                                noarch                        0.48-5.el7                             rhui-REGION-rhel-server-releases                         51 k
 perl-PlRPC                                     noarch                        0.2020-14.el7                          rhui-REGION-rhel-server-releases                         36 k
Updating for dependencies:
 mariadb-libs                                   x86_64                        1:5.5.52-1.el7                         rhui-REGION-rhel-server-releases                        761 k

Transaction Summary
===================================================================================================================================================================================
Install  1 Package  (+10 Dependent packages)
Upgrade             (  1 Dependent package)

Total download size: 21 M
Downloading packages:
Delta RPMs disabled because /usr/bin/applydeltarpm not installed.
(1/12): libaio-0.3.109-13.el7.x86_64.rpm                                                                                                                    |  24 kB  00:00:00     
(2/12): mariadb-libs-5.5.52-1.el7.x86_64.rpm                                                                                                                | 761 kB  00:00:00     
(3/12): perl-Compress-Raw-Bzip2-2.061-3.el7.x86_64.rpm                                                                                                      |  32 kB  00:00:00     
(4/12): perl-Compress-Raw-Zlib-2.061-4.el7.x86_64.rpm                                                                                                       |  57 kB  00:00:00     
(5/12): perl-DBD-MySQL-4.023-5.el7.x86_64.rpm                                                                                                               | 140 kB  00:00:00     
(6/12): perl-DBI-1.627-4.el7.x86_64.rpm                                                                                                                     | 802 kB  00:00:00     
(7/12): perl-Data-Dumper-2.145-3.el7.x86_64.rpm                                                                                                             |  47 kB  00:00:00     
(8/12): perl-IO-Compress-2.061-2.el7.noarch.rpm                                                                                                             | 260 kB  00:00:00     
(9/12): perl-Net-Daemon-0.48-5.el7.noarch.rpm                                                                                                               |  51 kB  00:00:00     
(10/12): perl-PlRPC-0.2020-14.el7.noarch.rpm                                                                                                                |  36 kB  00:00:00     
(11/12): mariadb-5.5.52-1.el7.x86_64.rpm                                                                                                                    | 8.6 MB  00:00:00     
(12/12): mariadb-server-5.5.52-1.el7.x86_64.rpm                                                                                                             |  11 MB  00:00:00     
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Total                                                                                                                                               22 MB/s |  21 MB  00:00:00     
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Updating   : 1:mariadb-libs-5.5.52-1.el7.x86_64                                                                                                                             1/13 
  Installing : perl-Data-Dumper-2.145-3.el7.x86_64                                                                                                                            2/13 
  Installing : 1:mariadb-5.5.52-1.el7.x86_64                                                                                                                                  3/13 
  Installing : libaio-0.3.109-13.el7.x86_64                                                                                                                                   4/13 
  Installing : perl-Net-Daemon-0.48-5.el7.noarch                                                                                                                              5/13 
  Installing : 1:perl-Compress-Raw-Zlib-2.061-4.el7.x86_64                                                                                                                    6/13 
  Installing : perl-Compress-Raw-Bzip2-2.061-3.el7.x86_64                                                                                                                     7/13 
  Installing : perl-IO-Compress-2.061-2.el7.noarch                                                                                                                            8/13 
  Installing : perl-PlRPC-0.2020-14.el7.noarch                                                                                                                                9/13 
  Installing : perl-DBI-1.627-4.el7.x86_64                                                                                                                                   10/13 
  Installing : perl-DBD-MySQL-4.023-5.el7.x86_64                                                                                                                             11/13 
  Installing : 1:mariadb-server-5.5.52-1.el7.x86_64                                                                                                                          12/13 
  Cleanup    : 1:mariadb-libs-5.5.50-1.el7_2.x86_64                                                                                                                          13/13 
  Verifying  : perl-Compress-Raw-Bzip2-2.061-3.el7.x86_64                                                                                                                     1/13 
  Verifying  : 1:mariadb-5.5.52-1.el7.x86_64                                                                                                                                  2/13 
  Verifying  : perl-Data-Dumper-2.145-3.el7.x86_64                                                                                                                            3/13 
  Verifying  : 1:mariadb-server-5.5.52-1.el7.x86_64                                                                                                                           4/13 
  Verifying  : 1:mariadb-libs-5.5.52-1.el7.x86_64                                                                                                                             5/13 
  Verifying  : perl-PlRPC-0.2020-14.el7.noarch                                                                                                                                6/13 
  Verifying  : 1:perl-Compress-Raw-Zlib-2.061-4.el7.x86_64                                                                                                                    7/13 
  Verifying  : perl-Net-Daemon-0.48-5.el7.noarch                                                                                                                              8/13 
  Verifying  : perl-DBI-1.627-4.el7.x86_64                                                                                                                                    9/13 
  Verifying  : libaio-0.3.109-13.el7.x86_64                                                                                                                                  10/13 
  Verifying  : perl-DBD-MySQL-4.023-5.el7.x86_64                                                                                                                             11/13 
  Verifying  : perl-IO-Compress-2.061-2.el7.noarch                                                                                                                           12/13 
  Verifying  : 1:mariadb-libs-5.5.50-1.el7_2.x86_64                                                                                                                          13/13 

Installed:
  mariadb-server.x86_64 1:5.5.52-1.el7                                                                                                                                             

Dependency Installed:
  libaio.x86_64 0:0.3.109-13.el7          mariadb.x86_64 1:5.5.52-1.el7         perl-Compress-Raw-Bzip2.x86_64 0:2.061-3.el7     perl-Compress-Raw-Zlib.x86_64 1:2.061-4.el7    
  perl-DBD-MySQL.x86_64 0:4.023-5.el7     perl-DBI.x86_64 0:1.627-4.el7         perl-Data-Dumper.x86_64 0:2.145-3.el7            perl-IO-Compress.noarch 0:2.061-2.el7          
  perl-Net-Daemon.noarch 0:0.48-5.el7     perl-PlRPC.noarch 0:0.2020-14.el7    

Dependency Updated:
  mariadb-libs.x86_64 1:5.5.52-1.el7                                                                                                                                               

Complete!
[root@ip-172-31-2-184 ~]# service mariadb stop
Redirecting to /bin/systemctl stop  mariadb.service
[root@ip-172-31-2-184 ~]# 
[root@ip-172-31-2-184 ~]# mv /var/lib/mysql/ib_logfile0 ~/
mv: cannot stat ‘/var/lib/mysql/ib_logfile0’: No such file or directory
[root@ip-172-31-2-184 ~]# mv /var/lib/mysql/ib_logfile1 ~/
mv: cannot stat ‘/var/lib/mysql/ib_logfile1’: No such file or directory
[root@ip-172-31-2-184 ~]# 
[root@ip-172-31-2-184 ~]# chkconfig mariadb on
Note: Forwarding request to 'systemctl enable mariadb.service'.
Created symlink from /etc/systemd/system/multi-user.target.wants/mariadb.service to /usr/lib/systemd/system/mariadb.service.
[root@ip-172-31-2-184 ~]# service mariadb start
Redirecting to /bin/systemctl start  mariadb.service
[root@ip-172-31-2-184 ~]# service mariadb status
Redirecting to /bin/systemctl status  mariadb.service
● mariadb.service - MariaDB database server
   Loaded: loaded (/usr/lib/systemd/system/mariadb.service; enabled; vendor preset: disabled)
   Active: active (running) since Thu 2017-05-04 19:09:47 EDT; 26ms ago
  Process: 2789 ExecStartPost=/usr/libexec/mariadb-wait-ready $MAINPID (code=exited, status=0/SUCCESS)
  Process: 2709 ExecStartPre=/usr/libexec/mariadb-prepare-db-dir %n (code=exited, status=0/SUCCESS)
 Main PID: 2788 (mysqld_safe)
   CGroup: /system.slice/mariadb.service
           ├─2788 /bin/sh /usr/bin/mysqld_safe --basedir=/usr
           └─2946 /usr/libexec/mysqld --basedir=/usr --datadir=/var/lib/mysql --plugin-dir=/usr/lib64/mysql/plugin --log-error=/var/log/mariadb/mariadb.log --pid-file=/var/run/...

May 04 19:09:45 manager.alanbluwol mariadb-prepare-db-dir[2709]: See the MariaDB Knowledgebase at http://mariadb.com/kb or the
May 04 19:09:45 manager.alanbluwol mariadb-prepare-db-dir[2709]: MySQL manual for more instructions.
May 04 19:09:45 manager.alanbluwol mariadb-prepare-db-dir[2709]: You can start the MariaDB daemon with:
May 04 19:09:45 manager.alanbluwol mariadb-prepare-db-dir[2709]: cd '/usr' ; /usr/bin/mysqld_safe --datadir='/var/lib/mysql'
May 04 19:09:45 manager.alanbluwol mariadb-prepare-db-dir[2709]: You can test the MariaDB daemon with mysql-test-run.pl
May 04 19:09:45 manager.alanbluwol mariadb-prepare-db-dir[2709]: cd '/usr/mysql-test' ; perl mysql-test-run.pl
May 04 19:09:45 manager.alanbluwol mariadb-prepare-db-dir[2709]: Please report any problems at http://mariadb.org/jira
May 04 19:09:45 manager.alanbluwol mysqld_safe[2788]: 170504 19:09:45 mysqld_safe Logging to '/var/log/mariadb/mariadb.log'.
May 04 19:09:45 manager.alanbluwol mysqld_safe[2788]: 170504 19:09:45 mysqld_safe Starting mysqld daemon with databases from /var/lib/mysql
May 04 19:09:47 manager.alanbluwol systemd[1]: Started MariaDB database server.
[root@ip-172-31-2-184 ~]# 
[root@ip-172-31-2-184 ~]# /usr/bin/mysql_secure_installation

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none): 
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

Set root password? [Y/n] y
New password: 
Re-enter new password: 
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] n
 ... skipping.

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!

[root@ip-172-31-2-184 ~]# cp /etc/my.cnf ~/bk.my.cnf
[root@ip-172-31-2-184 ~]# 
[root@ip-172-31-2-184 ~]# cat << 'EOF' > /etc/my.cnf
> [mysqld]
> server_id=198
> log-bin=mysqld-bin
> bind-address=0.0.0.0
> datadir=/var/lib/mysql
> socket=/var/lib/mysql/mysql.sock
> default-storage-engine=innodb
> sql_mode=STRICT_ALL_TABLES
> key_buffer_size = 32M
> max_allowed_packet = 32M
> thread_stack = 256K
> thread_cache_size = 64
> query_cache_limit = 8M
> query_cache_size = 64M
> query_cache_type = 1
> max_connections = 550
> binlog_format = row
> read_buffer_size = 2M
> read_rnd_buffer_size = 16M
> sort_buffer_size = 8M
> join_buffer_size = 8M
> symbolic-links=0
> innodb_file_per_table = 1
> innodb_flush_log_at_trx_commit = 2
> innodb_log_buffer_size = 64M
> innodb_buffer_pool_size = 4G
> innodb_thread_concurrency = 8
> innodb_flush_method = O_DIRECT
> 
> [mysqld_safe]
> EOF
[root@ip-172-31-2-184 ~]# 
[root@ip-172-31-2-184 ~]# service mariadb restart
Redirecting to /bin/systemctl restart  mariadb.service
[root@ip-172-31-2-184 ~]# mysql -u root -pcloudera
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 2
Server version: 5.5.52-MariaDB MariaDB Server

Copyright (c) 2000, 2016, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> set global binlog_format = 'ROW';
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> flush tables with read lock;
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> unlock tables;
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> show variables like 'binlog_format';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| binlog_format | ROW   |
+---------------+-------+
1 row in set (0.00 sec)

MariaDB [(none)]> create database scm; -- Cloudera Manager
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> create database hive; -- Hive Metastore
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> create database hue; -- Hue Metastore
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> create database rman; -- Reports Manager
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> create database oozie; -- Oozie database
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> create database sentry; -- Sentry database
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> create user cloudera identified by 'cloudera';
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> grant all on *.* to 'cloudera'@'%' identified by 'cloudera';
Query OK, 0 rows affected (0.00 sec)

MariaDB [(none)]> exit
Bye
[root@ip-172-31-2-184 ~]# mysql --version
mysql  Ver 15.1 Distrib 5.5.52-MariaDB, for Linux (x86_64) using readline 5.1
[root@ip-172-31-2-184 ~]# mysql -uroot -pcloudera -e "SELECT * FROM information_schema.user_privileges WHERE privilege_type = 'usage';"
[root@ip-172-31-2-184 ~]# mysql -uroot -pcloudera -e "SELECT user, host FROM mysql.user;"
+----------+--------------------+
| user     | host               |
+----------+--------------------+
| cloudera | %                  |
| root     | 127.0.0.1          |
| root     | ::1                |
| root     | localhost          |
| root     | manager.alanbluwol |
+----------+--------------------+
[root@ip-172-31-2-184 ~]# mysql -uroot -pcloudera -e "SHOW DATABASES;"
+--------------------+
| Database           |
+--------------------+
| information_schema |
| hive               |
| hue                |
| mysql              |
| oozie              |
| performance_schema |
| rman               |
| scm                |
| sentry             |
+--------------------+


[root@ip-172-31-5-118 ~]# wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.41.tar.gz
--2017-05-04 19:04:47--  https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-5.1.41.tar.gz
Resolving dev.mysql.com (dev.mysql.com)... 137.254.60.11
Connecting to dev.mysql.com (dev.mysql.com)|137.254.60.11|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://cdn.mysql.com//Downloads/Connector-J/mysql-connector-java-5.1.41.tar.gz [following]
--2017-05-04 19:04:49--  https://cdn.mysql.com//Downloads/Connector-J/mysql-connector-java-5.1.41.tar.gz
Resolving cdn.mysql.com (cdn.mysql.com)... 104.116.130.102
Connecting to cdn.mysql.com (cdn.mysql.com)|104.116.130.102|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3923677 (3.7M) [application/x-tar-gz]
Saving to: ‘mysql-connector-java-5.1.41.tar.gz’

100%[=========================================================================================================================================>] 3,923,677   --.-K/s   in 0.05s   

2017-05-04 19:04:49 (75.2 MB/s) - ‘mysql-connector-java-5.1.41.tar.gz’ saved [3923677/3923677]

# On all nodes

[root@ip-172-31-5-118 ~]# tar -zxvf mysql-connector-java-5.1.41.tar.gz
[root@ip-172-31-5-118 ~]# mkdir -p /usr/share/java/
[root@ip-172-31-5-118 ~]# cp mysql-connector-java-5.1.41/mysql-connector-java-5.1.41-bin.jar /usr/share/java/mysql-connector-java.jar

```

