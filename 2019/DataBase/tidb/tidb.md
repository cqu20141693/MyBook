# tidb
### tidb server
1. 使用笔记
 ```
    登录名和密码： 
    TiDB支持mysql协议可以使用任意mysql客户端连接，默认安装好的集群使用mysql登录，端口：4000，用户名：root，密码为空，修改密码跟mysql修改密码方式完全一样。
    SET PASSWORD FOR 'root'@'%' = 'xxx';

    root 用户： 默认密码为空；拥有所有权限
    查询用户表： select * from mysql.`user`
    查询一个用户的权限 ： show grants for 'root'@'%'
    创建一个用户： CREATE USER 'dev'@'%' IDENTIFIED BY 'dev';

 ```
2. 用户和权限管理
 ```
    以用户dev 为例：
    授予所有权限： GRANT ALL PRIVILEGES ON *.* TO 'dev'@'%';
    授予test数据库的所有权限：  GRANT ALL PRIVILEGES ON test.* TO 'dev'@'%';
    授予test库select权限： GRANT SELECT ON test.* TO 'dev'@'%';
    授予test.user_info 表所有权限： GRANT ALL PRIVILEGES ON test.user_info TO 'dev'@'%';

    收回test库所有权限： REVOKE ALL PRIVILEGES ON `test`.* FROM 'dev'@'%';

    修改表结构： alter table user_info add COLUMN sex SMALLINT(1) NOT NULL COMMENT ' 用户性别';
 ```
### pb server
### tikv server
