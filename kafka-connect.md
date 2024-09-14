# Kafka Connect
  * [Docker Compose](#docker-compose)
  * [Kafka Console Scripts](#kafka-console-scripts)
  * [Postgresql Scripts - Change Data Capture](#postgresql-scripts---change-data-capture)

----

## Docker Compose

{::options parse_block_html="true" /}

<details><summary markdown="span">Click to expand!</summary>

```bash
# Restart Kafka Connect (after installing connector)
docker-compose -f docker-compose-connect.yml -p connect restart kafka-connect
```
</details>
<br/>

{::options parse_block_html="false" /}

----

## Kafka Console Scripts

{::options parse_block_html="true" /}

<details><summary markdown="span">Click to expand!</summary>
 
Go to kafka console (terminal).  
If you are using docker, type `docker exec -it [kafka-container-name] bash`  
For container name, you can get it by typing `docker ps`    
 
```bash
# CSV Employee
kafka-console-consumer.sh --bootstrap-server localhost:9092 --from-beginning --topic t-spooldir-csv-demo
kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list
kafka-consumer-groups.sh --bootstrap-server localhost:9092 --group [group-id] --describe


# Change Data Capture - Legacy Modernization
kafka-topics.sh --bootstrap-server localhost:9092 --list
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic t-cdc-finance.public.fin_invoices --from-beginning
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic t-cdc-marketing.public.mkt_promotions --from-beginning
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic t-cdc-marketing.public.mkt_sales --from-beginning


# Data Engineering
kafka-console-consumer.sh --bootstrap-server localhost:9092 --from-beginning --topic t-person-address-postgresql
kafka-console-consumer.sh --bootstrap-server localhost:9092 --from-beginning --topic t-person-address-http
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-person-address-custom
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic t-person-address-custom


# Kafka Stream & Kafka Connect
kafka-console-consumer.sh --bootstrap-server localhost:9092 --from-beginning --topic t-person-address-postgresql --property print.key=true
kafka-console-consumer.sh --bootstrap-server localhost:9092 --from-beginning --topic t-person-address-http --property print.key=true
kafka-console-consumer.sh --bootstrap-server localhost:9092 --from-beginning --topic t-person-address-custom --property print.key=true
kafka-console-consumer.sh --bootstrap-server localhost:9092 --from-beginning --topic t-person-address-target --property print.key=true
```
</details>
<br/>

{::options parse_block_html="false" /}

----

## Postgresql Scripts - Change Data Capture

{::options parse_block_html="true" /}

<details><summary markdown="span">Click to expand!</summary>

- DDL (create table) scripts available as part of docker-compose, on folder `postgresql/docker-entrypoint-initdb.d`
- To alter replica identity, use these statements

```sql
ALTER TABLE public.fin_invoices REPLICA IDENTITY FULL;
ALTER TABLE public.mkt_promotions  REPLICA IDENTITY FULL;
ALTER TABLE public.mkt_sales REPLICA IDENTITY FULL;
```
- To create `kafka_fin_invoices`, use these statements

```sql
DROP TABLE IF EXISTS kafka_fin_invoices ;

CREATE TABLE IF NOT EXISTS kafka_fin_invoices (
	invoice_id INT PRIMARY KEY,
	invoice_amount INT,
	invoice_currency VARCHAR(3),
	invoice_number VARCHAR(50),
	invoice_date DATE
);
```
</details>
<br/>

{::options parse_block_html="false" /}

----

## Data Engineering

{::options parse_block_html="true" /}

<details><summary markdown="span">Click to expand!</summary>

- DDL (create table) scripts available as part of docker-compose, on folder `postgresql/docker-entrypoint-initdb.d`
- To alter replica identity, use these statements

```sql
ALTER TABLE public.fin_invoices REPLICA IDENTITY FULL;
ALTER TABLE public.mkt_promotions  REPLICA IDENTITY FULL;
ALTER TABLE public.mkt_sales REPLICA IDENTITY FULL;
```
- To create `kafka_fin_invoices`, use these statements

```sql
DROP TABLE IF EXISTS kafka_fin_invoices ;

CREATE TABLE IF NOT EXISTS kafka_fin_invoices (
	invoice_id INT PRIMARY KEY,
	invoice_amount INT,
	invoice_currency VARCHAR(3),
	invoice_number VARCHAR(50),
	invoice_date DATE
);
```
</details>
<br/>

{::options parse_block_html="false" /}

----

[Back to index](/spring-kafka-bootcamp)
