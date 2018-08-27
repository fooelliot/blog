
# docker容器的常用软件安装过程

### 安装mysql

docker run -d --name mysql -p 3306:3306 -e MYSQL_ROOT_PASSWORD=R8t05b1fo6 -v /topshow/data/docker/mysql:/var/lib/mysql -v /etc/my.cnf:/etc/my.cnf mysql:5.7.23 

docker run --name mysql -p 3307:3307 -e MYSQL_ROOT_PASSWORD=root -v /export/data/mysql/data:/var/lib/mysql -v /export/data/mysql/my.cnf:/etc/my.cnf -d mysql

create user 'topshow'@'%' identified by 'M81oPdMtc';

grant replication slave on *.* to 'topshow'@'%';

flush privileges; 

### 安装redis
docker run -d --name redis -p 6379:6379 -v /topshow/data/docker/redis:/data redis redis-server --appendonly yes --requirepass "NE1Ik4aEm1I1Br8aYJ0nwPV"

docker run \
-p 6379:6379 \ # 端口映射 宿主机:容器
-v $PWD/data:/data:rw \ # 映射数据目录 rw 为读写
-v $PWD/conf/redis.conf:/etc/redis/redis.conf:ro \ # 挂载配置文件 ro 为readonly
--privileged=true \ # 给与一些权限
--name redis \ # 给容器起个名字
--appendonly yes \ # 开启数据持久化
-d redis redis-server /export/docker/redis/conf/redis.conf # deamon 运行 服务使用指定的配置文件

### 安装gitlab
docker run --detach \
    --hostname 120.76.77.230 \
    --publish 444:443 --publish 8088:8088 --publish 25:22 \
    --name gitlab \
    --restart always \
    --volume /export/gitlab/config:/etc/gitlab \
    --volume /export/gitlab/logs:/var/log/gitlab \
    --volume /export/gitlab/data:/var/opt/gitlab \
    gitlab/gitlab-ce:latest


### 安装nexus


### 安装Jenkins

docker run -d -p 8001:8001 -p 50000:50000 --name jenkins --privileged=true  -v /export/data/jenkins:/var/jenkins_home jenkins

### 安装java

some-mysql： 容器别名
my-secret-pw：初始化设置的root用户的密码
tag：mysql的版本，不写默认使用最新版
-p 3306:3306：表示在这个容器中使用3306端口(第二个)映射到本机的端口号也为3306(第一个)

java -jar -Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=5005 ./spirng-docker.jar


