# Services Enablement Boot Camp

# 1 - Database Server

* Hostname
* MySQL Version
* Show Databaes

```sh
[root@ip-172-31-2-184 ~]# hostname
manager.alanbluwol

[root@ip-172-31-2-184 ~]# mysql --version
mysql  Ver 15.1 Distrib 5.5.52-MariaDB, for Linux (x86_64) using readline 5.1

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
```