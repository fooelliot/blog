# hadoop

### hadoop的配置和安装
* 配置Linux的java环境和ssh免密登录
```
        ssh-keygen
        ssh-copy-id -i ~/.ssh/id_rsa.put localhost
```
* 安装配置hadoop
```
    > 第一个：hadoop-env.sh
        export JAVA_HOME=/usr/java/jdk1.8.0_172
        
    > 第二个：core-site.xml
        <!-- 指定HADOOP所使用的文件系统schemaHDFS的老大（NameNode）的地址 -->
        <property>
            <name>fs.defaultFS</name>
            <value>hdfs://hadoop1:9000</value>
        </property>
        <!-- 指定hadoop运行时产生文件的存储目录 -->
        <property>
            <name>hadoop.tmp.dir</name>
            <value>/home/hadoop/hadoop-2.4.1/tmp</value>
    </property>


    > 第三个：hdfs-site.xml   
        <!-- 指定HDFS副本的数量 -->
        <property>
            <name>dfs.replication</name>
            <value>1</value>
        </property>
        <property>
            <name>dfs.secondary.http.address</name>
            <value>192.168.1.152:50090</value>
        </property>

    > 第四个：mapred-site.xml (mv mapred-site.xml.template mapred-site.xml)
        <!-- 指定mr运行在yarn上 -->
        <property>
            <name>mapreduce.framework.name</name>
            <value>yarn</value>
        </property>
        
    > 第五个：yarn-site.xml
        <!-- 指定YARN的老大（ResourceManager）的地址 -->
        <property>
            <name>yarn.resourcemanager.hostname</name>
            <value>hadoop1</value>
        </property>
        <!-- reducer获取数据的方式 -->
        <property>
            <name>yarn.nodemanager.aux-services</name>
            <value>mapreduce_shuffle</value>
        </property>
        
* 将hadoop添加到环境变量
    vim /etc/proflie
        export HADOOP_HOME=/export/servers/hadoop-2.8.1
        export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
    source /etc/profile
    
    > 格式化namenode（是对namenode进行初始化）
        hdfs namenode -format (hadoop namenode -format)
        
    > 启动hadoop
        先启动HDFS
        sbin/start-dfs.sh
        再启动YARN
        sbin/start-yarn.sh
        
    > 验证是否启动成功
        使用jps命令验证
        27408 NameNode
        28218 Jps
        27643 SecondaryNameNode
        28066 NodeManager
        27803 ResourceManager
        27512 DataNode
        http://192.168.1.101:50070 （HDFS管理界面）
        http://192.168.1.101:8088 （MR管理界面）
```
    
    
    
    
