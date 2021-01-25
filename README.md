# zookeeper部署
```
kubectl apply -f kafka-ha/zookeeper.yaml
```

验证

```
[root@master kafka]# for i in 0 1 2; do echo "myid zk-$i";kubectl exec zk-$i -nkafka -- cat /var/lib/zookeeper/data/myid; done
myid zk-0
1
myid zk-1
2
myid zk-2
1
[root@master kafka]#  for i in 0 1 2; do echo "myid zk-$i";kubectl exec zk-$i -nkafka -- cat /var/lib/zookeeper/data/myid; done
myid zk-0
1
myid zk-1
2
myid zk-2
1
[root@master kafka]#  for i in 0 1 2; do kubectl exec zk-$i -nkafka -- hostname -f; done
zk-0.zk-hs.kafka.svc.cluster.local
zk-1.zk-hs.kafka.svc.cluster.local
zk-2.zk-hs.kafka.svc.cluster.local


```

查看broker

```
[root@master kafka]#  kubectl exec -ti zk-1 -n kafka bash
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl kubectl exec [POD] -- [COMMAND] instead.
root@zk-1:/# zkCli.sh
Connecting to localhost:2181
2021-01-25 16:26:31,911 [myid:] - INFO  [main:Environment@100] - Client environment:zookeeper.version=3.4.10-39d3a4f269333c922ed3db283be479f9deacaa0f, built on 03/23/2017 10:13 GMT
2021-01-25 16:26:31,914 [myid:] - INFO  [main:Environment@100] - Client environment:host.name=zk-1.zk-hs.kafka.svc.cluster.local
2021-01-25 16:26:31,914 [myid:] - INFO  [main:Environment@100] - Client environment:java.version=1.8.0_131
2021-01-25 16:26:31,918 [myid:] - INFO  [main:Environment@100] - Client environment:java.vendor=Oracle Corporation
2021-01-25 16:26:31,919 [myid:] - INFO  [main:Environment@100] - Client environment:java.home=/usr/lib/jvm/java-8-openjdk-amd64/jre
2021-01-25 16:26:31,920 [myid:] - INFO  [main:Environment@100] - Client environment:java.class.path=/usr/bin/../build/classes:/usr/bin/../build/lib/*.jar:/usr/bin/../share/zookeeper/zookeeper-3.4.10.jar:/usr/bin/../share/zookeeper/slf4j-log4j12-1.6.1.jar:/usr/bin/../share/zookeeper/slf4j-api-1.6.1.jar:/usr/bin/../share/zookeeper/netty-3.10.5.Final.jar:/usr/bin/../share/zookeeper/log4j-1.2.16.jar:/usr/bin/../share/zookeeper/jline-0.9.94.jar:/usr/bin/../src/java/lib/*.jar:/usr/bin/../etc/zookeeper:
2021-01-25 16:26:31,920 [myid:] - INFO  [main:Environment@100] - Client environment:java.library.path=/usr/java/packages/lib/amd64:/usr/lib/x86_64-linux-gnu/jni:/lib/x86_64-linux-gnu:/usr/lib/x86_64-linux-gnu:/usr/lib/jni:/lib:/usr/lib
2021-01-25 16:26:31,920 [myid:] - INFO  [main:Environment@100] - Client environment:java.io.tmpdir=/tmp
2021-01-25 16:26:31,920 [myid:] - INFO  [main:Environment@100] - Client environment:java.compiler=<NA>
2021-01-25 16:26:31,921 [myid:] - INFO  [main:Environment@100] - Client environment:os.name=Linux
2021-01-25 16:26:31,921 [myid:] - INFO  [main:Environment@100] - Client environment:os.arch=amd64
2021-01-25 16:26:31,922 [myid:] - INFO  [main:Environment@100] - Client environment:os.version=4.18.0-240.10.1.el8_3.x86_64
2021-01-25 16:26:31,922 [myid:] - INFO  [main:Environment@100] - Client environment:user.name=root
2021-01-25 16:26:31,922 [myid:] - INFO  [main:Environment@100] - Client environment:user.home=/root
2021-01-25 16:26:31,922 [myid:] - INFO  [main:Environment@100] - Client environment:user.dir=/
2021-01-25 16:26:31,924 [myid:] - INFO  [main:ZooKeeper@438] - Initiating client connection, connectString=localhost:2181 sessionTimeout=30000 watcher=org.apache.zookeeper.ZooKeeperMain$MyWatcher@22d8cfe0
Welcome to ZooKeeper!
2021-01-25 16:26:31,946 [myid:] - INFO  [main-SendThread(localhost:2181):ClientCnxn$SendThread@1032] - Opening socket connection to server localhost/127.0.0.1:2181. Will not attempt to authenticate using SASL (unknown error)
JLine support is enabled
2021-01-25 16:26:31,996 [myid:] - INFO  [main-SendThread(localhost:2181):ClientCnxn$SendThread@876] - Socket connection established to localhost/127.0.0.1:2181, initiating session
[zk: localhost:2181(CONNECTING) 0] 2021-01-25 16:26:32,082 [myid:] - INFO  [main-SendThread(localhost:2181):ClientCnxn$SendThread@1299] - Session establishment complete on server localhost/127.0.0.1:2181, sessionid = 0x2773a44c70e0007, negotiated timeout = 30000

WATCHER::

WatchedEvent state:SyncConnected type:None path:null

[zk: localhost:2181(CONNECTED) 0] ls /
[cluster, controller_epoch, controller, brokers, zookeeper, admin, isr_change_notification, consumers, config]
[zk: localhost:2181(CONNECTED) 1] ls /brokers
[ids, topics, seqid]
[zk: localhost:2181(CONNECTED) 2] ls /brokers/

ids      topics   seqid
[zk: localhost:2181(CONNECTED) 2] ls /brokers/ids
[0, 1, 2]
[zk: localhost:2181(CONNECTED) 3] get /brokers/ids/0
{"listener_security_protocol_map":{"PLAINTEXT":"PLAINTEXT"},"endpoints":["PLAINTEXT://kafka-0.kafka-hs.kafka.svc.cluster.local:9093"],"jmx_port":-1,"host":"kafka-0.kafka-hs.kafka.svc.cluster.local","timestamp":"1611591513363","port":9093,"version":4}
cZxid = 0x10000003b
ctime = Mon Jan 25 16:18:33 UTC 2021
mZxid = 0x10000003b
mtime = Mon Jan 25 16:18:33 UTC 2021
pZxid = 0x10000003b
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x1773a44c7240001
dataLength = 250
numChildren = 0
[zk: localhost:2181(CONNECTED) 4] get /brokers/ids/1
{"listener_security_protocol_map":{"PLAINTEXT":"PLAINTEXT"},"endpoints":["PLAINTEXT://kafka-1.kafka-hs.kafka.svc.cluster.local:9093"],"jmx_port":-1,"host":"kafka-1.kafka-hs.kafka.svc.cluster.local","timestamp":"1611591514801","port":9093,"version":4}
cZxid = 0x100000042
ctime = Mon Jan 25 16:18:34 UTC 2021
mZxid = 0x100000042
mtime = Mon Jan 25 16:18:34 UTC 2021
pZxid = 0x100000042
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x1773a44c7240002
dataLength = 250
numChildren = 0
[zk: localhost:2181(CONNECTED) 5] get /brokers/ids/2
{"listener_security_protocol_map":{"PLAINTEXT":"PLAINTEXT"},"endpoints":["PLAINTEXT://kafka-2.kafka-hs.kafka.svc.cluster.local:9093"],"jmx_port":-1,"host":"kafka-2.kafka-hs.kafka.svc.cluster.local","timestamp":"1611591513829","port":9093,"version":4}
cZxid = 0x10000003e
ctime = Mon Jan 25 16:18:33 UTC 2021
mZxid = 0x10000003e
mtime = Mon Jan 25 16:18:33 UTC 2021
pZxid = 0x10000003e
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x2773a44c70e0003
dataLength = 250
numChildren = 0
[zk: localhost:2181(CONNECTED) 6] 

```



# kafka部署

```
kubectl apply -f kafka-ha/kafka.yaml
```

```
[root@master kafka]# kubectl get pod -owide -nkafka -w
NAME      READY   STATUS    RESTARTS   AGE   IP             NODE     NOMINATED NODE   READINESS GATES
kafka-0   1/1     Running   0          14m   10.244.1.186   node01   <none>           <none>
kafka-1   1/1     Running   0          14m   10.244.2.99    node02   <none>           <none>
kafka-2   1/1     Running   0          14m   10.244.1.187   node01   <none>           <none>
zk-0      1/1     Running   0          49m   10.244.1.178   node01   <none>           <none>
zk-1      1/1     Running   0          47m   10.244.2.94    node02   <none>           <none>
zk-2      1/1     Running   0          47m   10.244.1.179   node01   <none>           <none>

```



## 验证

```
[root@k8s-master sts]# kubectl exec -it kafka-0 -n kafka sh
$ cd/opt/kafka/bin
#创建test topic
$ ./kafka-topics.sh --create --topic test --zookeeper zk-0.zk-hs.kafka.svc.cluster.local:2181  --partitions 3 --replication-factor 3
Created topic "test".
#查看topic
$ ./kafka-topics.sh --zookeeper zk-0.zk-hs.kafka.svc.cluster.local:2181 --list
test
```

## 模拟生产者,消费者【开2个终端 输入输出】

```
#生产者
$   ./kafka-console-producer.sh --topic test --broker-list kafka-0.kafka-hs.kafka.svc.cluster.local:9093,kafka-1.kafka-hs.kafka.svc.cluster.local:9093,kafka-2.kafka-hs.kafka.svc.cluster.local:9093

this is a test message
hell world
#消费者消费数据
$  ./kafka-console-consumer.sh --topic test --zookeeper zookeeper zk-0.zk-hs.kafka.svc.cluster.local:2181 --from-beginning
Using the ConsoleConsumer with old consumer is deprecated and will be removed in a future major release. Consider using the new consumer by passing [bootstrap-server] instead of [zookeeper].
hell world
this is a test message
```

## 业务接入

## k8s集群内部域名:

broker-list: kafka-0.kafka-hs.kafka.svc.cluster.local:9093,kafka-1.kafka-hs.kafka.svc.cluster.local:9093,kafka-2.kafka-hs.kafka.svc.cluster.local:9093

## k8s集群外部域名:

broker-list:

zookeeper:

通过nodeport:

zk:

```
apiVersion: v1
kind: Service
metadata:
  name: zk-cs
  namespace: bigdata
  labels:
    app: zk
spec:
  type: NodePort
  ports:
  - port: 2181
    targetPort: 2181
    name: client
    nodePort: 32181
  selector:
    app: zk
```

外网访问:

 ./bin/zkCli.sh -server 192.168.240.102:32181

```
[zk: 192.168.100.102:32181(CONNECTED) 1] ls /
[cluster, controller_epoch, brokers, zookeeper, admin, isr_change_notification, consumers, hello, config]
[zk: 192.168.100.102:32181(CONNECTED) 2] get /hello  
world
cZxid = 0x100000004
ctime = Wed Nov 13 10:41:56 CST 2019
mZxid = 0x100000004
mtime = Wed Nov 13 10:41:56 CST 2019
pZxid = 0x100000004
cversion = 0
dataVersion = 0
aclVersion = 0
ephemeralOwner = 0x0
dataLength = 5
numChildren = 0
[zk: 192.168.100.102:32181(CONNECTED) 3] 
```

# TODO

亲和策略配置