# [redis](https://blog.fudenglong.site/2017/06/23/redis%E8%BF%90%E7%BB%B4%EF%BC%8C%E5%89%AF%E6%9C%AC%EF%BC%8C%E9%AB%98%E5%8F%AF%E7%94%A8/)
### Redis 主从
1. 下载redis安装包
2. 配置Redis服务器配置信息(参考上诉bolg)
 ```
    ####-----------------------------------------redis 通用设置-------------------------------------------

    daemonize yes

    pidfile /usr/lcoal/redis/run/redis.pid

    port 6379

    tcp-backlog 511

    bind 127.0.0.1 108.128.187.30

    # 关闭一个闲置N秒的客户端连接，0：禁用
    timeout 120

    tcp-keepalive 0

    loglevel notice

    logfile "/usr/lcoal/redis/logs/redis.log"

    databases 16

    ####-----------------------------------------redis 快照配置-------------------------------------------

    # 设置保存策略

    save 900 1
    save 300 10
    save 60 10000

    stop-writes-on-bgsave-error yes

    rdbcompression yes

    rdbchecksum yes

    dbfilename dump.rdb

    dir /usr/lcoal/redis/data/


    ####-----------------------------------------redis 复制集配置-------------------------------------------

    # slaveof 120.25.231.12 6379    # master这里需要注释,slave需要放开,并指定master地址

    masterauth f628c98b21dbeaa4314da2b710970ce6

    slave-serve-stale-data yes

    slave-read-only yes

    repl-diskless-sync yes

    repl-diskless-sync-delay 0

    repl-ping-slave-period 5

    repl-timeout 60

    repl-disable-tcp-nodelay no

    repl-backlog-size 5mb

    repl-backlog-ttl 3600

    slave-priority 100

    min-slaves-to-write 0

    min-slaves-max-lag 10


    ####-----------------------------------------redis 安全配置-------------------------------------------


    requirepass f628c98b21dbeaa4314da2b710970ce6

    rename-command FLUSHALL CMDOVERRITEFLUSHALL
    rename-command FLUSHDB  ""              
    rename-command KEYS CMDOVERRITEKEYS                 
    rename-command CONFIG CMDOVERRITECONFIG          

    ####-----------------------------------------redis 限制策略-------------------------------------------

    maxclients 100000

    maxmemory 4294967296

    maxmemory-policy allkeys-lru

    maxmemory-samples 10


    ####-----------------------------------------redis AOF配置-------------------------------------------

    appendonly yes

    appendfilename "appendonly.aof"

    appendfsync everysec

    no-appendfsync-on-rewrite no

    auto-aof-rewrite-percentage 100

    auto-aof-rewrite-min-size 64mb

    aof-load-truncated yes

    ####-----------------------------------------redis lua配置------------------------------------------

    lua-time-limit 5000
 ```
3. 
### Redis sentinel
1. 至少需要三个sentinel客户端，假设为 server1，server2，server3; ip分别为，ip1，ip2，ip3
2. 配置
 ```
    bind ip1
    port 26379
    dir /tmp

    sentinel monitor qmgrediscluster 118.148.197.31 6379 2
    sentinel down-after-milliseconds qmgrediscluster 60000
    sentinel failover-timeout qmgrediscluster 180000
    sentinel parallel-syncs qmgrediscluster 1
    sentinel auth-pass qmgrediscluster f628c9adsa21dbeaa4314d12asda
 ```
3. 启动三个这样的服务
    3.1 nohup sudo -u redis /usr/local/redis/bin/redis-sentinel /usr/local/redis/conf/sentinel.conf >> /dev/null 2>&1 &
4. 登录sentinel，获取redis master ip地址：
    4.1 redis-cli -h sentinel-ip -p sentinel-port
    4.2 SENTINEL get-master-addr-by-name qmgrediscluster


### Redis cluster
1. 安装配置
 ```
 配置
    daemonize yes
    #该集群阶段的端口
    port 7000
    #为每一个集群节点指定一个pid_file
    pidfile /var/run/redis_7000.pid
    #在bind指令后添加本机的ip
    bind 127.0.0.1 149.28.37.147
    #找到Cluster配置的代码段，使得Redis支持集群
    cluster-enabled yes
    #每一个集群节点都有一个配置文件，这个文件是不能手动编辑的。确保每一个集群节点的配置文件不通
    cluster-config-file nodes-7000.conf
    #集群节点的超时时间，单位：ms，超时后集群会认为该节点失败
    cluster-node-timeout 5000
    #最后将appendonly改成yes
    appendonly yes

    文件中的 cluster-enabled 选项用于开实例的集群模式， 而 cluster-conf-file 选项则设定了保存节点配置文件的路径， 默认值为nodes.conf 。其他参数相信童鞋们都知道。节点配置文件无须人为修改， 它由 Redis 集群在启动时创建， 并在有需要时自动进行更新。

  启动: 
  redis-server redis.conf
 ```
 
 2. 创建集群
    2.1 通过使用redis-trib.rb 创建集群
     ```
        1. 首先安装好redis-trib.rb
        2. redis-trib create --replicas 1 127.0.0.1:7000 127.0.0.1:7001 127.0.0.1:7002 127.0.0.1:7003 127.0.0.1:7004 127.0.0.1:7005
        选项 --replicas 1 表示我们希望为集群中的每个主节点创建一个从节点。
     ```

### Redis命令
1. 连接Redis sever : .\redis-cli.exe  -h dev-redis-4cache-redis-ha-master-svc.devdb.svc.cluster.local -p 6379
2. set
    2.1 sadd
    2.2 smembers
    2.3 spop
    2.4 sdelete