## kafka_zookeeper_monitor_summary
the summary of work on kafka and zookeeper monitor

### kafka监控

```
kafka属于apache生态圈的产品 可以采用获取jmx metrics获取其运行状态
```
#### jconsole或者VisualVM监控
```
kafka启动的时候指定jmx的端口 
JMX_PORT=9999 ./kafka-server-start.sh ../config/server.properties &
```

#### 通过mx4j-tools查看
```
查看kafka源码 发现kafka/utils/Mx4jLoader.scala中
存在通过load mx4j-tools.jar来实现MX4J的http调用
默认"mx4jport", 8082

loadpath在kafka/libs目录下 
将mx4j-3.0.2.jar 和 mx4j-tools-3.0.1.jar放到对应目录(主要起作用的应该是mx4j-tools-3.0.1.jar) 
然后重启kafka broker 直接可以查看MX4J的页面
```

#### 通过jmxtrans查看
```
安装jmxtrans
通过Json来配置,收集指定的Kafka运行状态数据
```

#### jmxtrans操作步骤
安装jmxtrans步骤参见: [jmxtrans installation](https://github.com/jmxtrans/jmxtrans/wiki/Installation)
```
操作步骤
1. 修改 kafka/bin/kaka-server-start.sh  添加jmx_port 如下：
      # new add for jmxtrans        
      export JMX_PORT=8086
2. 修改kafka/bin/kafka-run-class.sh  修改：JMX port to use如下
      # JMX port to use     
      if [ $JMX_PORT ]; then           
          HOST_IP=`hostname -i`            
          KAFKA_JMX_OPTS="$KAFKA_JMX_OPTS -Dcom.sun.management.jmxremote.port=$JMX_PORT -Djava.rmi.server.hostname=$HOST_IP"      
	  fi
3. 重启kafka服务
4. cd /usr/share/jmxtrans    
     4.1 建立jmxtrans.conf文件
          # configuration file for package jmxtrans
          export JAR_FILE="/usr/share/jmxtrans/jmxtrans-all.jar"
          export LOG_DIR="/usr/share/jmxtrans/runlog"
          export SECONDS_BETWEEN_RUNS=60
          export JSON_DIR="/usr/share/jmxtrans/runlog"
          export HEAP_SIZE=1024
          export NEW_SIZE=64
          export CPU_CORES=32
          export NEW_RATIO=8
          export LOG_LEVEL=debug
		  JMXTRANS_OPTS="-Dmyserverhost=`hostname -i` -Dmyserverport=8086"
     4.2 新建kafka_output.json文件 将附件配置放入
5. 启动  /usr/share/jmxtrans/jmxtrans.sh  start kafka_output.json
```

#### kafka监控项描述
官方文档: [kafka monitoring](http://kafka.apache.org/documentation.html#monitoring)

### zookeeper监控
官方文档: [zookeeper monitoring](http://zookeeper.apache.org/doc/trunk/zookeeperAdmin.html#sc_zkCommands)
```
The ZooKeeper service can be monitored in one of two primary ways; 1) the command port through the use of 4 letter words and 2) JMX.
```

#### zookeeper 4 letter words 
```
一个shell脚本解决
```
<pre><code>
#!/bin/bash
echo mntr | nc 127.0.0.1 2181 | egrep 'count|connections|latency' | while read metric value
do
	metric="zk.$metric"
	echo $metric $value
done
</code></pre>

#### zookeeper JMX monitor 
```
打开zookeeper/bin/zkServer.sh
修改ZOOMAIN参数
	ZOOMAIN="-Dcom.sun.management.jmxremote -Dcom.sun.management.jmxremote.port={$port} -Dcom.sun.management.jmxremote.authenticate=false -Dcom.sun.management.jmxremote.ssl=false -Dcom.sun.management.jmxremote.local.only=false org.apache.zookeeper.server.quorum.QuorumPeerMain"
重启zkServer.sh restart zookeeper/conf/zk.cfg
```
