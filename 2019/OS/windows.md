# windows
## 快捷键
### windows 使用
1. 如果添加一个命令到鼠标右击中
    - 通过注册表实现
2. network command
    - netstat –ano|findstr [指定端口号]  : 查看指定端口的进程
    - tasklist |findstr [pid 号]  : 查看占用的程序
3. 删除指定pid的进程
    - taskkill /f /pid [pid号]
