# jmeter
### jemter 使用
1. [官网下载压缩包](http://jmeter.apache.org/download_jmeter.cgi);运行jemter.bat;
2. 测试计划: (.jmx文件) 
    1. 包含测试计划的名称和用户定义的变量
    2. 用户组
        1. 右击“测试计划”>添加>Threads（Users）>线程组
        2. 线程数 : 表示想要模拟的用户数量
        3. Ramp-Up Period (in seconds): 表示在多少时间内完成;
        4. 循环次数 : 执行测试的次数
        5. 调度程序配置: 测试持续时间，启动延迟，运行的开始和结束时间
    3. 监听器 : 监听器提供JMeter收集的关于那些测试用例的数据的图形表示
        1. 监听器的列表：图表结果,样条曲线可视化器,断言结果...
    4. 逻辑控制器 : 控制线程中采样器处理顺序的流程
        1. 循环,运行时,if,事务,简单,switch 控制器...
    5. 样本生成控制器(采样器)
    6. 定时器
    7. 断言
    8. **配置元素**
        1. HTTP请求默认值 : 方便改用户组的请求
            1. 协议
            2. 域名
            3. 端口
        2. HTTP信息头管理器 : 
            1. Content-Type:application/json

3. 一个完整的测试计划:
    1. 线程组
    1. 默认的配置元素
    2. 需要测试的请求
    3. 断言
    4. 请求结果树
    5. 请求总结报告
    6. 通过命令执行 : .\jmeter.bat -n -t ..\测试内容\一个完整的测试计划.jmx -l ..\testplan/result/result.txt -e -o ..\testplan/webreport
