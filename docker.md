# Docker 基础命令

1. docker stop 容器Id 暂停容器
2. docker start 容器Id 启动容器
3. docker restart 容器Id 重启容器


# Docker 安装 mysql

**拉取最新版MySQL镜像（如果要安装指定版本将latest换成版本号即可）**

```
docker pull mysql:latest
```

 -p 3306:3306 ：映射容器服务的 3306 端口到宿主机的 3306 端口，外部主机可以直接通过 宿主机ip:3306 访问到 MySQL 的服务。
• MYSQL_ROOT_PASSWORD=123456：设置 MySQL 服务 root 用户的密码。
• --name 容器别名

```
docker run -itd --name mysql-test -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql–restart=always 跟随docker启动
–privileged=true 容器root用户享有主机root用户权限
-v 映射主机路径到容器
-e MYSQL_ROOT_PASSWORD=root 设置root用户密码
-d 后台启动
–lower_case_table_names=1 设置表名参数名等忽略大小写
```

关键之处就在这了 忽略大小写的启动方式，只能在初始化的时候改，或者安装指定版本
–lower_case_table_names=1

```
docker run -d --name mysql -v /d/mysql/data:/var/lib/mysql -v /d/mysql/log:/var/log/mysql -v /d/mysql/conf/my.cnf:/etc/mysql/my.cnf --restart=always -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql

ip 192.168.111.134 账号 root 密码 HwMDui5ZGBMIGtN2
数字运维直播回放
webrtc://dt-video.jie-xin.com:1988/live/GDT02a166_0_0_
http://10.0.8.229:8700/live/GDT02a166_0_0_.flv
```

3.创建一个账号并设置密码
create user 'jebf'@'%' identified with mysql_native_password by 'sLjjeZLYvSkteRVC';
4.设置 ‘root’@‘%’ 的密码永不过期,密码为sLjjeZLYvSkteRVC
ALTER USER 'jebf'@'%' IDENTIFIED BY 'sLjjeZLYvSkteRVC' PASSWORD EXPIRE NEVER;
5.这里为刚才创建的yaku@% 用户授予所有数据库的所有表的所有操作访问权限
grant all privileges on *.* to 'jebf'@'%' with grant option;

6.刷新权限

flush privileges;

# Docker 安装 redis

**创建Redis需要挂载的目录**

mkdir -p /data/home/docker/redis/{conf,data}

**在conf文件夹 创建配置文件 vi redis.conf 内容如下**

```
# ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~redis 配置~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
# 指定Redis 只接收来自于该IP 地址的请求，如果不进行设置，那么将处理所有请求，在生产环境中为了安全最好设置该项。默认注释掉，不开启
#bind 127.0.0.1
 
# #默认yes，开启保护模式，限制为本地访问
protected-mode no
# 监听端口，默认为6379
port 6379
 
# 链接密码
requirepass 123456
 
tcp-backlog 511
 
# 设置客户端连接时的超时时间，单位为秒。当客户端在这段时间内没有发出任何指令，那么关闭该连接
timeout 0
 
# 指定TCP连接是否为长连接,"侦探"信号有server端维护。默认为0.表示禁用
tcp-keepalive 300
 
# 默认情况下，redis 不是在后台运行的，如果需要在后台运行，把该项的值更改为yes。
daemonize no
 
supervised no
 
# 当Redis 在后台运行的时候，Redis 默认会把pid 文件放在/var/run/redis.pid，你可以配置到其他地址。当运行多个redis 服务时，需要指定不同的pid 文件和端口
pidfile /var/run/redis_6379.pid
 
# log 等级分为4 级，debug,verbose, notice, 和warning。生产环境下一般开启notice
loglevel notice
 
# 配置log 文件地址，默认使用标准输出，即打印在命令行终端的窗口上，修改为日志文件目录
logfile ""
 
# 设置数据库的个数，可以使用SELECT 命令来切换数据库。默认使用的数据库是0号库。默认16个库
databases 16
 
always-show-logo yes
 
# 保存数据快照的频率，即将数据持久化到dump.rdb文件中的频度。用来描述"在多少秒期间至少多少个变更操作"触发snapshot数据保存动作
#默认设置，意思是：
# if(在60 秒之内有10000 个keys 发生变化时){
# 进行镜像备份
# }else if(在300 秒之内有10 个keys 发生了变化){
# 进行镜像备份
# }else if(在900 秒之内有1 个keys 发生了变化){
# 进行镜像备份
# }
save 900 1
save 300 10
save 60 10000
 
# 当持久化出现错误时，是否依然继续进行工作，是否终止所有的客户端write请求。默认设置"yes"表示终止，一旦snapshot数据保存故障，那么此server为只读服务。如果为"no"，那么此次snapshot将失败，但下一次snapshot不会受到影响，不过如果出现故障,数据只能恢复到"最近一个成功点"、
stop-writes-on-bgsave-error yes
 
# 在进行数据镜像备份时，是否启用rdb文件压缩手段，默认为yes。压缩可能需要额外的cpu开支，不过这能够有效的减小rdb文件的大，有利于存储/备份/传输/数据恢复 读取和写入时候，会损失10%性能
rdbcompression yes
 
# 是否进行校验和，是否对rdb文件使用CRC64校验和,默认为"yes"，那么每个rdb文件内容的末尾都会追加CRC校验和，利于第三方校验工具检测文件完整性
rdbchecksum yes
 
# 镜像备份文件的文件名，默认为 dump.rdb
dbfilename dump.rdb
 
rdb-del-sync-files no
 
# 数据库镜像备份的文件rdb/AOF文件放置的路径。这里的路径跟文件名要分开配置是因为Redis 在进行备份时，先会将当前数据库的状态写入到一个临时文件中，等备份完成时，再把该临时文件替换为上面所指定的文件，而这里的临时文件和上面所配置的备份文件都会放在这个指定的路径当中
dir ./
 
replica-serve-stale-data yes
 
replica-read-only yes
 
repl-diskless-sync no
 
repl-diskless-sync-delay 5
 
repl-diskless-load disabled
 
repl-disable-tcp-nodelay no
 
replica-priority 100
 
acllog-max-len 128
 
lazyfree-lazy-eviction no
lazyfree-lazy-expire no
lazyfree-lazy-server-del no
replica-lazy-flush no
 
lazyfree-lazy-user-del no
 
# 默认情况下，redis 会在后台异步的把数据库镜像备份到磁盘，但是该备份是非常耗时的，而且备份也不能很频繁。所以redis 提供了另外一种更加高效的数据库备份及灾难恢复方式。开启append only 模式之后，redis 会把所接收到的每一次写操作请求都追加到appendonly.aof 文件中，当redis 重新启动时，会从该文件恢复出之前的状态。但是这样会造成appendonly.aof 文件过大，所以redis 还支持了BGREWRITEAOF 指令，对appendonly.aof 进行重新整理。如果不经常进行数据迁移操作，推荐生产环境下的做法为关闭镜像，开启appendonly.aof，同时可以选择在访问较少的时间每天对appendonly.aof 进行重写一次。
appendonly no
 
 
appendfilename "appendonly.aof"
 
# 设置对appendonly.aof 文件进行同步的频率。always 表示每次有写操作都进行同步，everysec 表示对写操作进行累积，每秒同步一次。no不主动fsync，由OS自己来完成。这个需要根据实际业务场景进行配置
appendfsync everysec
 
no-appendfsync-on-rewrite no
 
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb
 
aof-load-truncated yes
 
aof-use-rdb-preamble yes
 
lua-time-limit 5000
 
slowlog-log-slower-than 10000
 
slowlog-max-len 128
 
latency-monitor-threshold 0
 
notify-keyspace-events ""
 
hash-max-ziplist-entries 512
hash-max-ziplist-value 64
 
list-max-ziplist-size -2
 
list-compress-depth 0
 
set-max-intset-entries 512
 
zset-max-ziplist-entries 128
zset-max-ziplist-value 64
 
hll-sparse-max-bytes 3000
 
stream-node-max-bytes 4096
stream-node-max-entries 100
 
# 是否开启顶层数据结构的rehash功能,如果内存允许,请开启。rehash能够很大程度上提高K-V存取的效率
activerehashing yes
 
# 客户端buffer控制。在客户端与server进行的交互中,每个连接都会与一个buffer关联,此buffer用来队列化等待被client接受的响应信息。如果client不能及时的消费响应信息,那么buffer将会被不断积压而给server带来内存压力.如果buffer中积压的数据达到阀值,将会导致连接被关闭,buffer被移除。
 
#buffer控制类型包括:normal -> 普通连接；slave ->与slave之间的连接；pubsub ->pub/sub类型连接，此类型的连接，往往会产生此种问题;因为pub端会密集的发布消息,但是sub端可能消费不足.
#指令格式:client-output-buffer-limit <class> <hard> <soft> <seconds>",其中hard表示buffer最大值,一旦达到阀值将立即关闭连接;
#soft表示"容忍值",它和seconds配合,如果buffer值超过soft且持续时间达到了seconds,也将立即关闭连接,如果超过了soft但是在seconds之后，buffer数据小于了soft,连接将会被保留.
#其中hard和soft都设置为0,则表示禁用buffer控制.通常hard值大于soft.
client-output-buffer-limit normal 0 0 0
client-output-buffer-limit replica 256mb 64mb 60
client-output-buffer-limit pubsub 32mb 8mb 60
 
# Redis server执行后台任务的频率,默认为10,此值越大表示redis对"间歇性task"的执行次数越频繁(次数/秒)。"间歇性task"包括"过期集合"检测、关闭"空闲超时"的连接等,此值必须大于0且小于500。此值过小就意味着更多的cpu周期消耗,后台task被轮询的次数更频繁。此值过大意味着"内存敏感"性较差。建议采用默认值。
hz 10
 
dynamic-hz yes
 
aof-rewrite-incremental-fsync yes
 
rdb-save-incremental-fsync yes
 
jemalloc-bg-thread yes
```

**设置挂载重新运行redis**

docker run \
-p 6379:6379 \
-v /data/home/docker/redis/data:/data \
-v /data/home/docker/redis/conf/redis.conf:/etc/redis/redis.conf \
--privileged=true \
--name redis \
--restart=always \
-d redis:6.2.5 redis-server /etc/redis/redis.conf



docker run -d --name myredis -p 6379:6379 redis --requirepass "HwMDui5ZGBMIGtN2"

# Docker 安装 nexus3 包管理器

拉取最新的镜像

```
docker pull sonatype/nexus3:latest
```

```
docker run -d -p 8081:8081 --name nexus -v /data/nexus:/nexus-data sonatype/nexus3:latest
```

查看默认密码

cat admin.password
