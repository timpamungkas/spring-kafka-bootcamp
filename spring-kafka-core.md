# Spring Kafka Core

  * [Kafka Console Scripts](#kafka-console-scripts)
  * [Kafka Configuration Reference](#kafka-configuration-reference)

----

## Kafka Console Scripts

{::options parse_block_html="true" /}

<details><summary markdown="span">Click to expand!</summary>
 
Go to kafka console (terminal).  
If you are using docker, type `docker exec -it [kafka-container-name] bash`  
For container name, you can get it by typing `docker ps`    
 
```bash
# Hello Kafka
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-hello


# Consumer is Real Time
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-fixedrate
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-fixedrate-2


# Producing Message With Key
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 3 --replication-factor 1 --topic t-multi-partitions
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic t-multi-partitions --offset earliest --partition 0
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic t-multi-partitions --offset earliest --partition 1
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic t-multi-partitions --offset earliest --partition 2


# Multiple Consumers
kafka-topics.sh --bootstrap-server localhost:9092 --alter --topic t-multi-partitions --partitions 4


# Producing JSON
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-employee
kafka-console-consumer.sh --bootstrap-server localhost:9092 --offset earliest --partition 0 --topic t-employee


# Customize JSON
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-employee-2
kafka-console-consumer.sh --bootstrap-server localhost:9092 --offset earliest --partition 0 --topic t-employee-2


# Consuming with Consumer Group
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity
kafka-console-consumer.sh --bootstrap-server localhost:9092 --offset earliest --partition 0 --topic t-commodity
kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group cg-dashboard --describe
kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group cg-dashboard --execute --reset-offsets --to-offset 10 --topic t-commodity:0


# Handling Consumer Offset
kafka-console-consumer.sh --from-beginning --bootstrap-server localhost:9092 --property print.key=false --property print.value=false --topic t-counter --timeout-ms 5000 | tail -n 10|grep "Processed a total of"
kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group counter-group-fast --describe
kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group counter-group-slow --describe


# Rebalancing
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-rebalance
kafka-topics.sh --bootstrap-server localhost:9092 --alter --topic t-rebalance --partitions 2
kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic t-rebalance
kafka-topics.sh --bootstrap-server localhost:9092 --alter --topic t-rebalance --partitions 3


# Message Filter
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-location


# Idempotency
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-purchase-request
kafka-console-consumer.sh --bootstrap-server localhost:9092 --offset earliest --partition 0 --topic t-purchase-request

  
# Idempotency - Alternative
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-payment-request
kafka-console-consumer.sh --bootstrap-server localhost:9092 --offset earliest --partition 0 --topic t-payment-request

  
# Handling Exception on @KafkaListener
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-food-order
  
  
# Handling Exception - Global Error Handler
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-simple-number

  
# Retrying Consumer
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 2 --replication-factor 1 --topic t-image

  
# Handling Exception - Dead Letter Topic
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 2 --replication-factor 1 --topic t-invoice
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 2 --replication-factor 1 --topic t-invoice-dead
kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic t-invoice
kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic t-invoice-dead
  

# Non Blocking Retry
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 2 --replication-factor 1 --topic t-image-2
 
  
# Scheduling Consumer
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-general-ledger
kafka-console-consumer.sh --bootstrap-server localhost:9092 --from-beginning --topic t-general-ledger


# Order App - Test The App
kafka-console-consumer.sh --bootstrap-server localhost:9092 --from-beginning --topic t-commodity-order


# Order App - Promotion Publisher
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-promotion
kafka-console-consumer.sh --bootstrap-server localhost:9092 --from-beginning --topic t-commodity-promotion
```
</details>
<br/>

{::options parse_block_html="false" /}

----

## Kafka Configuration Reference
  - [Producer configuration](https://kafka.apache.org/documentation/#producerconfigs)
  - [Consumer configuration](https://kafka.apache.org/documentation/#consumerconfigs)

----

[Back to index](/spring-kafka-bootcamp)
