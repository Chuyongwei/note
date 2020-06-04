```ini
[client]
port = 3306
socket = /tmp/mysql.sock
default-character-set = utf8
[mysql]
no-auto-rehash                                            #仅允许使用键值的updates和deletes
[mysqld]
port = 3306                                                #msyql服务器端口号
basedir = /usr/local/mysql                                 #mysql安装目录
datadir = /usr/local/mysql/data                            #mysql数据存放目录
#datadir = /data/mysql/data                                #同上
socket = /usr/local/mysql/data/mysql.sock                  #sock文件
 
#字符集与校对规则
character-set-server = utf8                                #默认字符集
collation-server = utf8_general_ci                         #设置校对规则
 
external-locking = FALSE                                   #避免外部锁定(减少出错几率，增加稳定性)
skip-name-resolv                                           #禁止外部连接进行DNS解析
skip-slave-start                                           # 复制进程就不会随着数据库的启动而启动 http://blog.csdn.net/aeolus_pu/article/details/9419965 
#master库binlog参数相关
server-id = 1                                              #主从复制时，ID不能相同
#binlog_format = mixed                                     #二进制日志格式（mixed、row、statement）
binlog-cache-size = 32M                                    #设置二进制日志缓存大小
sync-binlog = 1                                            #每隔N秒将缓存中的二进制日志记录写回硬盘
max_binlog_cache_size = 8M                                 #最大的二进制Cache日志缓冲尺寸
max_binlog_size = 1G                                       #单个二进制日志文件的最大值，默认1G，最大1G
log-bin-index = /usr/local/mysql/data/mysql-bin.index      #binlog索引文件位置
log-bin = /usr/local/mysql/data/mysql-bin                  #binlog日志存放目录
expire_logs_days = 90                                      #二进制日志文件过期时间
#slave数据库binlog参数
server-id = 10                                             #各数据库id不能相同
log_slave_updates = 1                                      #级联也使用
relay-log = /usr/lcoal/mysql/data/relay-bin                #relady目录
relay-log-info-file = /usr/local/mysql/data/relay-log.info #info目录
slave-skip-errors = 1007,1008,1032,1062                    #跳过主从复制时的错误
read-only = 1                                              #从服务器只读,SQL线程不影响，具有super，root用户不限制
master-connect-retry = 60                                  #主从复制丢失，重连间隔时间，默认60s
#replicate-ignore-db = mysql                               #忽略mysql库不同步
replicate-wild-do-table=testdb1.%
replicate-wild-do-table=testdb2.%
replicate-wild-do-table=testdb3.%
#master半同步开启参数
rpl_semi_sync_master_enabled = ON
rpl_semi_sync_master_timeout = 10000
#rpl_semi_sync_master_wait_no_slave = ON
#rpl_semi_sync_master_trace_level = 32
#slave半同步开启参数
rpl_semi_sync_slave_enabled = ON
#rpl_semi_sync_slave_trace_level = 32
 
back_log = 1000                                          #指出在MySQL暂时停止响应新请求之前，短时间内的多少个请求
open_files_limit = 1024                                  #打开文件的最大个数，如果出现too mantopen files之类的就需要调整该值了
 
#连接相关
max_connections = 2000                                   #指定MySQL允许的最大连接进程数，show global variables like '%connections%'; http://elf8848.iteye.com/blog/1847445 
max_user_connections = 2000                              #单用户最大的连接数，max_user_connections < 实例 max_user_connections < max_connections
max_connect_errors = 100000                              #默认为10，设置每个主机的连接请求异常中断的最大次数，超过后会blocked，连接成功后初始0，出现错误后需要flush hosts
 
max_allowed_packet = 8M                                  #服务器一次能处理的最大的查询包的值
wait_timeout = 360                                       #指定一个请求的最大连接时间
interactive_timeout = 360                                #连接保持活动的时间
#访问日志
#general_log = on
#general_log_file = /usr/local/mysql/data/mysql_access.log
 
#错误日志
log_error = /data/mysql/data/mysql_error.log
#慢查询相关参数
slow_query_log = on                                    #开启慢查询
log-queries-not-using-indexes                          #记录所有没有使用到索引的查询语句
long_query_time = 2                                    #指定多少秒未返回结果的查询属于慢查询
min_examined_row_limit = 5                             #记录那些由于查找了多余5次而引发的慢查询
log-slow-admin-statements                              #记录那些慢的OPTIMIZE TABLE,ANALYZE TABLE和ALTER TABLE语句
log-slow-slave-statements                              #记录由slave所产生的慢查询
slow_query_log_file = /usr/local/mysql/data/slow.log   #指定慢查询日志文件路径
table_cache = 614                                      #表分配的内存，物理内存越大，设置就越大
table_open_cache = 512                                 #设置高速缓存表的数目
thread_cache_size = 64                                 #服务器线程缓存数，与内存大小有关(建议大于3G设置为64)
thread_concurrency = 32                                #CPU核数的两倍
query_cache_size = 32M                                 #指定MySQL查询缓冲区的大小
query_cache_limit = 2M                                 #只有小于此设置值的结果才会被缓存
query_cache_min_res_unit = 2k                          #设置查询缓存分配内存的最小单位
key_buffer_size = 512M                                #指定用于索引的缓冲区大小，增加它可得到更好的索引处理性能
sort_buffer_size = 2M                                 #设置查询排序时所能使用的缓冲区大小，系统默认大小为2MB
join_buffer_size = 1M                                 #联合查询操作所能使用的缓冲区大小
read_buffer_size = 4M                                 #读查询操作所能使用的缓冲区大小
read_rnd_buffer_size = 16M                            #设置进行随机读的时候所使用的缓冲区
thread_stack = 192K                                   #设置Mysql每个线程的堆栈大小，默认值足够大，可满足普通操作
bulk_insert_buffer_size = 8M                          #可以适当调整参数至16MB~32MB，建议8MB
#myisam参数引擎相关
myisam_sort_buffer_size = 128M
myisam_max_sort_file_size = 10G
myisam_repair_threads = 1
myisam_recover                                       #自动检查和修复没有适当关闭的MyISAM表
key_buffer_size = 16M                                #myisam索引buffer，只有key没有data
 
transaction_isolation = READ-COMMITTED              #事务隔离级别
tmp_table_size = 64M                                #设置内存临时表最大值
max_heap_table_size = 64M                           #独立的内存表所允许的最大容量
#innodb引擎参数相关
default-storage-engine=InnoDB                      #默认表的类型为InnoDB
innodb_old_blocks_time =1000                       #减小单次的大批量数据查询,默认为0，调整后性能提升80%                     http://www.cnblogs.com/cenalulu/archive/2012/10/10/2718585.html 
innodb_flush_method = O_DIRECT                     #从innode刷新到磁盘，不经过系统write,fdatasync(默认)，O_DSYNC，O_DIRECT http://blog.csdn.net/jiao_fuyou/article/details/16113403 
innodb_additional_mem_pool_size = 16M              #设置InnoDB存储的数据目录信息和其他内部数据结构的内存池大小
innodb_buffer_pool_size = 51G                      #InnoDB使用一个缓冲池来保存索引和原始数据，官方建议物理内存的80%
innodb_data_file_path = ibdata1:128M:autoextend    #表空间
innodb_file_io_threads = 4                         #InnoDB中的文件I/O线程，通常设置为4，innodb除master线程外，还有insert buffer, log, read, write这4种线程，默认各有一个
innodb_read_io_threads = 8
innodb_write_io_threads = 8
innodb_thread_concurrency = 8                      #服务器有几个CPU就设置为几，建议用默认设置，一般设为8
innodb_flush_log_at_trx_commit = 2                 #设置为0就等于innodb_log_buffer_size队列满后再统一存储，默认为1
innodb_log_buffer_size = 16M                       #默认为1MB，通常设置为6-8MB就足够
innodb_log_file_size = 512M                        #确定日志文件的大小，更大的设置可以提高性能，但也会增加恢复数据库的时间
innodb_log_files_in_group = 3                      #为提高性能，MySQL可以以循环方式将日志文件写到多个文件。推荐设置为3
innodb_max_dirty_pages_pct = 90                    #InnoDB主线程刷新缓存池中的数据
innodb_lock_wait_timeout = 120                     #InnoDB事务被回滚之前可以等待一个锁定的超时秒数
innodb_file_per_table = 1                          #InnoDB为独立表空间模式，每个数据库的每个表都会生成一个数据空间,0关闭,1开启
innodb_autoextend_increment = 256                  #这个参数的作用是控制innodb 共享表空间文件自动扩展的大小
[mysqldump]
quick
max_allowed_packet = 64M
[mysqld_safe]
log-error = /usr/local/mysql/data/mysql.err
pid-file = /usr/local/mysql/data/mysqld.pid
 
查询innodb分配资源
mysql> show engine innodb status;
----------------------
BUFFER POOL AND MEMORY
----------------------
Total memory allocated 137363456; in additional pool allocated 0
Dictionary memory allocated 59957
Buffer pool size   8191
Free buffers       8028
Database pages     163
Old database pages 0
Modified db pages  0
Pending reads 0
Pending writes: LRU 0, flush list 0, single page 0
Pages made young 0, not young 0
0.00 youngs/s, 0.00 non-youngs/s
Pages read 163, created 0, written 1
0.00 reads/s, 0.00 creates/s, 0.00 writes/s
No buffer pool page gets since the last printout
Pages read ahead 0.00/s, evicted without access 0.00/s, Random read ahead 0.00/s
LRU len: 163, unzip_LRU len: 0
I/O sum[0]:cur[0], unzip sum[0]:cur[0]
 
 
 
我们使用的是专用于MySQL的（5.5 Percona的）上运行CentOS的（不同口味），主要128GB的服务器。我们innodb_buffer_pool_size设置为104GB这些和他们也有/ tmp目录8GB的内存磁盘。他们被大量使用，但从未使用过任何的交换。当然vm.swappiness设置为1，
下面就是我们用较低层的服务器（包括128GB为简洁起见），他们又从来没有使用过任何掉期和运行大型数据库（2-3TB）：
128GB RAM：innodb_buffer_pool_size = 104GB
64GB RAM：innodb_buffer_pool_size = 56G
32GB RAM：innodb_buffer_pool_size = 28G
 
在大多数情况下，我们分配（N - 7G）* 0.9。所以对于一个64G的节点，我们最终分配给缓冲池内存〜51G
64GB RAM：innodb_buffer_pool_size = 51G
 
https://www.percona.com/blog/2015/06/02/80-ram-tune-innodb_buffer_pool_size/ 
http://osxr.org:8080/mysql/ident?_i=back_log&_remember=1 
 
 
王导DBA MySQL优化
log_error = localhost3306.err
sync_binlog=1
innodb_old_blocks_time =1000
innodb_flush_method = O_DIRECT
back_log=1000
max_connections = 2000
max_user_connections=2000
min_examined_row_limit =5
skip-slave-start
skip-name-resolve
max_connect_errors = 100000
character-set-server=utf8
collation-server=utf8_bin
binlog_cache_size=32M
query_cache_limit = 2M
tmp_table_size=256M
max_heap_table_size=256M
interactive_timeout=360
wait_timeout=360
log_slave_updates=1
expire_logs_days=60
binlog_format=mixed
tmpdir=/dev/shm
innodb_autoextend_increment = 256
innodb_buffer_pool_instances=8
innodb_additional_mem_pool_size=128M
innodb_max_dirty_pages_pct=80
innodb_read_io_threads = 8
innodb_write_io_threads = 8
innodb_log_file_size = 1G
innodb_log_files_in_group = 2
innodb_flush_log_at_trx_commit = 2
innodb_file_per_table=1
```



```ini
[client]
port = 3306
default-character-set = utf8
[mysql]
no-auto-rehash                                            #仅允许使用键值的updates和deletes
[mysqld]
port = 3306                                                #msyql服务器端口号
basedir = /usr/local/mysql                                 #mysql安装目录
datadir = /usr/local/mysql/data                            #mysql数据存放目录
#datadir = /data/mysql/data                                #同上
#socket = /usr/local/mysql/data/mysql.sock                  #sock文件
 
#字符集与校对规则
character-set-server = utf8                                #默认字符集
collation-server = utf8_general_ci                         #设置校对规则
 
external-locking = FALSE                                   #避免外部锁定(减少出错几率，增加稳定性)
skip-name-resolv                                           #禁止外部连接进行DNS解析
skip-slave-start                                           # 复制进程就不会随着数据库的启动而启动 http://blog.csdn.net/aeolus_pu/article/details/9419965 
#master库binlog参数相关
server-id = 1                                              #主从复制时，ID不能相同
#binlog_format = mixed                                     #二进制日志格式（mixed、row、statement）
binlog-cache-size = 32M                                    #设置二进制日志缓存大小
sync-binlog = 1                                            #每隔N秒将缓存中的二进制日志记录写回硬盘
max_binlog_cache_size = 8M                                 #最大的二进制Cache日志缓冲尺寸
max_binlog_size = 1G                                       #单个二进制日志文件的最大值，默认1G，最大1G
#log-bin-index = /usr/local/mysql/data/mysql-bin.index      #binlog索引文件位置
#log-bin = /usr/local/mysql/data/mysql-bin                  #binlog日志存放目录
expire_logs_days = 90                                      #二进制日志文件过期时间
#slave数据库binlog参数
server-id = 10                                             #各数据库id不能相同
log_slave_updates = 1                                      #级联也使用
#relay-log = /usr/lcoal/mysql/data/relay-bin                #relady目录
#relay-log-info-file = /usr/local/mysql/data/relay-log.info #info目录
slave-skip-errors = 1007,1008,1032,1062                    #跳过主从复制时的错误
read-only = 1                                              #从服务器只读,SQL线程不影响，具有super，root用户不限制
master-connect-retry = 60                                  #主从复制丢失，重连间隔时间，默认60s
#replicate-ignore-db = mysql                               #忽略mysql库不同步
replicate-wild-do-table=testdb1.%
replicate-wild-do-table=testdb2.%
replicate-wild-do-table=testdb3.%
#master半同步开启参数
rpl_semi_sync_master_enabled = ON
rpl_semi_sync_master_timeout = 10000
#rpl_semi_sync_master_wait_no_slave = ON
#rpl_semi_sync_master_trace_level = 32
#slave半同步开启参数
rpl_semi_sync_slave_enabled = ON
#rpl_semi_sync_slave_trace_level = 32
 
back_log = 1000                                          #指出在MySQL暂时停止响应新请求之前，短时间内的多少个请求
open_files_limit = 1024                                  #打开文件的最大个数，如果出现too mantopen files之类的就需要调整该值了
 
#连接相关
max_connections = 2000                                   #指定MySQL允许的最大连接进程数，show global variables like '%connections%'; http://elf8848.iteye.com/blog/1847445 
max_user_connections = 2000                              #单用户最大的连接数，max_user_connections < 实例 max_user_connections < max_connections
max_connect_errors = 100000                              #默认为10，设置每个主机的连接请求异常中断的最大次数，超过后会blocked，连接成功后初始0，出现错误后需要flush hosts
 
max_allowed_packet = 8M                                  #服务器一次能处理的最大的查询包的值
wait_timeout = 360                                       #指定一个请求的最大连接时间
interactive_timeout = 360                                #连接保持活动的时间
#访问日志
#general_log = on
#general_log_file = /usr/local/mysql/data/mysql_access.log
 
#错误日志
#log_error = /data/mysql/data/mysql_error.log
#慢查询相关参数
slow_query_log = on                                    #开启慢查询
log-queries-not-using-indexes                          #记录所有没有使用到索引的查询语句
long_query_time = 2                                    #指定多少秒未返回结果的查询属于慢查询
min_examined_row_limit = 5                             #记录那些由于查找了多余5次而引发的慢查询
log-slow-admin-statements                              #记录那些慢的OPTIMIZE TABLE,ANALYZE TABLE和ALTER TABLE语句
log-slow-slave-statements                              #记录由slave所产生的慢查询
#slow_query_log_file = /usr/local/mysql/data/slow.log   #指定慢查询日志文件路径
table_cache = 614                                      #表分配的内存，物理内存越大，设置就越大
table_open_cache = 512                                 #设置高速缓存表的数目
thread_cache_size = 64                                 #服务器线程缓存数，与内存大小有关(建议大于3G设置为64)
thread_concurrency = 32                                #CPU核数的两倍
query_cache_size = 32M                                 #指定MySQL查询缓冲区的大小
query_cache_limit = 2M                                 #只有小于此设置值的结果才会被缓存
query_cache_min_res_unit = 2k                          #设置查询缓存分配内存的最小单位
key_buffer_size = 512M                                #指定用于索引的缓冲区大小，增加它可得到更好的索引处理性能
sort_buffer_size = 2M                                 #设置查询排序时所能使用的缓冲区大小，系统默认大小为2MB
join_buffer_size = 1M                                 #联合查询操作所能使用的缓冲区大小
read_buffer_size = 4M                                 #读查询操作所能使用的缓冲区大小
read_rnd_buffer_size = 16M                            #设置进行随机读的时候所使用的缓冲区
thread_stack = 192K                                   #设置Mysql每个线程的堆栈大小，默认值足够大，可满足普通操作
bulk_insert_buffer_size = 8M                          #可以适当调整参数至16MB~32MB，建议8MB
#myisam参数引擎相关
myisam_sort_buffer_size = 128M
myisam_max_sort_file_size = 10G
myisam_repair_threads = 1
myisam_recover                                       #自动检查和修复没有适当关闭的MyISAM表
key_buffer_size = 16M                                #myisam索引buffer，只有key没有data
 
transaction_isolation = READ-COMMITTED              #事务隔离级别
tmp_table_size = 64M                                #设置内存临时表最大值
max_heap_table_size = 64M                           #独立的内存表所允许的最大容量
#innodb引擎参数相关
default-storage-engine=InnoDB                      #默认表的类型为InnoDB
innodb_old_blocks_time =1000                       #减小单次的大批量数据查询,默认为0，调整后性能提升80%                     http://www.cnblogs.com/cenalulu/archive/2012/10/10/2718585.html 
innodb_flush_method = O_DIRECT                     #从innode刷新到磁盘，不经过系统write,fdatasync(默认)，O_DSYNC，O_DIRECT http://blog.csdn.net/jiao_fuyou/article/details/16113403 
innodb_additional_mem_pool_size = 16M              #设置InnoDB存储的数据目录信息和其他内部数据结构的内存池大小
innodb_buffer_pool_size = 51G                      #InnoDB使用一个缓冲池来保存索引和原始数据，官方建议物理内存的80%
innodb_data_file_path = ibdata1:128M:autoextend    #表空间
innodb_file_io_threads = 4                         #InnoDB中的文件I/O线程，通常设置为4，innodb除master线程外，还有insert buffer, log, read, write这4种线程，默认各有一个
innodb_read_io_threads = 8
innodb_write_io_threads = 8
innodb_thread_concurrency = 8                      #服务器有几个CPU就设置为几，建议用默认设置，一般设为8
innodb_flush_log_at_trx_commit = 2                 #设置为0就等于innodb_log_buffer_size队列满后再统一存储，默认为1
innodb_log_buffer_size = 16M                       #默认为1MB，通常设置为6-8MB就足够
innodb_log_file_size = 512M                        #确定日志文件的大小，更大的设置可以提高性能，但也会增加恢复数据库的时间
innodb_log_files_in_group = 3                      #为提高性能，MySQL可以以循环方式将日志文件写到多个文件。推荐设置为3
innodb_max_dirty_pages_pct = 90                    #InnoDB主线程刷新缓存池中的数据
innodb_lock_wait_timeout = 120                     #InnoDB事务被回滚之前可以等待一个锁定的超时秒数
innodb_file_per_table = 1                          #InnoDB为独立表空间模式，每个数据库的每个表都会生成一个数据空间,0关闭,1开启
innodb_autoextend_increment = 256                  #这个参数的作用是控制innodb 共享表空间文件自动扩展的大小
[mysqldump]
quick
max_allowed_packet = 64M
[mysqld_safe]
log-error = /usr/local/mysql/data/mysql.err
pid-file = /usr/local/mysql/data/mysqld.pid
```



```ini
[mysqld]
basedir=D:/Program Files (x86)/MySql # 设置mysql的安装目录
datadir=D:/Program Files (x86)/MySql/data # 设置mysql数据库的数据的存放目录，必须是data，或者是//xxx/data
*************************分割线*******************
port = 3306
socket = /tmp/mysql.sock
default-character-set=utf8 # 设置mysql服务器的字符集
skip-locking
key_buffer = 16K
max_allowed_packet = 1M
table_cache = 4
sort_buffer_size = 64K
read_buffer_size = 256K
read_rnd_buffer_size = 256K
net_buffer_length = 2K
thread_stack = 64K
[client]
#password = your_password
port = 3306
socket = /tmp/mysql.sock
default-character-set=utf8
```

