# grafana
## grafana 使用
### 安装grafana
1. weget
    - wget https://dl.grafana.com/oss/release/grafana-5.4.2-1.x86_64.rpm <rpm package url>
    - yum localinstall grafana-5.4.2-1.x86_64.rpm -y
2. install
    - yum install -y https://dl.grafana.com/oss/release/grafana-5.4.2-1.x86_64.rpm
3. 启动grafana
    - systemctl daemon-reload
    - systemctl start grafana-server  --启动
    - systemctl enable grafana-server.service  --设置自开机启动
    - 打开浏览器，输入 localhost:3000，账号密码默认值均为admin

### 插件安装
4. 查看远程插件
    - grafana-cli plugins list-remote|grep "zabbix"  -- 查看包含zabbix
    - 
### 使用dashboard
1. 添加数据源
 ```
    配置项目	说明
    Name	给数据源起一个名字。
    Default	选择默认，意味着数据源将预先选定为新的面板。
    Type	选择数据源的类型。
    Url	这里的Url是http协议，地址和端口是promethus提供的接口。或为promethus api地址。
    Access	访问代理，这里选择了proxy表示Grfana通过后端访问，还有direct值表示从浏览器直接访问目录。
    Username	输入zabbix的用户名，需要进行认证，一般使用Admin。
    Password	输入zabbix用户的密码，Admin默认密码为zabbix。
 ```
2. 创建dashboard
    - General（常规选择）
    - Metrics（指标）
    - Axes（坐标轴）
    - Legend（图例）
    - Display（显示样式）
    - Time range（时间范围）
    - Alert 报警(需要安装插件)
