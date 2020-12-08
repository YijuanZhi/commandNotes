# Kafka Learning Note
[Reference](https://www.tutorialspoint.com/apache_kafka/index.htm)

## Running Zookeeper & Kafka on local machine
1. `bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic kafka_example --from-beginning`
   - start the consumer, accepting topic named kafka_example
2. `bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic kafka_example`
   - creating the topic name kafka_example
3. `bin/kafka-server-start.sh config/server.properties`
   - start kafka server, default port is 9092
4. `bin/zookeeper-server-start.sh config/zookeeper.properties`
   - start zookeeper server, default port is 2181

## Kafka Concepts
1. [Consumer Group](https://dev.to/de_maric/what-is-a-consumer-group-in-kafka-49i)
   - *A consumer group is a group of consumers (I guess you didn’t see this coming?) that share the same group id. When a topic is consumed by consumers in the same group, every record will be delivered to only one consumer. As the official documentation states: “If all the consumer instances have the same consumer group, then the records will effectively be load-balanced over the consumer instances.” This way you can ensure parallel processing of records from a topic and be sure that your consumers won’t be stepping on each other toes.*


## Kafka Architecture
1. [kafka message pattern](https://www.tutorialspoint.com/apache_kafka/apache_kafka_fundamentals.htm)
2. [kafka cluster Architecture](https://www.tutorialspoint.com/apache_kafka/apache_kafka_cluster_architecture.htm)