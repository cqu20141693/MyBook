# jmeter
### jemter 使用
1. [官网下载压缩包](http://jmeter.apache.org/download_jmeter.cgi);运行jemter.bat;
2. 测试计划: (.jmx文件) 
    - 包含测试计划的名称和用户定义的变量
    - 用户组
        - 右击“测试计划”>添加>Threads（Users）>线程组
        - 线程数 : 表示想要模拟的用户数量
        - Ramp-Up Period (in seconds): 表示在多少时间内完成;
        - 循环次数 : 执行测试的次数
        - 调度程序配置: 测试持续时间，启动延迟，运行的开始和结束时间
    - 监听器 : 监听器提供JMeter收集的关于那些测试用例的数据的图形表示
        - 监听器的列表：图表结果,样条曲线可视化器,断言结果...
    - 逻辑控制器 : 控制线程中采样器处理顺序的流程
        - 循环,运行时,if,事务,简单,switch 控制器...
    - 样本生成控制器(采样器)
    - 定时器
    - 断言
    - **配置元素**
        - HTTP请求默认值 : 方便改用户组的请求
            - 协议
            - 域名
            - 端口
        - HTTP信息头管理器 : 
            - Content-Type:application/json

3. 一个完整的测试计划:
    - 线程组
    - 默认的配置元素
    - 需要测试的请求
    - 断言
    - 请求结果树
    - 请求总结报告
    - 通过命令执行 : .\jmeter.bat -n -t ..\测试内容\一个完整的测试计划.jmx -l ..\testplan/result/result.txt -e -o ..\testplan/webreport
