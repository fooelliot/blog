# mysql文档



#### mysql用户管理常用操作
* 修改mysql的root密码
    > alter user 'root'@'localhost' identified by 'xxxx';
    > 
* 创建mysql用户
    > create user andy identified by 'admin';
    > grant all privileges on *.* to 'andy'@'%'identified by 'admin' with grant option;
    > flush privileges;


