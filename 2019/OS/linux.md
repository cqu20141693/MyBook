# linux
### linux 查用命令
1. 查看linux 内核版本
    - cat /proc/version
    - uname -a
2. 查看Linux系统版本的命令
    - lsb_release -a
    - cat /etc/issue

### linux 文件命令
#### 文件编辑
1. vi 编辑命令
    - vi /etc/sudoers
    - i/insert  --进入修改模式
    - esc  --进入命令模式 之后可进行以下操作
     ```
        :w 保存文件但不退出vi
    　　:w file 将修改另外保存到file中，不退出vi
    　　:w! 强制保存，不推出vi
    　　:wq 保存文件并退出vi
    　　:wq! 强制保存文件，并退出vi
    　　q: 不保存文件，退出vi
    　　:q! 不保存文件，强制退出vi
    　　:e! 放弃所有修改，从上次保存文件开始再编辑
     ```

#### 文件阅读
1. cat
2. tail
    