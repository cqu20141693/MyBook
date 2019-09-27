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
### 
