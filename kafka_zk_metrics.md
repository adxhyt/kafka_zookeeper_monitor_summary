### description of kafka metrics
```
kafka version: 0.8.2.1
```

#### JMX Description
```
Java Management Extensions, 管理系统和资源之间的一个接口，它定义了管理系统和资源之间交互的标准

Managerment System <=> JMX <=> Resources

文档: [IBM JMX Description](http://www.ibm.com/developerworks/cn/java/j-lo-jse63/)
```

#### Kafka Metrics
<table class="table table-bordered table-striped table-condensed">
   <tr>
      <td> 参数 </td>
      <td> Mbean名称 </td>
      <td> 说明 </td>
   </tr>
   <tr>
      <td> 所有topic的写入消息速率(消息数/秒) </td>
      <td> kafka.server:type=BrokerTopicMetrics,name=MessagesInPerSec </td>
      <td> 所有topic消息(进出)流量</td>
   </tr>
   <tr>
      <td> 所有topic的流入数据速率(字节/秒) </td>
      <td> kafka.server:type=BrokerTopicMetrics,name=BytesInPerSec</td>
      <td> </td>
   </tr>
   <tr>
      <td> producer或Fetch-consumer或Fetch-follower的请求速率(请求次数/秒) </td>
      <td> kafka.network:type=RequestMetrics,name=RequestsPerSec,request={Produce|FetchConsumer|FetchFollower} </td>
      <td> </td>
   </tr>
   <tr>
      <td> 所有的topic的流出数据速率(字节/秒) </td>
      <td> kafka.server:type=BrokerTopicMetrics,name=BytesOutPerSec </td>
      <td> </td>
   </tr>
   <tr>
      <td> 刷日志的速率和耗时 </td>
      <td> kafka.log:type=LogFlushStats,name=LogFlushRateAndTimeMs </td>
      <td> </td>
   </tr>
   <tr>
      <td> 正在做复制的partition的数量 (|ISR| < |all replicas|) </td>
	  <td> kafka.server:type=ReplicaManager,name=UnderReplicatedPartitions </td>
      <td> 0 </td>
   </tr>
   <tr>
      <td> 当前的broker是否为controller </td>
	  <td> kafka.controller:type=KafkaController,name=ActiveControllerCount </td>
      <td> 集群中只有一个broker的这个值为1</td>
   </tr>
   <tr>
      <td> 选举leader的速率 </td>
      <td> kafka.controller:type=ControllerStats,name=LeaderElectionRateAndTimeMs </td>
      <td> 如果有broker挂了，此值非0</td>
   </tr>
   <tr>
      <td> Unclean的leader选举速率 </td>
	  <td> kafka.controller:type=ControllerStats,name=UncleanLeaderElectionsPerSec</td>
      <td> 0 </td>
   </tr>
   <tr>
      <td> 该broker上的partition的数量 </td>
      <td> kafka.server:type=ReplicaManager,name=PartitionCount </td>
      <td> 应在各个broker中平均分布</td>
   </tr>
   <tr>
      <td> Leader的replica的数量 </td>
      <td> kafka.server:type=ReplicaManager,name=LeaderCount </td>
      <td> 应在各个broker中平均分布</td>
   </tr>
   <tr>
      <td> ISR的收缩(shrink)速率 </td>
	  <td> kafka.server:type=ReplicaManager,name=IsrShrinksPerSec </td>
      <td> 如果一个broker挂掉了，一些partition的ISR会收缩。当那个broker重新起来时，一旦它的replica完全跟上，ISR会扩大(expand)。除此之外，正常情况下，此值和下面的扩大速率都是0</td>
   </tr>
   <tr>
      <td> ISR的扩大(expansion)速率 </td>
	  <td> kafka.server:type=ReplicaManager,name=IsrExpandsPerSec </td>
      <td> 同ISR的收缩(shrink)速率</td>
   </tr>
   <tr>
      <td> follower落后leader replica的最大的消息数量 </td>
	  <td> kafka.server:type=ReplicaFetcherManager,name=MaxLag,clientId=Replica </td>
      <td> 小于replica.lag.max.messages</td>
   </tr>
   <tr>
      <td> 等待producer purgatory的请求数 </td>
	  <td> kafka.server:type=ProducerRequestPurgatory,name=PurgatorySize </td>
      <td> 如果ack=-1，应为非0值</td>
   </tr>
   <tr>
      <td> 等待fetch purgatory的请求数 </td>
	  <td> kafka.server:type=FetchRequestPurgatory,name=PurgatorySize</td>
      <td> 依赖于consumer的fetch.wait.max.ms的设置</td>
   </tr>
   <tr>
      <td> 一个请求(producer,Fetch-Consumer,Fetch-Follower)耗费的所有时间 </td>
	  <td> kafka.network:type=RequestMetrics,name=TotalTimeMs,request={Produce|FetchConsumer|FetchFollower}</td>
      <td> 包括了queue, local, remote和response send time</td>
   </tr>
   <tr>
      <td>发送响应花费的时间 </td>
	  <td> kafka.network:type=RequestMetrics,name=ResponseSendTimeMs,request={Produce|FetchConsumer|FetchFollower} </td>
      <td> </td>
   </tr>
   <tr>
      <td>consumer落后producer的消息数量 </td>
	  <td> kafka.consumer:type=ConsumerFetcherManager,name=MaxLag,clientId=([-.\w]+) </td>
   </tr>
</table>

#### ZooKeeper Info
```
echo mntr | nc localhost 2181
```
<table class="table table-bordered table-striped table-condensed">
   <tr>
      <td> Key </td>
      <td> Value </td>
      <td> Description </td>
   </tr>
   <tr>
      <td> zk_avg_latency </td>
      <td> 0 </td>
      <td> 平均时延</td>
   </tr>
   <tr>
      <td> zk_max_latency </td>
      <td> 0 </td>
      <td>  </td>
   </tr>
   <tr>
      <td> zk_max_latency </td>
      <td> 0 </td>
      <td>  </td>
   </tr>
   <tr>
      <td> zk_packets_received </td>
      <td> 123 </td>
      <td> 接收到的数据包</td>
   </tr>
   <tr>
      <td> zk_packets_sent </td>
      <td> 123 </td>
      <td> 发送的数据包 </td>
   </tr>
   <tr>
      <td> zk_znode_count </td>
      <td> 600 </td>
      <td> znode数量 zookeeper中实现了一个类似file system系统的数据结构，比如/zookeeper/status. 每个节点都对应于一个znode节点</td>
   </tr>
   <tr>
      <td> zk_watch_count </td>
      <td> 300 </td>
      <td> znode watch数量 zookeeper为解决数据的一致性，使用了Watcher的异步回调接口，将服务端znode的变化以事件的形式通知给客户端</td>
   </tr>
   <tr>
      <td> zk_ephemerals_count </td>
	  <td> 200 </td>
      <td> znode临时节点 分布式集群是否存活监控. (每个节点启动后，注册一个EPHEMERAL节点到zookeeper中，注册一个Watcher获取EPHEMERAL节点的存在情况，消失即可代表集群节点dead)</td>
   </tr>
</table>

