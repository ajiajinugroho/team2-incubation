# Hands-on Incubation (Install Kafka manual)

## Install Confluent bisa lihat dokumentasi

## Mulai dari SSL/TLS
- server.properties
- client-ssl.properties
- zookeeper.properties
- zk-client.properties
- consumer.properties
- producer.properties

## Untuk praktik
- create topic
- delete topic
- produce topic
- consume topic
- describe topic
- list topic

----------------------------------------------------------------------------------------------

## Konfigurasi yang dilakukan untuk menerapkan keamanan SSL/TLS

### server.properties
Konfigurasi SSL/TLS di Kafka meliputi pengaturan `server.properties` untuk mendefinisikan ID broker, listener dengan protokol SSL, lokasi keystore dan truststore, serta pengaturan keamanan untuk komunikasi antar broker dan otorisasi klien, guna memastikan keamanan komunikasi.

```
	broker.id=1
	listeners=SSL://:9092
	advertised.listeners=SSL://node2.kafka:9092
	listener.security.protocol.map=SSL:SSL
	log.dirs=/data/kafka
	confluent.license.topic.replication.factor=1
	zookeeper.clientCnxnSocket=org.apache.zookeeper.ClientCnxnSocketNetty
	zookeeper.ssl.client.enable=true
	zookeeper.ssl.protocol=TLSv1.2
	zookeeper.ssl.truststore.location=/var/ssl/private/broker1/kafka.broker1.truststore.jks
	zookeeper.ssl.truststore.password=confluent
	zookeeper.ssl.keystore.location=/var/ssl/private/broker1/kafka.broker1.keystore.jks
	zookeeper.ssl.keystore.password=confluent
	ssl.key.password=confluent
	ssl.truststore.location=/var/ssl/private/broker1/kafka.broker1.truststore.jks
	ssl.truststore.password=confluent
	ssl.keystore.location=/var/ssl/private/broker1/kafka.broker1.keystore.jks
	ssl.keystore.password=confluent
	ssl.key.password=confluent
	security.inter.broker.protocol=SSL
	ssl.client.auth=required
	ssl.protocol=TLSv1.2
```

### client-ssl.properties
Konfigurasi SSL/TLS di `client-ssl.properties`

```
bootstrap.servers=node1.kafka:9092,node2.kafka:9092,node3.kafka:9092,node4.kafka:9092
security.protocol=SSL
ssl.keystore.location=/var/ssl/private/broker1/kafka.broker1.keystore.jks
ssl.keystore.password=confluent
ssl.truststore.location=/var/ssl/private/broker1/kafka.broker1.truststore.jks
ssl.truststore.password=confluent
```


### zookeeper.properties
Konfigurasi SSL/TLS di `zookeeper.properties`
```
4lw.commands.whitelist=*
admin.enableServer=false
authProvider.x509=org.apache.zookeeper.server.auth.X509AuthenticationProvider
autopurge.purgeInterval=1
autopurge.snapRetainCount=10
dataLogDir=var/lib/zookeeper/data-log
initLimit=5
maxClientCnxns=0
secureClientPort=2182
server.1=node1.zookeeper:2888:3888
server.2=node2.zookeeper:2888:3888
server.3=node4.zookeeper:2888:3888
serverCnxnFactory=org.apache.zookeeper.server.NettyServerCnxnFactory
ssl.clientAuth=none
ssl.keyStore.location=/var/ssl/private/zookeeper-cert/kafka.ZOOKEEPER.keystore.jks
ssl.keyStore.password=confluent
ssl.trustStore.location=/var/ssl/private/zookeeper-cert/kafka.ZOOKEEPER.truststore.jks
ssl.trustStore.password=confluent
syncLimit=2
```


### zk-client.properties
Konfigurasi SSL/TLS di `zk-client.properties`
```
zookeeper.clientCnxnSocket=org.apache.zookeeper.ClientCnxnSocketNetty
zookeeper.ssl.client.enable=true
zookeeper.ssl.protocol=TLSv1.2
zookeeper.ssl.truststore.location=/var/ssl/private/zookeeper-client-cert/kafka.ZOOKEEPER-CLIENT.truststore.jks
zookeeper.ssl.truststore.password=confluent
zookeeper.ssl.keystore.location=/var/ssl/private/zookeeper-client-cert/kafka.ZOOKEEPER-CLIENT.keystore.jks
zookeeper.ssl.keystore.password=confluent
ssl.key.password=confluent
```


### consumer.properties
Konfigurasi SSL/TLS di `consumer.properties`
```
bootstrap.servers=node1.kafka:9092,node2.kafka:9092,node3.kafka:9092,node4.kafka:9092
group.id=test-consumer-group
ssl.keystore.location=/var/ssl/private/consumer-cert/kafka.consumer.keystore.jks
ssl.keystore.password=confluent
ssl.keystore.type=JKS
ssl.truststore.location=/var/ssl/private/consumer-cert/kafka.consumer.truststore.jks
ssl.truststore.password=confluent
ssl.key.password=confluent
ssl.truststore.type=JKS
security.protocol=SSL
```

### producer.properties
Konfigurasi SSL/TLS di `producer.properties`
```
bootstrap.servers=node1.kafka:9092,node2.kafka:9092,node3.kafka:9092,node4.kafka:9092
ssl.truststore.location=/var/ssl/private/producer-cert/kafka.producer.truststore.jks
ssl.truststore.password=confluent
ssl.keystore.type=JKS
ssl.keystore.location=/var/ssl/private/producer-cert/kafka.producer.keystore.jks
ssl.keystore.password=confluent
ssl.key.password=confluent
security.protocol=SSL
```



## Hands on Topic

Pada hands on ini tim kami melakukan beberapa hal pada topik yaitu:
- create topic
- delete topic
- produce topic
- consume topic
- describe topic
- list topic

### create dan delete topic
Pada tahap ini tim kami melakukan create dan delete topic menggunakan command berikut:
* create 
```
kafka-topics --bootstrap-server node1.kafka:9092 --create --topic test-aji-topic --partitions 3 --replication-factor 1 --command-config /etc/kafka/client-ssl.properties
```
* delete
```
kafka-topics --bootstrap-server node1.kafka:9092 --delete --topic dito-topic --command-config /etc/kafka/client-ssl.properties

```
berikut untuk hasil dari create dan delete topic. perlu diketahui bahwa node1.kafka bisa diganti dengan node2.kafka atau broker lainnya, namun pada create dan delete ini kita hands on menggunakan node1.kafka
![alt text](https://github.com/ajiajinugroho/team2-incubation/blob/main/SSL-Deployment/IMG/1.%20Create-Delete%20Topic.png?raw=true)
(link gambar)

### produce dan consume topic
Pada tahap ini tim kami melakukan produce dan consume topic menggunakan command berikut:
* produce
```
kafka-console-producer --broker-list node1.kafka:9092 --topic ssl-topic --producer.config /etc/kafka/client-ssl.properties
```

* consume
```
kafka-console-consumer --bootstrap-server node1.kafka:9092 --topic ssl-topic --from-beginning --consumer.config /etc/kafka/client-ssl.properties
```

berikut hasil menjalankan command diatas untuk produce dan consume
![alt text](https://github.com/ajiajinugroho/team2-incubation/blob/main/SSL-Deployment/IMG/2.%20Produce-Consume%20SSL.png?raw=true)
(link gambar)

### describe dan list topic
Pada tahap ini tim kami melakukan describe dan list topic menggunakan command berikut:
* describe
```
kafka-topics --bootstrap-server node1.kafka:9092 --describe --topic ssl-topic --command-config /etc/kafka/client-ssl.properties
```



* list
```
kafka-topics --bootstrap-server node1.kafka:9092 --list --command-config /etc/kafka/client-ssl.properties
```
![alt text](https://github.com/ajiajinugroho/team2-incubation/blob/main/SSL-Deployment/IMG/3.%20Describe-List%20Topic.png?raw=true)
(link gambar)
