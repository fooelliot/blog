#jvm


### jvm介绍


### jvm常见参数调优

-Xmx：最大JVM可用内存， 例：-Xmx4g

-Xms：最小JVM可用内存， 例：Xms4g

-Xmn：年轻代内存大小，例：-Xmn2560m

-XX:PermSize：永久代内存大小，该值太大会导致fullGC时间过长，太小将增加fullGC频率，例：-XX:PermSize=128m

-Xss：线程栈大小，太大将导致JVM可建的线程数量减少，例：-Xss256k

-XX:+DisableExplicitGC：禁止手动fullGC，如果配置，则System.gc()将无效，比如在为DirectByteBuffer分配空间过程中发现直接内存不足时会显式调用System.gc()

-XX:+UseConcMarkSweepGC：一般PermGen是不会被GC，如果希望PermGen永久代也能被GC，则需要配置该参数

-XX:+CMSParallelRemarkEnabled：GC进行时标记可回收对象时可以并行remark-XX:+UseCMSCompactAtFullCollection 表示在fullGC之后进行压缩，CMS默认不压缩空间

-XX:LargePageSizeInBytes：为java堆内存设置内存页大小，例：-XX:LargePageSizeInBytes=128m

-XX:+UseFastAccessorMethods：对原始类型进行快速优化

-XX:+UseCMSInitiatingOccupancyOnly：关闭预期开始的晋升率的统计

-XX:CMSInitiatingOccupancyFraction：使用cms作为垃圾回收，并设置GC百分比，例：-XX:CMSInitiatingOccupancyFraction=70（使用70％后开始CMS收集）

-XX:+PrintGCDetails：打印GC的详细信息

-XX:+PrintGCDateStamps：打印GC的时间戳

-Xloggc：指定GC文件路径