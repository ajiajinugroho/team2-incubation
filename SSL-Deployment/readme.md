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
zookeeper.ssl.truststore.location=/var/ssl/private/script/kafka.node2.zookeeper.truststore.jks
zookeeper.ssl.truststore.password=confluent
zookeeper.ssl.keystore.location=/var/ssl/private/script/kafka.node2.zookeeper.keystore.jks
zookeeper.ssl.keystore.password=confluent
ssl.key.password=confluent
ssl.truststore.location=/var/ssl/private/script/kafka.node2.kafka.truststore.jks
ssl.truststore.password=confluent
ssl.keystore.location=/var/ssl/private/script/kafka.node2.kafka.keystore.jks
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
ssl.keystore.location=/var/ssl/private/script/kafka.node2.kafka.keystore.jks
ssl.keystore.password=confluent
ssl.truststore.location=/var/ssl/private/script/kafka.node2.kafka.truststore.jks
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
ssl.keyStore.location=/var/ssl/private/script/kafka.node2.zookeeper.keystore.jks
ssl.keyStore.password=confluent
ssl.trustStore.location=/var/ssl/private/script/kafka.node2.zookeeper.truststore.jks
ssl.trustStore.password=confluent
syncLimit=2
```


### zk-client.properties
Konfigurasi SSL/TLS di `zk-client.properties`
```
zookeeper.clientCnxnSocket=org.apache.zookeeper.ClientCnxnSocketNetty
zookeeper.ssl.client.enable=true
zookeeper.ssl.protocol=TLSv1.2
zookeeper.ssl.truststore.location=/var/ssl/private/script/kafka.node2.zookeeper.truststore.jks
zookeeper.ssl.truststore.password=confluent
zookeeper.ssl.keystore.location=/var/ssl/private/script/kafka.node2.zookeeper.keystore.jks
zookeeper.ssl.keystore.password=confluent
ssl.key.password=confluent
```


### consumer.properties
Konfigurasi SSL/TLS di `consumer.properties`
```
bootstrap.servers=node1.kafka:9092,node2.kafka:9092,node3.kafka:9092,node4.kafka:9092
group.id=test-consumer-group
ssl.keystore.location=/var/ssl/private/script/kafka.node2.kafka.keystore.jks
ssl.keystore.password=confluent
ssl.keystore.type=JKS
ssl.truststore.location=/var/ssl/private/script/kafka.node2.kafka.truststore.jks
ssl.truststore.password=confluent
ssl.key.password=confluent
ssl.truststore.type=JKS
security.protocol=SSL
```

### producer.properties
Konfigurasi SSL/TLS di `producer.properties`
```
bootstrap.servers=node1.kafka:9092,node2.kafka:9092,node3.kafka:9092,node4.kafka:9092
ssl.truststore.location=/var/ssl/private/script/kafka.node2.kafka.truststore.jks
ssl.truststore.password=confluent
ssl.keystore.type=JKS
ssl.keystore.location=/var/ssl/private/script/kafka.node2.kafka.keystore.jks
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


# SASL_SSL

## Konfigurasi yang dilakukan untuk menerapkan keamanan SASL_SSL

### Konfigurasi server.properties
```
#Mengubah
listeners=SASL_SSL://:9092
advertised.listeners=SASL_SSL://node2.kafka:9092
listener.security.protocol.map=SASL_SSL:SASL_SSL
security.inter.broker.protocol=SASL_SSL

#Menambah
sasl.enabled.mechanisms=SCRAM-SHA-256
sasl.mechanism.inter.broker.protocol=SCRAM-SHA-256
listener.name.sasl_ssl.scram-sha-256.sasl.jaas.config=org.apache.kafka.common.security.scram.ScramLoginModule required serviceName="kafka" username="admins" password="admins";
super.user=User:admins
```

### konfigurasi producer.properties dan consumer.properties

```
#Pengubahan
security.protocol=SASL_SSL

#Tambahan 
sasl.mechanism=SCRAM-SHA-256
sasl.jaas.config=org.apache.kafka.common.security.scram.ScramLoginModule required serviceName="kafka" username="alices" password="bob";
```


### konfigurasi client-ssl.properties

```
#Mengubah
security.protocol=SASL_SSL

#Menambahkan
sasl.mechanism=SCRAM-SHA-256
sasl.jaas.config=org.apache.kafka.common.security.scram.ScramLoginModule required serviceName="kafka" username="admins" password="admins";
```

# ACLS

### membuat user alice dan bob
```
kafka-configs --bootstrap-server node1.kafka:9092 --command-config /etc/kafka/client-ssl.properties --alter --add-config 'SCRAM-SHA-256=[password=bob]' --entity-type users --entity-name alice
kafka-configs --bootstrap-server node1.kafka:9092 --command-config /etc/kafka/client-ssl.properties --alter --add-config 'SCRAM-SHA-256=[password=bob]' --entity-type users --entity-name bob
kafka-configs --bootstrap-server node1.kafka:9092 --command-config /etc/kafka/client-ssl.properties --alter --add-config 'SCRAM-SHA-256=[password=admins]' --entity-type users --entity-name admins
```

### membuat alice hanya bisa write data
```
kafka-acls --bootstrap-server node2.kafka:9092 --add --allow-principal User:alice --operation write --topic ssl-topic --command-config /etc/kafka/client-ssl.properties
```

### konfigurasi server.properties, client-ssl.properties, alice.properties,bob.properties,
```
#zookeeper.set.acl=true
authorizer.class.name=kafka.security.authorizer.AclAuthorizer
```


### produce dan consume menggunakan alice dan bob

```
kafka-console-producer --broker-list node1.kafka:9092 --topic ssl-topic --producer.config /etc/kafka/alice.properties
kafka-console-consumer --bootstrap-server node1.kafka:9092 --topic ssl-topic --from-beginning --consumer.config /etc/kafka/client-bob.properties
```

# KSQL

### konfigurasi ksql-server.properties pada /etc/ksqldb

```
listeners=http://node2.ksql:8088
advertised.listeners=https://node2.ksql:8088
bootstrap.servers=node1.kafka:9092,node2.kafka:9092,node3.kafka:9092,node4.kafka:9092

# Security settings for SSL
ssl.keystore.location=/var/ssl/private/script/kafka.node2.kafka.keystore.jks
ssl.keystore.password=confluent
ssl.key.password=confluent
ssl.truststore.location=/var/ssl/private/script/kafka.node2.kafka.truststore.jks
ssl.truststore.password=confluent

# Enable SASL/SCRAM authentication
sasl.enabled.mechanisms=SCRAM-SHA-256
security.protocol=SASL_SSL

# SASL settings
sasl.mechanism=SCRAM-SHA-256
sasl.jaas.config=org.apache.kafka.common.security.scram.ScramLoginModule required username="admins" password="admins";
```
