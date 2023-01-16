### LightDB分布式部署-多机单实例模式

- #### 分布式部署模式
```sql
# 1.常规模式
    1台服务器作为协调者节点，N(N>1)台服务器作为工作节点,每个节点都按照高可用方式进行部署
    比如，一个协调节点，2个工作节点，
    * 工作节点按照一主一从高可用方式部署，则一共需要[(1+2)*2=6台]服务器
    * 工作节点按照一主一从一witness方式部署，则一共需要[(1+2)*3=9台]服务器    
# 2.多机单实例
    1台服务器作为协调者节点，N(N>1)台服务器作为工作节点,每个节点都按照单机模式部署
# 3.单机多实例
    使用1台服务器同时安装协调者节点和工作节点的实例，所有实例均按照单机模式部署

生产环境仅建议使用常规模式部署。
```

- #### 机器列表
```sql
1. 192.168.121.112 --CN
2. 192.168.121.113 --work1
3. 192.168.121.114 --work2
```

- #### 命令行安装过程
```sql
[lightdb@localhost lightdb-x-13.8-22.3-7953-el7.x86_64]$ ./install.sh 
Whether to use the graphical user interface (GUI, Make sure DISPLAY is configured, Such as [export DISPLAY=127.0.0.1:0.0])?(Yes or No)
no
Choice a kind of configuration mode!
1: Only install.
2: Install database and Create instance.
3: Developer
Please enter 1 2 or 3(The default is 1):
2
Choice a kind of install mode!

1: Single Mode.
2: High Availability Mode
3: Distributed Mode
Please enter 1, 2 or 3:(The default is 1)
3
Please select distributed mode!
Please enter distributed mode:
1 Normal mode;
2 Multi-server single instance;
3 Single server multi-instance.(Default 1)
2
Configure the directory and port for the single-node multi-instance.
Please enter coordinator node:
Please enter the ip of coordinator node, such as 192.168.217.234:
192.168.121.112
Please enter the port of coordinator node. (Default port 5432):
5437
Please enter worker node:
Please enter the ip of worker node, such as 192.168.217.234:
192.168.121.113
Please enter the port of worker node. (Default port 5432):
5438
Please enter the ip of worker node, such as 192.168.217.234:
192.168.121.114
Please enter the port of worker node. (Default port 5432):
5439
Continue to add worker node?(1[yes],2[no])
2
======================================= Servers =======================================
Coordinator Node : 192.168.121.112
Worker Node : 192.168.121.113
Worker Node : 192.168.121.114
======================================= Servers =======================================
Please check the servers of Multi-server single instance. 1 continue; 2 clear server config (Default 1)
1
====================================== Generate pg_hba ================================
====================================== Generate end ===================================
====================================== Copying files takes time =======================
192.168.121.113: start copy
192.168.121.113: end copy
192.168.121.114: start copy
192.168.121.114: end copy
====================================== Copying files end ==============================
Check system parameters and dependency packages!
========================================= 192.168.121.112 =========================================
NETWORK
name: sem, recommend value: 500,2048000,200,4096, current value: 500,2048000,200,4096, status: OK
name: aio_max_nr, recommend value: 1048576, current value: 1048576, status: OK
name: somaxconn, recommend value: 2000, current value: 2000, status: OK
name: tcp_max_syn_backlog, recommend value: 2000, current value: 2000, status: OK
name: tcp_tw_reuse, recommend value: 1, current value: 1, status: OK
name: tcp_syn_retries, recommend value: 3, current value: 3, status: OK
name: tcp_retries2, recommend value: 5, current value: 5, status: OK
name: tcp_slow_start_after_idle, recommend value: 0, current value: 0, status: OK
PAGE_CACHE
name: dirty_background_ratio, recommend value: 5, current value: 5, status: OK
name: dirty_ratio, recommend value: 40, current value: 40, status: OK
name: dirty_expire_centisecs, recommend value: 500, current value: 500, status: OK
name: dirty_writeback_centisecs, recommend value: 250, current value: 250, status: OK
MEMORY
name: shmmni, recommend value: 4096, current value: 4096, status: OK
name: shmmax, recommend value: 1976979456, current value: 1976979456, status: OK
name: shmall, recommend value: 482661, current value: 482661, status: OK
name: swappiness, recommend value: 5, current value: 5, status: OK
name: overcommit_memory, recommend value: 2, current value: 2, status: OK
name: overcommit_ratio, recommend value: 75, current value: 75, status: OK
FILE_HANDLER
name: file_max, recommend value: 524288, current value: 524288, status: OK
ULIMIT
name: ulimit_core, recommend value: unlimited, current value: unlimited, status: OK
name: ulimit_nofile, recommend value: 8192, current value: 524288, status: OK
Dependency Package
name: JSON-C-0.11 is existed: yes
name: C-ARES-1 is existed: yes
name: LIBNL3 is existed: yes
name: LIBPCAP-1 is existed: yes
name: LIBZSTD-1 is existed: yes
name: LZ4-1 is existed: yes
name: NCURSES-LIBS-5 is existed: yes
name: READLINE-6 is existed: yes
name: SNAPPY-1 is existed: yes
name: UUID-1.6 is existed: yes
name: LIBICU-50 is existed: yes
System service
name: ntp is install: no
========================================= 192.168.121.113 =========================================
NETWORK
name: sem, recommend value: 500,2048000,200,4096, current value: 500,2048000,200,4096, status: OK
name: aio_max_nr, recommend value: 1048576, current value: 1048576, status: OK
name: somaxconn, recommend value: 2000, current value: 2000, status: OK
name: tcp_max_syn_backlog, recommend value: 2000, current value: 2000, status: OK
name: tcp_tw_reuse, recommend value: 1, current value: 1, status: OK
name: tcp_syn_retries, recommend value: 3, current value: 3, status: OK
name: tcp_retries2, recommend value: 5, current value: 5, status: OK
name: tcp_slow_start_after_idle, recommend value: 0, current value: 0, status: OK
PAGE_CACHE
name: dirty_background_ratio, recommend value: 5, current value: 5, status: OK
name: dirty_ratio, recommend value: 40, current value: 40, status: OK
name: dirty_expire_centisecs, recommend value: 500, current value: 500, status: OK
name: dirty_writeback_centisecs, recommend value: 250, current value: 250, status: OK
MEMORY
name: shmmni, recommend value: 4096, current value: 4096, status: OK
name: shmmax, recommend value: 1976979456, current value: 1976979456, status: OK
name: shmall, recommend value: 482661, current value: 482661, status: OK
name: swappiness, recommend value: 5, current value: 5, status: OK
name: overcommit_memory, recommend value: 2, current value: 2, status: OK
name: overcommit_ratio, recommend value: 75, current value: 75, status: OK
FILE_HANDLER
name: file_max, recommend value: 524288, current value: 524288, status: OK
ULIMIT
name: ulimit_core, recommend value: unlimited, current value: unlimited, status: OK
name: ulimit_nofile, recommend value: 8192, current value: 524288, status: OK
Dependency Package
name: JSON-C-0.11 is existed: yes
name: C-ARES-1 is existed: yes
name: LIBNL3 is existed: yes
name: LIBPCAP-1 is existed: yes
name: LIBZSTD-1 is existed: yes
name: LZ4-1 is existed: yes
name: NCURSES-LIBS-5 is existed: yes
name: READLINE-6 is existed: yes
name: SNAPPY-1 is existed: yes
name: UUID-1.6 is existed: yes
name: LIBICU-50 is existed: yes
System service
name: ntp is install: no
========================================= 192.168.121.114 =========================================
NETWORK
name: sem, recommend value: 500,2048000,200,4096, current value: 500,2048000,200,4096, status: OK
name: aio_max_nr, recommend value: 1048576, current value: 1048576, status: OK
name: somaxconn, recommend value: 2000, current value: 2000, status: OK
name: tcp_max_syn_backlog, recommend value: 2000, current value: 2000, status: OK
name: tcp_tw_reuse, recommend value: 1, current value: 1, status: OK
name: tcp_syn_retries, recommend value: 3, current value: 3, status: OK
name: tcp_retries2, recommend value: 5, current value: 5, status: OK
name: tcp_slow_start_after_idle, recommend value: 0, current value: 0, status: OK
PAGE_CACHE
name: dirty_background_ratio, recommend value: 5, current value: 5, status: OK
name: dirty_ratio, recommend value: 40, current value: 40, status: OK
name: dirty_expire_centisecs, recommend value: 500, current value: 500, status: OK
name: dirty_writeback_centisecs, recommend value: 250, current value: 250, status: OK
MEMORY
name: shmmni, recommend value: 4096, current value: 4096, status: OK
name: shmmax, recommend value: 1976979456, current value: 1976979456, status: OK
name: shmall, recommend value: 482661, current value: 482661, status: OK
name: swappiness, recommend value: 5, current value: 5, status: OK
name: overcommit_memory, recommend value: 2, current value: 2, status: OK
name: overcommit_ratio, recommend value: 75, current value: 75, status: OK
FILE_HANDLER
name: file_max, recommend value: 524288, current value: 524288, status: OK
ULIMIT
name: ulimit_core, recommend value: unlimited, current value: unlimited, status: OK
name: ulimit_nofile, recommend value: 8192, current value: 524288, status: OK
Dependency Package
name: JSON-C-0.11 is existed: yes
name: C-ARES-1 is existed: yes
name: LIBNL3 is existed: yes
name: LIBPCAP-1 is existed: yes
name: LIBZSTD-1 is existed: yes
name: LZ4-1 is existed: yes
name: NCURSES-LIBS-5 is existed: yes
name: READLINE-6 is existed: yes
name: SNAPPY-1 is existed: yes
name: UUID-1.6 is existed: yes
name: LIBICU-50 is existed: yes
System service
name: ntp is install: no
Choice a kind of Compatible Type!
1: LightdDB(Compatible with PostgreSQL).
2: ORACLE(Compatible with ORACLE).
3: MYSQL(Compatible with MYSQL).
Please enter 1, 2 or 3:(The default is 1)
1
Choice a kind of LightDB workload!
1: OLTP(On-line Transaction Processing).
2: OLAP(On-Line Analytical Processing).
Please enter 1 or 2:(The default is 1)
1
Specify a path for installing all LightDB software and storing configuration information.
Please enter base location(The default is /usr/local/lightdb):
/usr/local/lightdb-dis-multi
Base Location: /usr/local/lightdb-dis-multi
Install Location: /usr/local/lightdb-dis-multi/lightdb-x/13.8-22.3
Please enter instance location(The default is /usr/local/lightdb-dis-multi/lightdb-x/13.8-22.3/data/defaultCluster/):

Instance location: /usr/local/lightdb-dis-multi/lightdb-x/13.8-22.3/data/defaultCluster/
Please configure memory(MB) and character set!
Please enter shared_buffers, Default value is (942):

Please enter effective_cache_size, Default value is (2639):

Please choice a kind of Character Set.
1. UTF8
2. GBK
3. SQL_ASCII
4. LATIN1
The default choice 1(UTF8)
1
Please enter LightDB password!
Please enter original password:

Please enter confirm password:

Do you want to deploy immediately?(Yes or No, The default is yes)
yes
[>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>]100%
Ntp Server
Enter the ip address or domain name of the ntp server, Allowed to change.(Default ntp ip:192.168.121.112)

Execute follow commands as root
Execute follow commands to start ntp:

sh /home/lightdb/lightdb-x-13.8-22.3-7953-el7.x86_64/script/13_ntp_start.sh /usr/local/lightdb-dis-multi/lightdb-x/13.8-22.3

Check service:
Please enter any key to continue for checking?

192.168.121.113 ntp service not start or problem!
192.168.121.114 ntp service not start or problem!

Check service:
Please enter any key to continue for checking?
1^H
Choose whether to install LVS to load balance coordinator node ?(Yes or No,The default is no)
no
Install Finish
yes
[>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>]100%
Ntp Server
Enter the ip address or domain name of the ntp server, Allowed to change.(Default ntp ip:192.168.121.112)

Execute follow commands as root
Execute follow commands to start ntp:

sh /home/lightdb/lightdb-x-13.8-22.3-7953-el7.x86_64/script/13_ntp_start.sh /usr/local/lightdb-dis-multi/lightdb-x/13.8-22.3

Check service:
Please enter any key to continue for checking?

192.168.121.113 ntp service not start or problem!
192.168.121.114 ntp service not start or problem!

Check service:
Please enter any key to continue for checking?
1^H
Choose whether to install LVS to load balance coordinator node ?(Yes or No,The default is no)
no
Install Finish

```

- #### 查看安装进程
```sql
[lightdb@localhost lightdb-x-13.8-22.3-7953-el7.x86_64]$ ps aux|grep lightdb
root       1594  0.0  0.0 191892  2348 pts/0    S    11:14   0:00 su - lightdb
lightdb    1595  0.0  0.0 115544  2128 pts/0    S    11:14   0:00 -bash
root       3493  0.0  0.0 191892  2348 pts/1    S    12:16   0:00 su - lightdb
lightdb    3494  0.0  0.0 115544  2088 pts/1    S    12:16   0:00 -bash
root       7162  0.0  0.0 191892  2348 pts/1    S    13:16   0:00 su - lightdb
lightdb    7163  0.0  0.0 115544  2088 pts/1    S    13:16   0:00 -bash
lightdb   11974  0.6  9.5 1708684 368848 ?      Ss   14:00   0:04 /usr/local/lightdb-dis-multi/lightdb-x/13.8-22.3/bin/lightdb -D /usr/local/lightdb-dis-multi/lightdb-x/13.8-22.3/data/defaultCluster
lightdb   11977  0.0  0.1 292224  4688 ?        Ss   14:00   0:00 lightdb: logger 
lightdb   11979  0.0  0.1 1708564 4976 ?        Ss   14:00   0:00 lightdb: checkpointer 
lightdb   11980  0.1  0.4 1710096 17056 ?       Ss   14:00   0:01 lightdb: background writer 
lightdb   11981  0.0  3.5 1708564 136388 ?      Ss   14:00   0:00 lightdb: walwriter 
lightdb   11982  0.0  0.2 1712684 8340 ?        Ss   14:00   0:00 lightdb: autovacuum launcher 
lightdb   11983  0.0  0.1 294988  5280 ?        Ss   14:00   0:00 lightdb: stats collector 
lightdb   11984  0.3  0.5 1713772 22728 ?       Ss   14:00   0:02 lightdb: lt_cron launcher 
lightdb   11985  0.0  0.1 1712320 7720 ?        Ss   14:00   0:00 lightdb: logical replication launcher 
lightdb   11986  0.0  0.4 1715756 17992 ?       Ss   14:00   0:00 lightdb: Canopy Maintenance Daemon: 14198/10 
root      12378  0.0  0.0  47120  2380 ?        Ss   14:03   0:00 /usr/local/lightdb-dis-multi/lightdb-x/13.8-22.3/tools/bin/ntpd -g -c /usr/local/lightdb-dis-multi/lightdb-x/13.8-22.3/etc/ntp/ntp.conf
lightdb   12905  0.0  0.3 1713244 11992 ?       Ss   14:09   0:00 lightdb: Canopy Maintenance Daemon: 25717/10 
lightdb   12985  0.0  0.4 1717192 18604 ?       Ss   14:11   0:00 lightdb: lightdb postgres 192.168.121.114(47264) idle
lightdb   12988  0.0  0.4 1717192 18604 ?       Ss   14:11   0:00 lightdb: lightdb postgres 192.168.121.113(39916) idle

```
- #### 数据库访问
```sql
[lightdb@localhost ~]$ ltsql -h 192.168.121.112 -p 5437
[lightdb@localhost ~]$ ltsql -h 192.168.121.113 -p 5438
[lightdb@localhost ~]$ ltsql -h 192.168.121.114 -p 5439
```