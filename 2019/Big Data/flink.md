### flink 
1. [flink 介绍](https://mp.weixin.qq.com/s/M-XK0bEHdbVLtwtsRG_e8Q)
    1. 数据集类型
        1. 无穷数据集：无穷的持续集成的数据集合
            1. 实时交互的数据,日志,交易
        2. 有界数据集：有限不会改变的数据集合
            1. 
    2. 数据运算模型
        1. 流式：只要数据一直在产生，计算就持续地进行
        2. 批处理：在预先定义的时间内运行计算，当完成时释放计算机资源
    3. 事件驱动应用
        1. 监控事件的服务
    4. flink 基石
        1. CheckPoint : 基于CahndyLamport算法,实现了分布式一致性算法,提供了一致性的语义   
            1. 检查点定期触发，产生快照，快照中记录了
                1. 当前检查点开始时数据源（例如Kafka）中消息的offset，
                2. 记录了所有有状态的operator当前的状态信息（例如sum中的数值）。
        2. State : 丰富的State API: ValueState,ListState,MapState,BroadcateState
            1. 两种基本类型的State，即Keyed State和Operator State
        3. Time : 实现了Watermark机制,乱序数据处理,迟到数据容忍
        4. Window : 开箱即用的滚动,滑动,会话窗口;以及灵活的自定义窗口
    5. flink APIs
        1. SQL/Table API(动态tables) : 分析api
            1. 其中包括强大的连接器
            2. 强大的执行引擎
        2. DataStream API(stream,windows) : 流和批数据处理
            1. 强大的连接器
            2. 强大的translation
            3. 强大的
        3. ProcessFunction(events,state,time) : 
    6. flink 结构
        1. 部署：Flink 支持本地运行、能在独立集群或者在被 YARN 或 Mesos 管理的集群上运行， 也能部署在云上。
        2. 运行：Flink 的核心是分布式流式数据引擎，意味着数据以一次一个事件的形式被处理。
        3. API：DataStream、DataSet、Table、SQL API。
        4. 扩展库：Flink 还包括用于复杂事件处理，机器学习，图形处理和 Apache Storm 兼容性的专用代码库。

    1. 主线池
        1. 线程 : 执行      js   while(){}



