### linux下安装mysql8
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