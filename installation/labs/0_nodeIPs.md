# Services Enablement Boot Camp

# Lab 0: Node IPs

### Host Configuration

Script

```sh
getent hosts
```

Output

```sh
[root@ip-172-31-1-85 ~]# getent hosts
127.0.0.1       localhost localhost.localdomain localhost4 localhost4.localdomain4
127.0.0.1       localhost localhost.localdomain localhost6 localhost6.localdomain6
172.31.1.85     manager.alanbluwol manager manager ip-172-31-1-85
172.31.5.116    node1.alanbluwol node1 master1 ip-172-31-5-116
172.31.7.189    node2.alanbluwol node2 worker1 ip-172-31-7-189
172.31.13.46    node3.alanbluwol node3 worker2 ip-172-31-13-46
172.31.15.253   node4.alanbluwol node4 worker3 ip-172-31-15-253
```