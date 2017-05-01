# Services Enablement Boot Camp

# Lab 2: Replica Working

## Prepare Database

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
