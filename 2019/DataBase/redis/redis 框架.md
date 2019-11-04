# redis 框架 使用
### spring
### 按使用方式
1. StringRedisTemplate
    1.1 String
    1.2 Set
    1.3 Hash
    1.4 list
        1.4.1 队列/堆栈 : rpush/rpop/lpush/lpop
        1.4.2 左进右出是队列;左进左出是栈
    1.5 sortedset
        1.5.1 key members score : 某个key 下的每一个成员按照score的排序
        1.5.2 用户排行榜,排序的使用
    1.6 pub/sub
    1.7 transaction
    1.8 HyperLogLog
    1.9 BitSet
    2.0 pipleline
    2.1 lua script
### redis 使用优化
1. 键值设计
    1. 键设计 ： 模块名 +  标识符 + 数据 + 数据名称 ： thingmodel:productKey:wqertyuc:identifier:testInt
    2. 最好维护一个表单 ： 维护每个key的名称对应的前缀（可以约定简称）；
2. value 设计
    1. 【强制】：拒绝bigkey(防止网卡流量、慢查询)
    ```
        string类型控制在10KB以内，hash、list、set、zset元素个数不要超过5000。

```
# Server
redis_version:5.0.3
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:179735ca33dc5000
redis_mode:standalone
os:Linux 4.4.189-1.el7.elrepo.x86_64 x86_64
arch_bits:64
multiplexing_api:epoll
atomicvar_api:atomic-builtin
gcc_version:8.2.0
process_id:1
run_id:2c42a5614abc5da7a13616c3b84d75b3cfb9d810
tcp_port:6379
uptime_in_seconds:6175925
uptime_in_days:71
hz:10
configured_hz:10
lru_clock:12207818
executable:/data/redis-server
config_file:/data/conf/redis.conf

# Clients
connected_clients:13
client_recent_max_input_buffer:2
client_recent_max_output_buffer:0
blocked_clients:0

# Memory
used_memory:7121264
used_memory_human:6.79M
used_memory_rss:16695296
used_memory_rss_human:15.92M
used_memory_peak:11335880
used_memory_peak_human:10.81M
used_memory_peak_perc:62.82%
used_memory_overhead:1319782
used_memory_startup:790896
used_memory_dataset:5801482
used_memory_dataset_perc:91.65%
allocator_allocated:7170960
allocator_active:7880704
allocator_resident:10604544
total_system_memory:33737793536
total_system_memory_human:31.42G
used_memory_lua:37888
used_memory_lua_human:37.00K
used_memory_scripts:0
used_memory_scripts_human:0B
number_of_cached_scripts:0
maxmemory:0
maxmemory_human:0B
maxmemory_policy:volatile-lru
allocator_frag_ratio:1.10
allocator_frag_bytes:709744
allocator_rss_ratio:1.35
allocator_rss_bytes:2723840
rss_overhead_ratio:1.57
rss_overhead_bytes:6090752
mem_fragmentation_ratio:2.36
mem_fragmentation_bytes:9616032
mem_not_counted_for_evict:0
mem_replication_backlog:0
mem_clients_slaves:0
mem_clients_normal:252758
mem_aof_buffer:0
mem_allocator:jemalloc-5.1.0
active_defrag_running:0
lazyfree_pending_objects:0

# Persistence
loading:0
rdb_changes_since_last_save:5
rdb_bgsave_in_progress:0
rdb_last_save_time:1572488766
rdb_last_bgsave_status:ok
rdb_last_bgsave_time_sec:0
rdb_current_bgsave_time_sec:-1
rdb_last_cow_size:806912
aof_enabled:0
aof_rewrite_in_progress:0
aof_rewrite_scheduled:0
aof_last_rewrite_time_sec:-1
aof_current_rewrite_time_sec:-1
aof_last_bgrewrite_status:ok
aof_last_write_status:ok
aof_last_cow_size:0

# Stats
total_connections_received:2678470
total_commands_processed:184378570
instantaneous_ops_per_sec:0
total_net_input_bytes:12515033150
total_net_output_bytes:20636364747
instantaneous_input_kbps:0.01
instantaneous_output_kbps:0.00
rejected_connections:0
sync_full:0
sync_partial_ok:0
sync_partial_err:0
expired_keys:14679
expired_stale_perc:0.00
expired_time_cap_reached_count:0
evicted_keys:0
keyspace_hits:142166478
keyspace_misses:97472
pubsub_channels:1
pubsub_patterns:0
latest_fork_usec:27991
migrate_cached_sockets:0
slave_expires_tracked_keys:0
active_defrag_hits:0
active_defrag_misses:0
active_defrag_key_hits:0
active_defrag_key_misses:0

# Replication
role:master
connected_slaves:0
master_replid:ade14402d48179064c9ae14ccd9a557d75fa3903
master_replid2:0000000000000000000000000000000000000000
master_repl_offset:0
second_repl_offset:-1
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0

# CPU
used_cpu_sys:6623.959097
used_cpu_user:7040.057567
used_cpu_sys_children:59.165280
used_cpu_user_children:230.279083

# Cluster
cluster_enabled:0

# Keyspace
db0:keys=3949,expires=1021,avg_ttl=1184741395548
db2:keys=220,expires=0,avg_ttl=0
db3:keys=16,expires=0,avg_ttl=0
```