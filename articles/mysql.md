# mysql文档



#### mysql用户管理常用操作
* 修改mysql的root密码
    > alter user 'root'@'localhost' identified by 'xxxx';

##### mysql创建用户
- 创建普通用户
> $ CREATE USER '用户名称'@'主机名称'
> $ CREATE USER 'jack';
- 创建带有主机名的用户
    > CREATE USER '用户名称'@'主机名称' INDENTIFIED BY '用户密码'
    > create user 'name'@'localhost' identified by 'password';
    > grant all privileges on *.* to 'andy'@'%'identified by 'admin' with grant option;
    > flush privileges;
- 验证
> $ select user,host from mysql.user;

##### 使用GRANT来创建用户，以及授予权限
> grant 权限 on 数据库.表 to '用户名'@'登录主机'  [INDENTIFIED BY '用户密码'];
权限： select ,update,delete,insert(表数据)、create,alert,drop(表结构)、references(外键)、create temporary tables(创建临时表)、index(操作索引)、create view,show view(视图)、create routine,alert routine,execute(存储过程)、all,all privileges(所有权限)

数据库：[数据库名]或者[*]代表所有数据库
表：[表名]或者[*]某数据库下所有表，[*.*]表示所有数据库的所有表
主机:[主机名]或者[%]代表任何主机
例如:grant selec,insert,update,delete on *.* to 'jack'@'%';

##### 删除MYSQL的用户
delete from mysql.user where user='用户名称' and host='主机名称';
例：DELETE FROM mysql.user WHERE user='user3' AND host='localhost';
删除后使用：FLUSH PRIVILEGES 来刷新权限

### linux下安装mysql8.0
* 下载mysql 
> $ wget http://mirrors.ustc.edu.cn/mysql-ftp/Downloads/MySQL-8.0/mysql-8.0.4-rc-linux-glibc2.12-x86_64.tar.gz
* 解压
> $ mysql tar -zxvf mysql-8.0.4-rc-linux-glibc2.12-x86_64.tar.gz -C /usr/local/
* 修改文件夹名称 
> $ mv mysql-8.0.4-rc-linux-glibc2.12-x86_64/ mysql
* 添加默认配置文件
> $ vim/etc/my.cnf

```
[client]
port=3306
socket=/tmp/mysql/mysql.sock

[mysqld]
port=3306
user=mysql
socket=/tmp/mysql/mysql.sock
basedir=/usr/local/mysql
datadir=/usr/local/mysql/data
log-error=error.log
```

* 创建mysql组和mysql用户
> $ groupadd mysql
> $ useradd -g mysql mysql

* 初始化mysql 
> $ /usr/local/mysql/bin/mysqld  --initialize --user=mysql --basedir=/usr/local/mysql/ --datadir=/usr/local/mysql/data/


* 在初始化过程中可能会遇到错误，日志如下可以修改/tmp/mysql的目录权限
> $ chown -R mysql:mysql /tmp/mysql

```
2018-07-08T02:53:24.542370Z 0 [System] [MY-010116] /usr/local/mysql/bin/mysqld (mysqld 8.0.4-rc) starting as process 17745 ...
mysqld: Can't create/write to file '/tmp/mysql/data/ibd35qXQ' (Errcode: 13 - Permission denied)
2018-07-08T02:53:24.554816Z 1 [ERROR] [MY-011066] InnoDB: Unable to create temporary file; errno: 13
2018-07-08T02:53:24.554856Z 1 [ERROR] [MY-011066] InnoDB: InnoDB Database creation was aborted with error Generic error. You may need to delete the ibdata1 file before trying to start up again.
2018-07-08T02:53:24.555000Z 0 [ERROR] [MY-010020] Data Dictionary initialization failed.
2018-07-08T02:53:24.555033Z 0 [ERROR] [MY-010119] Aborting
2018-07-08T02:53:24.555919Z 0 [System] [MY-010910] /usr/local/mysql/bin/mysqld: Shutdown complete.
```

* 如果无异常情况日志如下可以看到mysql默认会生成root账号和密码root@localhost: J&:rqhMd/9>f
```
2018-07-08T02:57:57.631578Z 0 [System] [MY-010116] /usr/local/mysql/bin/mysqld (mysqld 8.0.4-rc) starting as process 18894 ...
2018-07-08T02:58:03.499117Z 0 [Warning] [MY-010068] CA certificate ca.pem is self signed.
2018-07-08T02:58:03.718786Z 5 [Note] [MY-010454] A temporary password is generated for root@localhost: J&:rqhMd/9>f
```

* 接下来可以启动mysql服务器
> $ ./support-files/mysql.server start
* 修改mysql的默认初始化密码
> $ alter user 'root'@'%' identified with mysql_native_password by 'root';
* 创建用户 
> $ create user 'jack'@'localhost' identified by 'jack';
* 授予权限 
> $ grant replication slave on *.* to 'jack'@'localhost';
* 刷新
> $ flush privileges; 
* 修改root用户可以远程连接
> $ update mysql.user set host='%' where user='root'

CHANGE MASTER TO MASTER_HOST='localhost', MASTER_USER='jack', MASTER_PASSWORD='jack',MASTER_LOG_FILE='filename',
MASTER_LOG_POS=position;

change master to master_host='39.108.125.41',master_user='jack',master_password='jack',master_log_file='mysql-bin.000001',master_log_pos=154

set GLOBAL SQL_SLAVE_SKIP_COUNTER=1;
