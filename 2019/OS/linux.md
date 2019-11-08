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
    

### 文件上传和下载
1. rz 上传文件到服务器 ： 会打开一个上传的窗口选择文件上传；
2. sz 下载文件

### 文件复制和移动，删除
1. cp [-adfilprsu] 源文件(source) 目标文件(destination)
 ```
    参数说明：
    -a:是指archive的意思，也说是指复制所有的目录
    -d:若源文件为连接文件(link file)，则复制连接文件属性而非文件本身
    -f:强制(force)，若有重复或其它疑问时，不会询问用户，而强制复制
    -i:若目标文件(destination)已存在，在覆盖时会先询问是否真的操作
    -l:建立硬连接(hard link)的连接文件，而非复制文件本身
    -p:与文件的属性一起复制，而非使用默认属性
    -r:递归复制，用于目录的复制操作
    -s:复制成符号连接文件(symbolic link)，即“快捷方式”文件
    -u:若目标文件比源文件旧，更新目标文件 
 ```
2. mv [-fiv] source destination
 ```
    -f:force，强制直接移动而不询问
    -i:若目标文件(destination)已经存在，就会询问是否覆盖
    -u:若目标文件已经存在，且源文件比较新，才会更新
 ```
3. rm [fir] 文件或目录
```
    参数说明：
    -f:强制删除
    -i:交互模式，在删除前询问用户是否操作
    -r:递归删除，常用在目录的删除
```