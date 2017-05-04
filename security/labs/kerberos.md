# Services Enablement Boot Camp

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