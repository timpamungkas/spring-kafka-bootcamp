# Kafka User Interface

  * [Create Topics](#create-topics)

----

## Create Topics

{::options parse_block_html="true" /}

<details><summary markdown="span">Click to expand!</summary>
 
Go to kafka console (terminal).  
If you are using docker, type `docker exec -it [kafka-container-name] bash`  
For container name, you can get it by typing `docker ps`    
 
```bash
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 3 --replication-factor 1 --topic t-ui-dummy-01
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 4 --replication-factor 1 --topic t-ui-dummy-02
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 5 --replication-factor 1 --topic t-ui-dummy-03
```
</details>
<br/>

{::options parse_block_html="false" /}

----

[Back to index](/spring-kafka-bootcamp)
