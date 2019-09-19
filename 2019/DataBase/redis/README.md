# redis 介绍 
### redis 基础知识
#### redis 的优点
    1. **特点**
        1. 单个线程处理请求
        2. 提供的每一个命令都是原子性命令
        3. redis 的所有数据都在内存中
        4. 字符串自动扩长 ： 用于统计
    2. **使用注意事项**
        1. 首先是考虑使用哪一种数据结构存储数据
            1. String ： 使用的频率最高，用于缓存部分的数据，
        2. 考虑使用中是不是原子性命令 ： 原子性命令可以避免并发问题
        3. 考虑命令的时间复杂度，防止后面造成不可挽回的局面
        4. 服务器突然宕机,导致数据丢失,数据持久化
    3. redis的结构
        1. 其中所有key都是放在一个很多大的字典中:类似hashmap :一维的数组(2^n,扩容加倍 )+二维的链表

#### redis的使用场景
1. 缓存
2. 分布式锁
3. 计数 
    1. 使用**String或者hash**的incr 命令进行计数 ：原子命令避免高并发中数据累积的不准确性
4. 异步消息队列
    1. 使用**list** rpush/lpop
        1. 放到list中，通过消费者进行消费
        2. 没有响应保证，无法保证数据绝对可靠
    2. Rabbitmq ， kafka
        1. 实时性的消息队列
    3. **队列空**
        1. kafka中，可以直接返回
        2. redis list:
            1. 消费端循环的进行pop 
                1. 拉高客户端的CPU，redis的QPS也会拉高
            2. sleep 
                1. 如果有多个消费者，那么延迟会增大
            3. 堵塞读
                1. blpop/brpop ： 如果没有数据，就会睡眠，一有数据就会醒过来
                2. 注意如果长时间的休眠，会被认定为空闲连接，服务器就会断开连接，上面的命令就会抛出异常，注意捕获异常；
5. 延时队列
    1. 使用zadd(key,score,value);
    2. 循环取 ZRANGEBYSCORE(key,0,maxScore,offset,count)
    3. 取出第一个value，然后进行移除zrem(key,value)
    4. 如果删除成功，就可以处理；否则，就不进行处理
    5. 进一步优化，可以使用lua将zrangbyscore ，zrem 合并为一个原子操作
6. 位图
    1. setbit ,getbit ,get **
    2. bitcount ,bitpos
        1. bitcout : 统计指定位置范围内1的个数
            1. bitcount key [start] [end] O(n)
        2. bitpos : 查找范围中第一个出现的0或者1
            1. bitpos key 0|1 [start] [end] O(n)
7. redis 限流
    1. 使用zset:    
        1. key : 用户; score: time ;value : time
        2. 每次发生行为,将zset中限流时间段以外的数据进行删除,保留时间段内的数据,然后进行统计,看是否出现
            1. zdd(key,score,value): 添加数据
            2. zremrangebyscore(key,0,nowTime-period*1000) : 删除限流时间段外的数据
            3. zcard(key) : 计算当前可以的size 统计是否超出了限制
    2. 漏斗限流
        1. 思想 : 给定一个限流的容量,流量的处理能力vo,流量的流进速度vi,剩余容量m 
            1. 当处理能力vo>vi 时没有限制
            2. 当vi>vo时需要设置一个最小的剩余容量,当到达最小剩余容量时,不准进入,等待一会再检查剩余容量,开启处理能力(使用hash结构存储所有信息,但是需要保证存取的原子性)
8. GeoHash
    1. 地理位置Hash存储
        1. geoadd key 经度 纬度 地点
        2. geodist key place1 place22 km/m
        3. geopos key place 
        4. geohash key place
        5. georadiusbymember key place value1 km/m count value2 asc : 查找距离place  value km/m 中最近的value2 个目的地
            1. place :可以是地点名,也可以是经纬度坐标
9. scan
    1. vs keys
        1. keys 命令放回所有满足的key,如果数据库的key数据量很多,会阻塞redis : O(n)
            1. keys regex
        2. scan 命令可以限制每次返回的size,每次返回一个游标和满足条件的key,知道返回游标0 : O(n)
            1. scan cursor regex limit hint  
                1. hint : **服务器单词遍历的字典槽位数**  : 字典中以为数组的索引位置
                2. 遍历算法: 高位进位加法进行遍历
                3. zscan,hscan,sscan ;
                4. 返回结果中 :游标整数,满足key(可有有重复,需要去重) 
            2. 对大key的排查
                1. 大 key 会导致redis的内存分配很大,同时在迁移和删除时出现卡顿
10. redis IO 模型
    1. redis是单线程程序 : node.js , nginx 同样
        1. 使用指令要注意其时间复杂度,对于复杂度为O(n)需要谨慎
    2. redis 单线程为什么还跑得很快
        1. 数据都是在内存中,所有操作都是内存级的原子性操作
    3. 非阻塞IO
        1. VS 阻塞IO
            1. 每一个socket的读写都会一次性完成
                1. read : 如果给定的缓冲区字节不够,会等待
                2. write : 如果缓冲区给定的字节不够,会等待空闲的缓冲区进行写
        2. 每一个socket的读写会根据缓冲区给定的大小进行读写,之后会通过时间轮询(多路复用 )对socket进行处理;
        
    4. 指令命令
        1. redis 会将每个客户端的socket关联一个指令队列,多有的执行都是排队进行顺序处理
    5. 响应队列
        1. redis给每一个socket关了一个响应队列,通过该队列来将指令返回的结果返回给客户端.当队列为空,就会从轮询API中移除(socket 描述符从write_fds中移除);
    6. 定时任务
        1. **在每一个轮询中,如果有一个到期的定时任务,会最快执行**,然后记录最近需要执行的定时任务时间间隔tvp,确保系统调用的timeout时间.
        2. 将tvp作为IO 时间的timeout;用于执行IO操作
11. redis 通信协议
    1.  redis 序列化协议 : 一种简单,解析性能极好的文本协议
    2. 协议的数据结构
        1. 单行字符串 以 + 符号开头。
        2. 多行字符串 以 $ 符号开头， 后跟字符串长度。
        3. 整数值 以 : 符号开头， 后跟整数的字符串形式。
        4. 错误消息 以 - 符号开头。
        5. 数组 以 * 号开头， 后跟数组的长度
        6. NULL : $-1\r\n
        7. 空串 : $0\r\n\r\n  :两个\r\n之间是空串
    3. 客户端->服务端
        1. 指令格式:多行字符串
            1. set author taoge
            2. *3\r\n$3\r\nset\r\n$6\r\nauthor\r\n$8\r\taoge\r\n
    4. 服务器->客户端
        1. 单行字符串响应 : +OK
        2. 错误响应 : -ERR value is not an integer or out of range
        3. 整数响应 :   :1
        4. 多行字符串响应 : 
            1. $5
            2. taoge 
        5. 数组响应
        6. 组合
12. redis 持久化 
13. redis 管道
    1. redis的请求流程
        1. 流程图
        2. redis的每一个请求都是一个TCP请求,每次请求返回都是阻塞的,需要一次网络开销
        3. pipeline : 可以将多个请求放到管道中,将多次请求的网络开销转化为一次,大大的增加了读取的效率;
14. redis 事务
    1.  事务模型 : begin,exec,rollback
    2. 使用
        1. mutil : 表示事务开始
        2. exec : 表示事务的执行
            1. redis是单线程程序,保证了单个命令原子性
            2. redis事务只保证了隔离性,当事务中的命令失败,其他的执行还会执行
        3. discard : 放弃事务 : 丢弃任何缓存的命令
    3. watch : 乐观锁
        1. 在使用事务之前,使用watch(key):
            1. 服务器端会监控key,如果在监控期间,key被使用了,其他的执行就会抛出异常
    4. 为什么redis的事务不支持回滚
15. redis 集群
    1. 
16. redis 消息队列
    1. PubSub :
        1. 支持多个消费者消费数据
            1. 当客户端发送订阅命令后,返回一个订阅成功的消息 : 会有一定的延迟
            2. 空处理 :当消费数据为空 : 使用listen方式阻塞监听消息;
        2. 只对消费多个主题
        3. **支持pattern subscribe**(匹配订阅)
        4. 消息结构:
            1. pattern : 订阅匹配的模式: 如果使用subsribe,返回null
            2. type :表示消息的类型 
            3. channel : 表示主题
            4. data : 表示数据
        5. 缺点 :
            1. 对于生产者产生的消息,redis会直接找到消费者推送过去,如果没有消费者,数据丢失;比如开始有三个消费者,但是其中一个消费者中途断了,数据就会丢失.
            2. PubSub 的消息是不会持久化,如果redis重启,历史数据丢失;
    2. Stream : redis5.0
        1. **支持多播的可持久化的消息队列**
        2. 命令:  
            1. xadd : 首次追加创建stream
                1. maxlen : 提供了stream定长消息,超过将会删除老消息
            2. xgroup create : 创建群组
            3. xrange 获取消息列表，会自动过滤已经删除的消息
            4. xlen 消息长度
            5. del 删除 Stream
            6. xinfo stream/groups name : 表示查看stream的信息
            7. xreadgroup 
                1. 指定消费组,stream进行组内消费
                2. 可以进行阻塞消费;
            8. xack : 对消费的数据进行相应;
            9. 
        3. 重要属性
            1.  Stream -> key 
            2. 消费群组
            3. last_delivered_id
            4. pending_ids
                1. 记录了当前已经被客户端读取的消息，但是还没有 ack
                2. 用来确保客户端至少消费了消息一次，而不会在网络传输的中途丢失了没处理
                3. 如果防止pel数据丢失
                    1. 当客户端重连的时候,需要先读取所有的pel数据,然后在读取last_delivered_id之后的新消息
        4. 独立消费
            1. xread : 将stream当做list消费
                1. 不定义消费组消费
                2. 当 Stream 没有新消息时，甚至可以阻塞等待
                3. 每次消费的时候需要将上次消费的最后记录参数传递
        5. 分区 partition
            1. redis 没有提供分区,但是可以通过增加stream达到目的

    3. 使用list,或者zset 做异步消息队列或者是延迟队列
17. redis info
    1. Server 服务器运行的环境参数
        1. 
        2. 
    2. Clients 客户端相关信息
        1.  redis-cli info clients : 正在连接的客户端数量
        2.  client list :列出所有的客户端链接地址来确定源头
    3. Memory 服务器运行内存统计数据
        1. redis-cli info memory | grep used | grep human : 查看内存使用情况
    4. Persistence 持久化信息
    5. Stats 通用统计数据
        1. redis-cli info stats |grep ops :  每秒操作数
        2.  redis-cli monitor : monitor 指令快速观察一下究竟是哪些 key 访问比较频繁
        3. redis-cli info stats |grep reject : 超出最大连接数限制而被拒绝的客户端连接次数
        4. redis-cli info stats | grep sync : 查看 sync_partial_err 变量的次数,它表示主从半同步复制失败的次数
    6. Replication 主从复制相关信息
        1. redis-cli info replication |grep backlog : 通过复制积压缓冲区大小判断主从复制的效率
    7. CPU CPU 使用情况
    8. Cluster 集群信息
    9. KeySpace 键值对统计数量信息
18. redis 分布式锁
    1. 如何避免主从切换中分布式锁被多个客户端获取
19. redis 过期策略
    1. 
20. redis使用
    1. 正确使用Jedis
    2. redis 数据安全
21. redis 源码数据结构
    1. 字符串
        1. 
    2. 字典
        1. Hashtable  
        2. hash 函数 : 快速定位数据的二维链表
        3. 扩容 : 渐进式扩容,同时条件为元素个数为一维数组的长度,扩容2倍,当key的大小大于1M后,最多扩容1M
        4. **hash 和set** 都是字典存储;
    3. 压缩列表
        1. 支持双向遍历 : ztail_offset 
        2. 压缩列表是一块连续的内存空间，元素之间紧挨着存储，没有任何冗余空隙 : 就不会存在指针占用空间
        3. IntSet 是紧凑的数组结构 : et 集合容纳的元素都是整数并且元素个数较小
    4. 快速列表
        1.  **list** 列表数据结构使用的是压缩列表 ziplist 和普通的双向链表linkedlist
            1. 元素少时用 ziplist 
            2. 元素多时用 linkedlist : 存在两个双向指针,很消耗内存
        2. quicklist 是 ziplist 和 linkedlist 的混合体
            1. 将 linkedlist 按段切分，每一段使用 ziplist 来紧凑存储，多个 ziplist 之间使用双向指针串接起来 : 从而减少指针数量
            2. 默认单个 ziplist 长度为 8k
    5. 跳跃列表
        1. zset 是一个复合结构,需要一个 hash 结构来存储 value 和 score,按照 score 来排序的功能
        2. 减少查找的时间复杂度log(n)
    6. 紧凑列表
        1. listpack 跟 ziplist 的结构几乎一摸一样，只是少了一个 zltail_offset 字段 :目前只用在stream中
        2. 
    7. 基础树
        1. Rax是一个有序字典树,按照 key 的字典序排列，支持快速地定位、插入和删除操作
        2. 
#### redis基本数据
1. String ： set key value
    1. set ，get ，setnx,getset **redis 锁**
    2. incr : 整形自增 **计数**

2. list : rpush key value ... **相当于java的LinkedList** 
    1. rpush 
        1. rpop : 栈，先进后出  
        2. lpop ： 队里，先进先出  **异步队列**
    2. lpush
    3. 慢查询 O(n)
        1. lindex key 0 : get(0)  
        2. ltrim key 0 -1: 表示保存所有（0 - n-1） 
        3. lrange key 0 -1 : 表示取所有的数据 
    4. 常用用异步队列使用 
        1. 将任务结构体序列化为字符串塞进redis的列表，其他线程读取列表价进行处理

3. hash : hset key field value : **存储结构类似HashMap**
    1. vs String    
        1. String 将整个结构序列化为String 存储，获取时也是整体
        2. hash 则是将每一个field 单独存储，获取单个field 方便，
        3. hash 比String更多存储消耗
    2. hset,hmset
    3. hget,hgetall key
    4. hlen，hincrby **计数**

4. Set ：sadd key value  **类似java HashSet**
    1. sadd **去重**
    2. smembers，sismember,scard **包含**

5. zset : zadd key [score value] ... 一个
    1. zadd 
    2. zrange(按score升序i m),zrevrange(score逆序) key i m **获取某个范围的values
    3. zcard,zscore,zrangebyscore **获取score**
    4. zrem key value **删除value**

6. redis pipeline
    1. 管道对于连续处理同一个key的存取速率有显著提升
    2. 
7. 

#### redis 持久化存储
1. RDB : 是一个非常紧凑（compact）的文件，它保存了 Redis 在某个时间点上的数据集(时间点快照)
    1. 优点
        1. 备份中最大化redis的性能
        2. 备份文件体积比AOF小,恢复速度快
        3. 可以对备份文件进行加密后传输到其他的数据中心
        4. 内存数据的二进制序列化形式
    2. 缺点
        1. 数据存在丢失(比如一个小时一次快照备份)
        2. 每次备份都需要fork一个子进程,如果数据集庞大,非常耗时,可能会影响服务器对客户端的处理 
2. AOF : 记录服务器执行的所有写操作命令，并在服务器启动时，通过重新执行这些命令来还原数据集
    1. 优点
        1. AOF文件有序的保存了对数据库的所有修改内存的操作,连续的增量备份
        2. AOF 可以设置不同的fsync策略,默认一秒一次;数据之丢失一秒,同时对redis服务性能很好保证;
    2. 缺点
        1. 文件体积较大
        2. 写入恢复速率较慢,同时服务器的性能略比RDB差
        3. 存在bug : 比如阻塞命令
    3. AOF 文件重写
3. redis是如何做到单线处理持久化
    1. **快照**
        1. Redis 使用操作系统的多进程 COW(Copy On Write) 机制来实现快照持久化
            1. 调用 glibc 的函数 fork 产生一个子进程 
                1.  父进程继续处理客户端请求
                2. 父子进程共享内存中的代码和数据段  
            2. 使用操作系统的 COW 机制来进行数据段页面的分离 :数据段由很多的系统页面组成
                1. 子进程做数据持久化，它不会修改现有的内存数据结构
                2. 父进程对其中一个页面的数据进行修改时，会将被共享的页面复
    制一份分离出来，然后对这个复制的页面进行修改
    2. AOF原理
        1. redis 接收到客户端的修改指令后,先进行校验,然后立即将文本存储到AOF中,再放到指令队列中执行.
            1. fsync
                1. redis AOF 日志文件进行写操作时，实际上是将内容写到了内核为文件描述符分配的一个内存缓存中，然后内核会异步将脏数据刷回到磁盘的
                2. redis 使用fsync(int fd) 强制将内核缓存写到磁盘 
                    1. redis 默认: 1s 
                    2. 用不fsync : 由操作系统决定 : 极不安全
                    3. 一次指令一次fsnc : 影响redis性能,生产基本不会使用
4. 运维
    1. 快照是通过开启子进程的方式进行的，它是一个比较耗资源的操作。
    2. 遍历整个内存，大块写磁盘会加重系统负载
    3. AOF 的 fsync 是一个耗时的 IO 操作，它会降低 Redis 性能，同时也会增加系统 IO 负担
    4. **Redis 的主节点是不会进行持久化操作，持久化操作主要在从节点进行。从节点是备份节点，没有来自客户端请求的压力，它的操作系统资源往往比较充沛**
5. redis 混合持久化
    1. rdb 持久化回复会丢失很多数据,AOF 持久化在恢复的时候会花费很多时间,
    2. 当我们在执行一次rdb 持久化后,之后的时间使用AOF 持久化  : **备份文件中开始部分为rdb,之后部分为AOF**
        1. 恢复的时候就需要先进行rdb 恢复,接着再AOF恢复
        2. 缺点 : rdb 部分是压缩后的数据,可读性差 

#### redis 设计与实现
1. 数据结构与对象性
    1. 
