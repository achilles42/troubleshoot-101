## Let's Get started with zookeeper and kafka:

### using 2 separate Boxes for zookeeper and kafka

** 1. Zookeeper Box (IP: 192.168.10.100) **
  * Install Java
  * Download zookeeper: wget http://apache.org/dist/zookeeper/zookeeper-3.4.6/zookeeper-3.4.6.tar.gz
  * Untar the download file: tar -zxvf zookeeper-3.4.6.tar.gz
  * Go to zookeeper-3.4.6/conf
  * cp zoo_sample.cfg zoo.cfg
  * change dataDir path to store zookeeper all data about kafka
  * start zookeeper server:  bin/zkServer.sh start conf/zoo.cfg

** 2. Kafka Box (IP 192.168.10.101) **
  * Install java
  * Download kafka: wget http://mirror.fibergrid.in/apache/kafka/0.10.0.1/kafka_2.10-0.10.0.1.tgz
  * Untar the downloaded file: tar -zxvf kafka_2.10-0.10.0.1.tgz
  * Command to server kafka server: bin/kafka-server-start.sh config/server.properties
  * Create topic:
  ```
    root@kafka-cluster:~/kafka_2.10-0.10.0.1# bin/kafka-topics.sh --create --zookeeper 192.168.10.100:2181 --replication-factor 1 --partitions 1 --topic data
  ```
  
  * List all topics:
  ```
    root@kafka-cluster:~/kafka_2.10-0.10.0.1# bin/kafka-topics.sh --list --zookeeper 192.168.10.100:2181
  ```

  * Producer:
  ```
    root@kafka-cluster:~/kafka_2.10-0.10.0.1# bin/kafka-console-producer.sh --broker-list 192.168.10.101:9092 --topic data
  ```  

  * Consumer:
  ```
    root@kafka-cluster:~/kafka_2.10-0.10.0.1# bin/kafka-console-consumer.sh --zookeeper 192.168.10.100:2181 --topic data --from-beginning
  ```  
