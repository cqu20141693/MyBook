# [canal release](https://github.com/alibaba/canal/releases)
### canal developer
1. 下载包进行配置
  ```
    canal.properties
    # canal 注册zk地址
    canal.zkServers = 10.233.99.44:2181
    # destinations 配置
    canal.destinations = example
    canal.auto.scan = true
    canal.auto.scan.interval = 5
    canal.instance.global.mode = spring


    instance.properties
    # position info
    # MySQL主库位置
    canal.instance.master.address=10.233.27.248:3306
    # binlog 配置 : 起始文件,迁移量,时间戳,gtid点
    canal.instance.master.journal.name=
    canal.instance.master.position=
    canal.instance.master.timestamp=
    # MySQL 库的账号,密码,字符编码
    canal.instance.dbUsername=root
    canal.instance.dbPassword=root
    # 需要监听的表
    canal.instance.filter.regex=ioternet.device_info
    # table black regex
    canal.instance.filter.black.regex=

  ```
2. 启动程序查看日志

3. 其他消息中间件的使用

### canal adapter 
1. 下载release 进行配置
  ```
    application.properties
    # 单节点deployer 配置;开发测试
    canalServerHost: 127.0.0.1:11111
    # 集群deployer 的功能
    zookeeperHosts: slave1:2181
    #mysql 数据源,用途暂时不详,可能是etl中DataSource
    srcDataSources:
    defaultDS:
        url: jdbc:mysql://10.233.27.248:3306/ioternet?useUnicode=true
        username: root
        password: root
    canalAdapters:
    - instance: example # canal instance Name or mq topic name
    groups:
    - groupId: g1 #对应于es 配置文件中的group
        outerAdapters:
        - name: logger # 日志输出
        - name: es # es adapter
        key: DeviceInfoKey #对应es中的key
        hosts: 172.16.20.64:39200 # 127.0.0.1:9200 for rest mode
        properties:
            mode: rest # or transport 
            # security.auth: test:123456 #  only used for rest mode
            cluster.name: elasticsearch

    es.device_info.yml
    dataSourceKey: defaultDS  # 对应是用的数据源,全量同步时的数据库
    destination: example
    groupId: g1
    outerAdapterKey: DeviceInfoKey
    esMapping:
    _index: device_info_test1 # 索引名称,可以用别名
    _type: _doc
    _id: _id
    upsert: true  # 全量时,为更新,不会覆盖
    #  pk: id
    sql: "select id as _id,id as deviceId,device_key as deviceKey,create_at as createAt,create_protocol as createProtocol,
            device_name as deviceName,product_id as productId,sn,device_type as deviceType,
            active_at as activeAt,device_status as deviceStatus
            from device_info"
    #  objFields:
    #    _labels: array:;
    etlCondition: ""  # 同步条件
    commitBatch: 3000
  ```
