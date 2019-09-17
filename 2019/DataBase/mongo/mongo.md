# mongo
### mongo 特征
1. wiredTiger
    - 默认支持对collection and index 的压缩存储
2. 内存处理

3. 集群 
    - sharding
    - relpSet

4. mongodb 连接数
    - mongo连接数限制由mongo maxConnections和操作系统中单个进程可操作文档数共同决定
    - **如何避免多个连接数的问题**
        - 只用同一个Client创建的mongoTemplate，只会建立一个连接数

5. [mongodb 内存用在了哪里](http://mongoing.com/archives/8781)
    - 为什么我的 MongoDB 使用了 XX GB 内存？
        - 主要是存储引擎会耗费大量的内存
        - 对客户端连接请求处理
            - MongoDB Driver 会跟 mongod 进程建立 tcp 连接，tcp 协议栈除了为连接维护socket元数据为，每个连接会有一个read buffer及write buffer，
            - mongo针对每一个连接，会单独起一个线程负责处理；
            - mongodb 一些后台进程 ：主备同步、定期刷新 Journal、TTL、evict 等线程
        - 
    - 一个机器上部署多个 Mongod 实例/进程，WiredTiger cache 应该如何配置？
        - 配置cacheSizeGB : 大约为服务器内存的60%
        - 如果是单个mongod,默认配置即可
    - MongoDB 是否应该使用 SWAP 空间来降低内存压力？
        - 当内存的压力加大时： 如果使用swap : 来缓解内存压力，此时整个数据库服务会变慢，但具体变慢到什么程度是不可控的。
        - 如果不适用swap,当出现内存不够用的时候会kill部分线程。**推荐通过扩展内存资源，或者数据库优化来进行处理**
    - 使用的内存分配器
        - google [tcmalloc](https://gperftools.github.io/gperftools/tcmalloc.html) 
        
6. mongodb 自身特点
    - mongo 是一个极占用内存的数据库服务;其实现原理中会将需要用到的数据通过操作系统映射到内存中;依次来提高查询和写入效率
    - mongo Client 在创建Template的时候会公用一个连接;即一个库一个连接;这给我们的路由方案提供了支持
    - mongo 索引在创建后,不会随数据的删除而删除;只有当集合被删除后,才会对对应的索引进行删除;

### mongo 身份安全
  ```
    KeyWords: mongodb security,Authentication,  Authorization ，用户角色，权限
        1. <security> :一但开启了安全认证，所有的访问都会需要进行身份认证；
            1. 启动配置中：  auth=true #表示开启了认证
        2. <Authority> : 对用户进行授权 ：[角色管理](http://www.mongoing.com/docs/reference/method.html#role-management-methods)授权API：
            详细细节见附件a;
        3. <Authentication>：通过某个用户的用户名和密码进行登录；认证通过后的权限为该user的所有权限；
            1. db.auth();
            ```
            db.auth("Administrator", "abc123" )  # 具有超级用户权限
            ```
        4. <用户角色>： [mongoDB Built-in-roles](https://docs.mongodb.com/v3.6/core/security-built-in-roles/)
            1. 超级账户：root，userAdminAnyDatabase
                1.db.auth()
            2. 管理员账户角色： userAdmin，dbOwner,dbAdmin
            3. 一般账户角色： read,readWrite(all non-system collections and the system.js collection)
            4. 所有权限用户角色：readAnyDatabase，readWriteAnyDatabase，userAdminAnyDatabase  
                1. read :只能读对应的库
                2. readWrite: 可以对库进行读写。可以删除和创建集合，不能建新库； readWriteAnyDatabase，可以建新库，不能授权；
                3. userAdmin: 只能是管理用户，不能对数据库进行操作；	可以给自己赋予读写数据的操作，也可以赋予其他用户对应的权限；
                
        
        最后：连接mongodb的命令： mongo --port 27017 -u "myUserAdmin" -p "abc123" --authenticationDatabase "admin"
    附件： 
    a.
    ```
    1. 创建超级管理员或者管理员；db.createUser({})
                ```
                use admin
                db.createUser(
                    {
                    user: "Administrator",      <用户名称>
                    pwd: "abc123",         <用户密码>
                    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ], <角色，database>
                    mechanisms : ["SCRAM-SHA-1"] <加密方法>
                    }
                )
            ```
    2. 管理员可以添加其他用户角色，或者修改其他用户角色权限
        1. 首先进行认证 (Authentication)：db.auth()
            1. db.auth("Administrator","abc1223")
        2. 添加其他的用户角色
            1. db.createUser({})
                ```
                    给tester用户授予test库readWrite权限，reporting库read权限，ysten 库userAdmin权限
                    use admin;
                    db.createUser(
                        {
                        user: "myTester",  <user>
                        pwd: "xyz123",
                        roles: [ { role: "readWrite", db: "test" }, <role1>
                                    { role: "read", db: "reporting" },  <role2>
                                    { role: "userAdmin", db: "ysten" } ], <role3>
                                    mechanisms : ["SCRAM-SHA-1"] 
                        }
                    )
                
                ```
        3. 给用户授予特权(privileges)
            1. 特权：
            ```     A <privilege> consists of a specified resource and the actions permitted on the resource.
                A <resource> is a database, collection, set of collections, or the cluster.    ```
    ```
    b. 

    A. admin： root ->具有最高的权限。
    B. 在哪个库创建的用户角色，就必须在哪个库进行身份认证；
    C: 管理员账户角色只能认证admin库；所有权角色需要用户为admin库；
    D:注意用户是通过用户名和创建时的库所共同决定的；所以在使用权限的时候也需要在对应的库中去；

    c. 分析理解
    { "_id" : "testDB.test", "user" : "test", "db" : "testDB", "credentials" : { ....... }, "roles" : [ { "role" : "readWrite", "db" : "testDB" } ] }
    _id：表示用户test属于testDB库，校验需要到testDB库;
    user: 表示用户的名字；
    db:表示创建的库；
    credentials:表示身份证书；
    roles：表示该用户的所有角色； <某个/any 库的摸个权限>
    role:表示角色名称
    db：表示对应的库
 
  ```
### mongo 4.0 新特性
1. [transactions](https://docs.mongodb.com/manual/core/transactions/)
    1. 


