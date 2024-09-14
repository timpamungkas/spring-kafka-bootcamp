# KSQLDB

  * [How to Start](#how-to-start)
  * [ksqlDB Syntax Reference](#ksqldb-syntax-reference)
  * [Hello ksqlDB](#hello-ksqldb)
  * [Basic Data 1](#basic-data-1)
  * [Basic Data 2](#basic-data-2)
  * [Basic Data 3](#basic-data-3)
  * [Array](#array)
  * [Map](#map)
  * [Complex Data Types](#complex-data-types)
  * [Stream & Table Key](#stream--table-key)
  * [Commodity - First Step](#commodity---first-step)
  * [Row Key](#row-key)
  * [Notes From This Point Forward](#notes-from-this-point-forward)
  * [Commodity - Additional Requirements](#commodity---additional-requirements)
  * [Commodity - Reward for Each Location](#commodity---reward-for-each-location)
  * [Run Script File](#run-script-file)
  * [Commodity - Further Fraud Processing](#commodity---further-fraud-processing)
  * [ksqldb REST API](#ksqldb-rest-api)
  * [Feedback - Are We Good Enough?](#feedback---are-we-good-enough-)
  * [Feedback - Who Owns This Feedback?](#feedback---who-owns-this-feedback-)
  * [Feedback - Good Feedback or Bad Feedback?](#feedback---good-feedback-or-bad-feedback-)
  * [Feedback - Group Using Table](#feedback---group-using-table)
  * [Feedback - Overall Good or Bad](#feedback---overall-good-or-bad)
  * [Insert Data Using Ksql](#insert-data-using-ksql)
  * [Customer  - Web & Mobile](#customer----web--mobile)
  * [Customer - Shopping Cart & Wishlist](#customer---shopping-cart--wishlist)
  * [Pull Query](#pull-query)
  * [Flash Sale](#flash-sale)
  * [Flash Sale - Timestamp](#flash-sale---timestamp)
  * [Feedback - Average Rating](#feedback---average-rating)
  * [Feedback - Detailed Rating](#feedback---detailed-rating)
  * [Inventory - Sum & Subtract Records](#inventory---sum--subtract-records)
  * [Inventory - Timestamp Extractor](#inventory---timestamp-extractor)
  * [Inventory - Tumbling Time Window](#inventory---tumbling-time-window)
  * [Inventory - Hopping Time Window](#inventory---hopping-time-window)
  * [Inner Join Stream / Stream](#inner-join-stream--stream)
  * [Left Join Stream / Stream](#left-join-stream--stream)
  * [Outer Join Stream / Stream](#outer-join-stream--stream)
  * [Inner Join Table / Table](#inner-join-table--table)
  * [Left Join Table / Table](#left-join-table--table)
  * [Outer Join Table / Table](#outer-join-table--table)
  * [Inner Join Stream / Table](#inner-join-stream--table)
  * [Left Join Stream / Table](#left-join-stream--table)
  * [Stream / Table Co-partition](#stream--table-co-partition)
  * [User Defined Function](#user-defined-function)
  * [User Defined Tabular Function](#user-defined-tabular-function)
  * [User Defined Aggregation Function](#user-defined-aggregation-function)
  * [Avro on KsqlDB](#avro-on-ksqldb)
  * [Writing Avro Schema](#writing-avro-schema)
  * [Avro Json Conversion](#avro-json-conversion)
  * [KsqlDB & Kafka Connect](#ksqldb--kafka-connect)
  * [Java Client](#java-client)

----

## How to Start
1. Stop docker using `docker-compose down` (see [here](/spring-kafka-bootcamp/docker-compose-commands))
2. Delete subfolder `data` that exists at same location with docker compose scripts
3. Start docker using `docker-compose up`, on file `docker-compose-full.yml` (see [here](/spring-kafka-bootcamp/docker-compose-commands))
4. From eclipse, run all `kafka-stream-xxx` projects, *except* `kafka-stream-sample`
5. Re-create kafka topics using [this script](/spring-kafka-bootcamp/spring-kafka-stream#create-kafka-topics) 

----

## ksqlDB Syntax Reference
  - Reference below only quick version of ksqlDB syntax which is used on the course.  
  - Some examples for quick start (more complete than reference below, but still simple enough) available [here](https://ksqldb.io/examples.html).
  - For complete example, see [ksqlDB website](https://docs.ksqldb.io/en/0.24.0-ksqldb/reference/). Note that documentation for each ksqlDB release might different. Choose the appropriate version when you see the reference on [ksqlDB website](https://docs.ksqldb.io/en/0.24.0-ksqldb/reference/).

Each ksqlDB statement must be ended with semicolon (`;`).
   
Single-line ksql comment is started with double dash (--)

```
-- This is a comment
```

  
Multi-line ksql comments is between `/* ... */`

```
/* 
Multiple
comment
lines
*/
```

### Works with kafka topic

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details><summary markdown="span">Click to expand!</summary>

**Notes**
  - Topic name is case-sensitive (`myTopic` is different with `myTOPIC`)
  - If the topic name contains non-alphanumeric characters, quote the topic name between single quotes (`'`) 
  
```bash
-- List all kafka topics
SHOW TOPICS;  

-- Show messages in kafka topic, start from message received after the command executed
-- Topic name is alphanumeric
PRINT topicNameCaseSensitive;
  
-- Topic name contains non-alphanumeric characters, put topic name between backtick  
PRINT `topic-name-with-some-dash`;

  
-- Topics below contain non-alphanumeric characters, so written between single quotes 
-- Show messages in kafka topic, start from message received after the command executed, until 500 messages only
PRINT `topic-name-with-some-dash` LIMIT 500;

-- Show all messages in kafka topic
PRINT `topic-name-with-some-dash` FROM BEGINNING;

-- Show the first 1000 messages in kafka topic
PRINT `topic-name-with-some-dash` FROM BEGINNING LIMIT 1000;
```
</details>


{::options parse_block_html="false" /}


### Describe ksqlDB objects

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details><summary markdown="span">Click to expand!</summary>
```bash
-- List all ksqlDB streams
SHOW STREAMS;  

-- List all ksqlDB tables
SHOW TABLES;  
  
-- Describe kafka stream or tables (summary)
DESCRIBE `stream-or-table-name`;

-- Describe kafka stream or tables (detail)  
DESCRIBE `stream-or-table-name` EXTENDED;
```
</details>


{::options parse_block_html="false" /}

----

## Hello ksqlDB

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details><summary markdown="span">Click to expand!</summary>
```bash
-- Set the offset to earliest
SET 'auto.offset.reset'='earliest';


-- Print data in topic
PRINT `t-commodity-promotion` 
 FROM BEGINNING;


-- Create stream from underlying topic, with JSON value
CREATE STREAM `s-commodity-promotion` (
  promotionCode VARCHAR
) WITH (
  KAFKA_TOPIC = 't-commodity-promotion',
  VALUE_FORMAT = 'JSON'
);


-- Push query from stream
SELECT * 
  FROM `s-commodity-promotion` 
EMIT CHANGES;


-- Push query with transformed data
SELECT UCASE(promotionCode) AS uppercasePromotionCode
  FROM `s-commodity-promotion` 
EMIT CHANGES;


-- Create stream from stream, publishing the transformed data into custom output topic
CREATE STREAM `s-commodity-promotion-uppercase` 
WITH (
  kafka_topic = 't-ksql-commodity-promotion-uppercase'
)
AS 
SELECT UCASE(promotionCode) AS uppercasePromotionCode
   FROM `s-commodity-promotion` 
EMIT CHANGES;

   
-- Push query from the uppercase stream
SELECT * 
  FROM `s-commodity-promotion-uppercase`
EMIT CHANGES;
```
</details>


{::options parse_block_html="false" /}


----

## Basic Data 1

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details><summary markdown="span">Click to expand!</summary>
```bash
-- Print data in topic
PRINT `t-ksql-basic-data-one` 
 FROM BEGINNING;
 

-- Create stream
CREATE STREAM `s-basic-data-one` (
  `myString` STRING,
  `myFloat` DOUBLE,
  `myBoolean` BOOLEAN,
  `myInteger` INT,
  `myDouble` DOUBLE,
  `myBigDecimal` DECIMAL(30,18),
  `myLong` BIGINT,
  `myAnotherString` VARCHAR
) 
WITH (
  KAFKA_TOPIC = 't-ksql-basic-data-one',
  VALUE_FORMAT = 'JSON'
);


-- Try to replace stream with different column order
CREATE OR REPLACE STREAM `s-basic-data-one` (
  `myBoolean` BOOLEAN,
  `myFloat` DOUBLE,
  `myDouble` DOUBLE,
  `myInteger` INT,
  `myLong` BIGINT,
  `myString` STRING,
  `myAnotherString` VARCHAR,
  `myBigDecimal` DECIMAL(30,18)
) 
WITH (
  KAFKA_TOPIC = 't-ksql-basic-data-one',
  VALUE_FORMAT = 'JSON'
);


-- Drop stream without dropping topic
DROP STREAM IF EXISTS `s-basic-data-one`;


-- Drop stream and delete underlying topic (be careful, data will lost)
DROP STREAM IF EXISTS `s-basic-data-one` 
DELETE TOPIC;


-- Set the offset to earliest
SET 'auto.offset.reset'='earliest';


-- Push query, up to 15 data only
SELECT * 
  FROM `s-basic-data-one`
EMIT CHANGES
LIMIT 15;
```
</details>


{::options parse_block_html="false" /}

----

## Basic Data 2

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details><summary markdown="span">Click to expand!</summary>
```bash
-- Print data in topic
PRINT `t-ksql-basic-data-two` 
 FROM BEGINNING;
 

-- Create stream with ksqldb date / time data type
CREATE OR REPLACE STREAM `s-basic-data-two` (
  `myEpochDay` DATE,
  `myMillisOfDay` TIME,
  `myEpochMillis` TIMESTAMP
) 
WITH (
  KAFKA_TOPIC = 't-ksql-basic-data-two',
  VALUE_FORMAT = 'JSON'
);


-- Push query
SELECT * 
  FROM `s-basic-data-two`
EMIT CHANGES;


-- Date / time functions sample
SELECT `myEpochDay`,
       DATEADD(DAYS, 7, `myEpochDay`) AS `aWeekAfterMyEpochDay`,
       `myMillisOfDay`,
       TIMESUB(HOURS, 2, `myMillisOfDay`) AS `twoHoursBeforeMyMillisOfDay`,
       `myEpochMillis`,
       FORMAT_TIMESTAMP(`myEpochMillis`, 'dd-MMM-yyyy, HH:mm:ss Z', 'Asia/Jakarta') as `epochMillisAtJakartaTimezone`
  FROM `s-basic-data-two`
EMIT CHANGES;
```
</details>


{::options parse_block_html="false" /}

----

## Basic Data 3

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details><summary markdown="span">Click to expand!</summary>
```bash
-- Print data in topic
PRINT `t-ksql-basic-data-three`;
 

-- Create stream
CREATE OR REPLACE STREAM `s-basic-data-three` (
  `myLocalDate` VARCHAR,
  `myLocalDateCustomFormat` VARCHAR,
  `myLocalTime` VARCHAR,
  `myLocalTimeCustomFormat` VARCHAR,
  `myLocalDateTime` VARCHAR,
  `myLocalDateTimeCustomFormat` VARCHAR
) 
WITH (
  KAFKA_TOPIC = 't-ksql-basic-data-three',
  VALUE_FORMAT = 'JSON'
);


-- Push query
SELECT * 
  FROM `s-basic-data-three`
EMIT CHANGES;


-- Test query (LocalDate)
SELECT `myLocalDate`,
       DATEADD(DAYS, 7, `myLocalDate`) AS `aWeekAfterMyLocalDate`,
       CONCAT('Prefix string- ', `myLocalDate`, ' -suffix String') AS `myLocalDateConcatString`,
       `myLocalDateCustomFormat`,
       DATEADD(DAYS, 7, `myLocalDateCustomFormat`) AS `aWeekAfterMyLocalDateCustomFormat`,
       CONCAT('Prefix string- ', `myLocalDateCustomFormat`, ' -suffix String') AS `myLocalDateCustomFormatConcatString`
  FROM `s-basic-data-three`
EMIT CHANGES;


-- Test query (LocalTime)
SELECT `myLocalTime`,
       TIMEADD(HOURS, 3, `myLocalTime`) AS `3HoursAfterMyLocalTime`,
       CONCAT('Prefix string- ', `myLocalTime`, ' -suffix String') AS `myLocalTimeConcatString`,
       `myLocalTimeCustomFormat`,
       TIMEADD(HOURS, 3, `myLocalTimeCustomFormat`) AS `3HoursAfterMyLocalDateCustomFormat`,
       CONCAT('Prefix string- ', `myLocalTimeCustomFormat`, ' -suffix String') AS `myLocalTimeCustomFormatConcatString`
  FROM `s-basic-data-three`
EMIT CHANGES;


-- Test query (LocalDateTime)
SELECT `myLocalDateTime`,
       DATEADD(DAYS, 2, `myLocalDateTime`) AS `2DaysAfterMyLocalDateTime`,
       CONCAT('Prefix string- ', `myLocalDateTime`, ' -suffix String') AS `myLocalDateTimeConcatString`,
       `myLocalDateTimeCustomFormat`,
       DATEADD(DAYS, 2, `myLocalDateTimeCustomFormat`) AS `2DaysAfterMyLocalDateTimeCustomFormat`,
       CONCAT('Prefix string- ', `myLocalDateTimeCustomFormat`, ' -suffix String') AS `myLocalDateTimeCustomFormatConcatString`
  FROM `s-basic-data-three`
EMIT CHANGES;


-- Parse date / time from string
SELECT PARSE_DATE(`myLocalDate`, 'yyyy-MM-dd') AS `parsedLocalDate`,
		PARSE_DATE(`myLocalDateCustomFormat`, 'dd MMM yyyy') AS `parsedLocalDateCustomFormat`,
		PARSE_TIME(`myLocalTime`, 'HH:mm:ss') AS `parsedLocalTime`,
		PARSE_TIME(`myLocalTimeCustomFormat`, 'hh:mm:ss a') AS `parsedLocalTimeCustomFormat`,
		PARSE_TIMESTAMP(`myLocalDateTime`, 'yyyy-MM-dd''T''HH:mm:ss') AS `parsedLocalDateTime`,
		PARSE_TIMESTAMP(`myLocalDateTimeCustomFormat`, 'dd-MMM-yyyy hh:mm:ss a') AS `parsedLocalDateTimeCustomFormat`
  FROM `s-basic-data-three`
EMIT CHANGES;


-- Create new stream for parsed date / time
CREATE STREAM `s-basic-data-three-parsed`
AS
SELECT PARSE_DATE(`myLocalDate`, 'yyyy-MM-dd') AS `parsedLocalDate`,
		PARSE_DATE(`myLocalDateCustomFormat`, 'dd MMM yyyy') AS `parsedLocalDateCustomFormat`,
		PARSE_TIME(`myLocalTime`, 'HH:mm:ss') AS `parsedLocalTime`,
		PARSE_TIME(`myLocalTimeCustomFormat`, 'hh:mm:ss a') AS `parsedLocalTimeCustomFormat`,
		PARSE_TIMESTAMP(`myLocalDateTime`, 'yyyy-MM-dd''T''HH:mm:ss') AS `parsedLocalDateTime`,
		PARSE_TIMESTAMP(`myLocalDateTimeCustomFormat`, 'dd-MMM-yyyy hh:mm:ss a') AS `parsedLocalDateTimeCustomFormat`
  FROM `s-basic-data-three`
EMIT CHANGES;


-- Describe stream
DESCRIBE `s-basic-data-three`;
DESCRIBE `s-basic-data-three-parsed`;
DESCRIBE `s-basic-data-three` EXTENDED;
DESCRIBE `s-basic-data-three-parsed` EXTENDED;


-- Test query from parsed stream (LocalDate)
SELECT `parsedLocalDate`,
       DATEADD(DAYS, 7, `parsedLocalDate`) AS `aWeekAfterParsedLocalDate`,
       `parsedLocalDateCustomFormat`,
       DATEADD(DAYS, 7, `parsedLocalDateCustomFormat`) AS `aWeekAfterParsedLocalDateCustomFormat`
  FROM `s-basic-data-three-parsed`
EMIT CHANGES;


-- Test query from parsed stream (LocalTime)
SELECT `parsedLocalTime`,
       TIMEADD(HOURS, 3, `parsedLocalTime`) AS `3HoursAfterParsedLocalTime`,
       `parsedLocalTimeCustomFormat`,
       TIMEADD(HOURS, 3, `parsedLocalTimeCustomFormat`) AS `3HoursAfterParsedLocalDateCustomFormat`
  FROM `s-basic-data-three-parsed`
EMIT CHANGES;


-- Test query from parsed stream (LocalDateTime)
SELECT `parsedLocalDateTime`,
       TIMESTAMPADD(DAYS, 2, `parsedLocalDateTime`) AS `2DaysAfterParsedLocalDateTime`,
       `parsedLocalDateTimeCustomFormat`,
       TIMESTAMPADD(DAYS, 2, `parsedLocalDateTimeCustomFormat`) AS `2DaysAfterParsedLocalDateTimeCustomFormat`
  FROM `s-basic-data-three-parsed`
EMIT CHANGES;
```
</details>


{::options parse_block_html="false" /}


----

## Array

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details><summary markdown="span">Click to expand!</summary>
```bash
-- Print data in topic
PRINT `t-ksql-basic-data-four`;


-- Create stream which contains array / list / set
CREATE STREAM `s-basic-data-four` (
  `myStringArray` ARRAY<VARCHAR>,
  `myIntegerList` ARRAY<INT>,
  `myDoubleSet` ARRAY<DOUBLE>
) WITH (
  KAFKA_TOPIC = 't-ksql-basic-data-four',
  VALUE_FORMAT = 'JSON'
);


-- Push query
SELECT *
  FROM `s-basic-data-four`
EMIT CHANGES;


-- Describe stream
DESCRIBE `s-basic-data-four`;


-- Test array functions
SELECT ARRAY_LENGTH(`myStringArray`) as `lengthMyStringArray`,
       ARRAY_CONCAT(`myIntegerList`, ARRAY[999, 998, 997]) as `concatMyIntegerList`,
       ARRAY_SORT(`myDoubleSet`, 'DESC') as `sortedDescMyDoubleSet`
  FROM `s-basic-data-four`
EMIT CHANGES;
```
</details>


{::options parse_block_html="false" /}

----

## Map

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details><summary markdown="span">Click to expand!</summary>
```bash
-- Print data in topic
PRINT `t-ksql-basic-data-five`;
 

-- Create stream which contains array / list / set
CREATE STREAM `s-basic-data-five` (
  `myMapAlpha` MAP<VARCHAR, VARCHAR>,
  `myMapBeta` MAP<VARCHAR, VARCHAR>
) WITH (
  KAFKA_TOPIC = 't-ksql-basic-data-five',
  VALUE_FORMAT = 'JSON'
);


-- Push query
SELECT *
  FROM `s-basic-data-five`
EMIT CHANGES;


-- Describe stream
DESCRIBE `s-basic-data-five`;


-- Test map functions
SELECT MAP_VALUES(`myMapAlpha`) as `valuesAtMyMapAlpha`,
       MAP_KEYS(`myMapBeta`) as `keysAtMyMapBeta`
  FROM `s-basic-data-five`
EMIT CHANGES;
```
</details>


{::options parse_block_html="false" /}

----

## Complex Data Types

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details><summary markdown="span">Click to expand!</summary>
```bash
-- Print data in topic
PRINT `t-ksql-basic-data-person` 
 FROM BEGINNING;
 

-- Create stream which contains complex data types
CREATE STREAM `s-basic-data-person` (
  `firstName` VARCHAR,
  `lastName` VARCHAR,
  `birthDate` VARCHAR,
  `contacts` MAP<VARCHAR, VARCHAR>,
  `passport` STRUCT<
  			     `number` VARCHAR,
  				  `expiryDate` VARCHAR
  				>,
  `addresses` ARRAY<
  				  STRUCT<
  				    `streetAddress` VARCHAR,
  				    `country` VARCHAR,
  				    `location` STRUCT<
  				      `latitude` DOUBLE,
  				      `longitude` DOUBLE
  				    >
  				  >
  				>
) WITH (
  KAFKA_TOPIC = 't-ksql-basic-data-person',
  VALUE_FORMAT = 'JSON'
);


-- Push query
SELECT *
  FROM `s-basic-data-person`
EMIT CHANGES;


-- Accessing contact (MAP) by key
SELECT `contacts`['email'] AS `emailFromContactsMap`,
		`contacts`['phoneHome'] AS `phoneHomeFromContactsMap`,
		`contacts`['phoneWork'] AS `phoneWorkFromContactsMap`
  FROM `s-basic-data-person`
EMIT CHANGES;


-- Accessing passport (STRUCT) by field name
SELECT `passport`->`number` AS `passportNumber`,
		`passport`->`expiryDate` AS `passportExpiryDate`
  FROM `s-basic-data-person`
EMIT CHANGES;


-- Convert each element in list addresses into one record
SELECT `firstName`, `lastName`, 
       EXPLODE(`addresses`) as `addressSingle`
  FROM `s-basic-data-person`
EMIT CHANGES;


-- Convert each element in ARRAY addresses into one record, then access each field in address 
SELECT `firstName`, `lastName`, 
       EXPLODE(`addresses`)->`streetAddress`,
       EXPLODE(`addresses`)->`country`,
       EXPLODE(`addresses`)->`location`
  FROM `s-basic-data-person`
EMIT CHANGES;


-- Convert each element in ARRAY addresses into one record, then access each field in address 
-- including latitude / longitude which actually field at STRUCT within STRUCT
SELECT `firstName`, `lastName`, 
       EXPLODE(`addresses`)->`streetAddress`,
       EXPLODE(`addresses`)->`country`,
       EXPLODE(`addresses`)->`location`->`latitude` AS `latitude`,
       EXPLODE(`addresses`)->`location`->`longitude` AS `longitude`
  FROM `s-basic-data-person`
EMIT CHANGES;


-- Create stream which each field at STRUCT converted as one column, and ARRAY exploded so each 
-- element at ARRAY becomes one row. Also, date string converted to DATE data type
CREATE STREAM `s-basic-data-person-complete`
AS
SELECT `firstName`, 
		`lastName`, 
		PARSE_DATE(`birthDate`, 'yyyy-MM-dd') AS `birthDate`,
		`contacts`,
		`passport`->`number` AS `passportNumber`,
		PARSE_DATE(`passport`->`expiryDate`,'yyyy-MM-dd') AS `passportExpiryDate`,		
       EXPLODE(`addresses`)->`streetAddress`,
       EXPLODE(`addresses`)->`country`,
       EXPLODE(`addresses`)->`location`->`latitude` AS `latitude`,
       EXPLODE(`addresses`)->`location`->`longitude` AS `longitude`
  FROM `s-basic-data-person`
EMIT CHANGES;


-- Describe the complete stream
DESCRIBE `s-basic-data-person-complete`;


-- Push query from complete stream
SELECT *
  FROM `s-basic-data-person-complete`
EMIT CHANGES;
```
</details>


{::options parse_block_html="false" /}

----

## Stream & Table Key

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details><summary markdown="span">Click to expand!</summary>

```bash
-- Print country topic
PRINT `t-ksql-basic-data-country`
FROM BEGINNING;


-- Create stream country
CREATE STREAM `s-basic-data-country` (
  `countryName` VARCHAR,
  `currencyCode` VARCHAR,
  `population` INT
) WITH (
  KAFKA_TOPIC = 't-ksql-basic-data-country',
  VALUE_FORMAT = 'JSON'
);


-- Describe stream
DESCRIBE `s-basic-data-country`;


-- Show data from stream country
SET 'auto.offset.reset'='earliest';

SELECT * 
  FROM `s-basic-data-country`
EMIT CHANGES;


-- Re-key by country name, create new stream
DROP STREAM IF EXISTS `s-basic-data-country-rekeyed`;

CREATE STREAM `s-basic-data-country-rekeyed`
AS
SELECT `countryName`, `currencyCode`, `population` 
  FROM `s-basic-data-country`
PARTITION BY `countryName`
EMIT CHANGES;


-- Describe rekeyed stream
DESCRIBE `s-basic-data-country-rekeyed`;


-- Re-key by country name, create new stream, preserve value
DROP STREAM IF EXISTS `s-basic-data-country-rekeyed`;

CREATE STREAM `s-basic-data-country-rekeyed`
AS
SELECT `countryName` AS `rowkey`, AS_VALUE(`countryName`) AS `countryName`, `currencyCode`, `population` 
  FROM `s-basic-data-country`
PARTITION BY `countryName`
EMIT CHANGES;


-- Show data from stream country (rekeyed)
SET 'auto.offset.reset'='earliest';

SELECT * 
  FROM `s-basic-data-country-rekeyed`
EMIT CHANGES;


-- Re-key by country name & currency code (JSON), create new stream
DROP STREAM IF EXISTS `s-basic-data-country-rekeyed-json`;

CREATE STREAM `s-basic-data-country-rekeyed-json`
WITH (
  KEY_FORMAT = 'JSON'
)
AS
SELECT STRUCT(`countryName` := `countryName`, `currencyCode` := `currencyCode`) AS `jsonKey`,
       AS_VALUE(`countryName`) AS `countryName`,
       AS_VALUE(`currencyCode`) AS `currencyCode`,
       `population`
  FROM `s-basic-data-country`
PARTITION BY STRUCT(`countryName` := `countryName`, `currencyCode` := `currencyCode`)
EMIT CHANGES;


-- Show data from stream country (rekeyed)
SET 'auto.offset.reset'='earliest';

SELECT * 
  FROM `s-basic-data-country-rekeyed-json`
EMIT CHANGES;


-- Create table where key is country name and summarize population
DROP TABLE IF EXISTS `tbl-basic-data-country`;

CREATE TABLE `tbl-basic-data-country`
AS
SELECT `countryName`, SUM(`population`) AS `totalPopulation`
  FROM `s-basic-data-country`
GROUP BY `countryName`
EMIT CHANGES;


-- Show data from table country
SET 'auto.offset.reset'='earliest';

SELECT * 
  FROM `tbl-basic-data-country`
EMIT CHANGES;
```
</details>


{::options parse_block_html="false" /}
	
----
 
## Commodity - First Step

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details><summary markdown="span">Click to expand!</summary>
```bash
-- Create base commodity stream
CREATE STREAM `s-commodity-order` (
  `rowkey` VARCHAR KEY,
  `creditCardNumber` VARCHAR,
  `itemName` VARCHAR,
  `orderDateTime` VARCHAR,
  `orderLocation` VARCHAR,
  `orderNumber` VARCHAR,
  `price` INT,
  `quantity` INT
) WITH (
  KAFKA_TOPIC = 't-commodity-order',
  VALUE_FORMAT = 'JSON'
);

-- Describe stream
DESCRIBE `s-commodity-order`;

-- Push query
SELECT * 
  FROM `s-commodity-order`
EMIT CHANGES;


-- Mask credit card number
CREATE STREAM `s-commodity-order-masked`
AS
SELECT `rowkey`, MASK_LEFT(`creditCardNumber`, 12, '*', '*', '*', '*') AS `maskedCreditCardNumber`, 
       `itemName`, `orderDateTime`, `orderLocation`, `orderNumber`, `price`, `quantity`
  FROM `s-commodity-order`
EMIT CHANGES;


-- Push query
SELECT * 
  FROM `s-commodity-order-masked`
EMIT CHANGES;


-- Calculate total item amount to pattern output
CREATE STREAM `s-commodity-pattern-one`
AS
SELECT `rowkey`, `itemName`, `orderDateTime`, `orderLocation`, `orderNumber`, 
		 (`price` * `quantity`) as `totalItemAmount`
  FROM `s-commodity-order-masked`
EMIT CHANGES;


-- Push query from pattern
SELECT *
  FROM `s-commodity-pattern-one`
EMIT CHANGES;


-- Filter only "large" quantity to reward output
CREATE STREAM `s-commodity-reward-one`
AS
SELECT `rowkey`, `itemName`, `orderDateTime`, `orderLocation`,
		 `orderNumber`, `price`, `quantity`
  FROM `s-commodity-order-masked`
 WHERE `quantity` > 200
EMIT CHANGES;


-- Push query from reward
SELECT *
  FROM `s-commodity-reward-one`
EMIT CHANGES;


-- Storage sink is just as is
CREATE STREAM `s-commodity-storage-one`
AS
SELECT *
  FROM `s-commodity-order-masked`
EMIT CHANGES;


-- Push query from storage
SELECT *
  FROM `s-commodity-reward-one`
EMIT CHANGES;
```
</details>


{::options parse_block_html="false" /}

----

## Row Key
	
{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details><summary markdown="span">Click to expand!</summary>

```bash
-- Create stream with key from kafka record value
CREATE STREAM `s-commodity-order-key-from-value` (
  `creditCardNumber` VARCHAR,
  `itemName` VARCHAR,
  `orderDateTime` VARCHAR,
  `orderLocation` VARCHAR,
  `orderNumber` VARCHAR KEY,
  `price` INT,
  `quantity` INT
) WITH (
  KAFKA_TOPIC = 't-commodity-order',
  VALUE_FORMAT = 'JSON'
);


-- Describe stream
DESCRIBE `s-commodity-order-key-from-value`;
```
</details>

{::options parse_block_html="false" /}
	
----

## Notes From This Point Forward
From this point forward, the regular push query:

```bash
SELECT *
  FROM `s-my-stream`
EMIT CHANGES;
```

and describe:

```bash
DESCRIBE `s-my-stream`;
```

Will be ommited from statement reference. You can simply change `s-my-stream` above to push query / describe.
  
On the create statement, I might use `CREATE OR REPLACE` or `CREATE s-my-stream IF NOT EXISTS`, but they are optional.

----

## Commodity - Additional Requirements

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details><summary markdown="span">Click to expand!</summary>

```bash
-- Create stream for plastic items
CREATE OR REPLACE STREAM `s-commodity-pattern-two-plastic`
AS
SELECT `rowkey`, `itemName`, `orderDateTime`, `orderLocation`, 
		 `orderNumber`, (`price` * `quantity`) as `totalItemAmount`
  FROM `s-commodity-order-masked`
 WHERE LCASE(`itemName`) LIKE 'plastic%'
EMIT CHANGES;


-- Create stream for non-plastic items
CREATE OR REPLACE STREAM `s-commodity-pattern-two-notplastic`
AS
SELECT `rowkey`, `itemName`, `orderDateTime`, `orderLocation`, 
		 `orderNumber`, (`price` * `quantity`) as `totalItemAmount`
  FROM `s-commodity-order-masked`
 WHERE LCASE(`itemName`) NOT LIKE 'plastic%'
EMIT CHANGES;


-- Create stream for large & not cheap items
CREATE OR REPLACE STREAM `s-commodity-reward-two`
AS
SELECT `rowkey`, `itemName`, `orderDateTime`, `orderLocation`,
		 `orderNumber`, `price`, `quantity`
  FROM `s-commodity-order-masked`
 WHERE `quantity` > 200
       AND `price` > 100
EMIT CHANGES;


-- Replace key for storage 
CREATE OR REPLACE STREAM `s-commodity-storage-two`
AS
SELECT FROM_BYTES(
		     TO_BYTES(`orderNumber`, 'utf8'), 'base64'
       ) AS `base64Rowkey`, 
       `itemName`, `orderDateTime`, `orderLocation`,
  		 `orderNumber`, `price`, `quantity`
  FROM `s-commodity-order-masked`
PARTITION BY FROM_BYTES(
				       TO_BYTES(`orderNumber`, 'utf8'), 'base64'
             )
EMIT CHANGES;
  
  
-- describe stream  
DESCRIBE `s-commodity-storage-two`;
```
</details>


{::options parse_block_html="false" /}
	

----

## Commodity - Reward for Each Location

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details><summary markdown="span">Click to expand!</summary>

```bash
-- Replace key for reward
CREATE OR REPLACE STREAM `s-commodity-reward-four`
AS
SELECT `itemName`, `orderDateTime`, `orderLocation`,
		`orderNumber`, `price`, `quantity`
  FROM `s-commodity-order-masked`
PARTITION BY `orderLocation`
EMIT CHANGES;
```
</details>


{::options parse_block_html="false" /}
	
----

## Run Script File

1. Copy the content below into a file (for example `commodity-sample.ksql`. File name and extension are free.

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details><summary markdown="span">Click to expand!</summary>

```bash
-- Sample ksql script

DROP STREAM IF EXISTS `s-commodity-pattern-from-script`;
DROP STREAM IF EXISTS `s-commodity-reward-from-script`;
DROP STREAM IF EXISTS `s-commodity-storage-from-script`;
DROP STREAM IF EXISTS `s-commodity-order-from-script`;


CREATE STREAM `s-commodity-order-from-script` (
  `creditCardNumber` VARCHAR,
  `itemName` VARCHAR,
  `orderDateTime` VARCHAR,
  `orderLocation` VARCHAR,
  `orderNumber` VARCHAR KEY,
  `price` INT,
  `quantity` INT
) WITH (
  KAFKA_TOPIC = 't-commodity-order',
  VALUE_FORMAT = 'JSON'
);


CREATE OR REPLACE STREAM `s-commodity-pattern-from-script`
AS
SELECT `itemName`, `orderDateTime`, `orderLocation`, 
		 `orderNumber`, (`price` * `quantity`) as `totalItemAmount`
  FROM `s-commodity-order-from-script`
 WHERE LCASE(`itemName`) LIKE 'wooden%' 
       OR LCASE(`itemName`) LIKE 'metal%' 
EMIT CHANGES;


CREATE OR REPLACE STREAM `s-commodity-reward-from-script`
AS
SELECT `itemName`, `orderDateTime`, `orderLocation`,
		 `orderNumber`, `price`, `quantity`
  FROM `s-commodity-order-from-script`
 WHERE `quantity` >= 100
       AND `price` >= 500
EMIT CHANGES;


CREATE OR REPLACE STREAM `s-commodity-storage-from-script`
AS
SELECT `itemName`, `orderDateTime`, `orderLocation`,
		`orderNumber`, `price`, `quantity`
  FROM `s-commodity-order-from-script`
PARTITION BY `orderLocation`
EMIT CHANGES;


SET 'auto.offset.reset'='earliest';
```
</details>


{::options parse_block_html="false" /}
	
	
2. Put the file `commodity-sample.ksql` under folder `data/kafka-ksqldb/scripts` located on same folder as `docker-compose-full.yml`. The docker compose script will bind that volume into docker volume named `/data/scripts`
3. Go to ksql console
4. Execute using

```
RUN SCRIPT /data/scripts/commodity-sample.ksql;
```

----

## Commodity - Further Fraud Processing

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details><summary markdown="span">Click to expand!</summary>

```bash
CREATE OR REPLACE STREAM `s-commodity-fraud-six`
AS
SELECT CONCAT( SUBSTRING(`orderLocation`, 1, 1), '***' ) as `rowkey`,
       (`price` * `quantity`) as `totalValue`
  FROM `s-commodity-order`
 WHERE LCASE(`orderLocation`) LIKE 'c%'
PARTITION BY CONCAT( SUBSTRING(`orderLocation`, 1, 1), '***' )
EMIT CHANGES;


SELECT * 
  FROM `s-commodity-fraud-six`
EMIT CHANGES;
```
</details>


{::options parse_block_html="false" /}

----

## ksqldb REST API

  - Complete reference [here](https://docs.ksqldb.io/en/latest/developer-guide/ksqldb-rest-api/)
  - Curl statement at this reference is generated using postman on Windows, where each generated curl contain multi lines. To run properly, you might need to adjust the statement (e.g. on Windows make the curl as single line).
  - Alternatively, import the statement at postman, or just use postman collection which is downloadable from the course.
  
**Note for Push Query**
Push query response is streaming response. [Postman](https://www.postman.com/) currently does not stream response. The curl statement will display data when using command-line curl, but when you run the push query using postman, it will not show any response body.

----

## Feedback - Are We Good Enough?

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details><summary markdown="span">Click to expand!</summary>
Drop stream if exists (1)

```curl
curl --location --request POST "http://localhost:8088/ksql" \
--header "Content-Type: application/json" \
--data-raw "{
    \"ksql\": \"DROP STREAM IF EXISTS `s-commodity-feedback-one-good`;\"
}"
```

Drop stream if exists (2)

```curl
curl --location --request POST "http://localhost:8088/ksql" \
--header "Content-Type: application/json" \
--data-raw "{
    \"ksql\": \"DROP STREAM IF EXISTS `s-commodity-feedback-word`;\"
}"
```

Drop stream if exists (3)

```curl
curl --location --request POST "http://localhost:8088/ksql" \
--header "Content-Type: application/json" \
--data-raw "{
    \"ksql\": \"DROP STREAM IF EXISTS `s-commodity-feedback`;\"
}"
```

Create base stream

```curl
curl --location --request POST "http://localhost:8088/ksql" \
--header "Content-Type: application/json" \
--data-raw "{
    \"ksql\": \"CREATE OR REPLACE STREAM `s-commodity-feedback` (`feedback` VARCHAR, `feedbackDateTime` VARCHAR, `location` VARCHAR, `rating` INT) WITH (KAFKA_TOPIC = 't-commodity-feedback', VALUE_FORMAT = 'JSON');\"
}"
```

Check Base Stream

```curl
curl --location --request POST "http://localhost:8088/ksql" \
--header "Content-Type: application/json" \
--data-raw "{
    \"ksql\": \"DESCRIBE `s-commodity-feedback`;\"
}"
```

Create distinct word stream

```curl
curl --location --request POST "http://localhost:8088/ksql" \
--header "Content-Type: application/json" \
--data-raw "{
    \"ksql\": \"CREATE OR REPLACE STREAM `s-commodity-feedback-word` AS SELECT EXPLODE( ARRAY_DISTINCT( REGEXP_SPLIT_TO_ARRAY( LCASE( REGEXP_REPLACE(`feedback`, '[^a-zA-Z ]', '')), '\\s+'))) AS `word`, `feedbackDateTime`, `location`, `rating` FROM `s-commodity-feedback` EMIT CHANGES;\"
}"
```

Push query word stream (run this on command-line curl to stream the response, as postman does not support response streaming)

```curl
curl --location --request POST "http://localhost:8088/query" \
--header "Content-Type: application/json" \
--data-raw "{
    \"ksql\": \"SELECT * FROM `s-commodity-feedback-word` EMIT CHANGES;\"
}"
```

Create good word stream

```curl
curl --location --request POST "http://localhost:8088/ksql" \
--header "Content-Type: application/json" \
--data-raw "{
    \"ksql\": \"CREATE OR REPLACE STREAM `s-commodity-feedback-one-good` AS SELECT * FROM `s-commodity-feedback-word` WHERE `word` IN ('happy', 'good', 'helpful') EMIT CHANGES;\"
}"
```

Push query good word stream (run on command-line curl)

```curl
curl --location --request POST "http://localhost:8088/query" \
--header "Content-Type: application/json" \
--data-raw "{
    \"ksql\": \"SELECT * FROM `s-commodity-feedback-one-good` EMIT CHANGES;\"
}"
```

</details>

----

## Feedback - Who Owns This Feedback?

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details><summary markdown="span">Click to expand!</summary>
Create good word using location as key

```curl
curl --location --request POST "http://localhost:8088/ksql" \
--header "Content-Type: application/json" \
--data-raw "{
    \"ksql\": \"CREATE OR REPLACE STREAM `s-commodity-feedback-two-good` AS SELECT * FROM `s-commodity-feedback-word` WHERE `word` IN ('happy', 'good', 'helpful') PARTITION BY `location` EMIT CHANGES;\"
}"
```

Print stream

```curl
curl --location --request POST "http://localhost:8088/query" \
--header "Content-Type: application/json" \
--data-raw "{
    \"ksql\": \"PRINT `s-commodity-feedback-two-good`;\"
}"
```
</details>


{::options parse_block_html="false" /}
	
----

## Feedback - Good Feedback or Bad Feedback?

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details><summary markdown="span">Click to expand!</summary>
Create bad word using location as key

```curl
curl --location --request POST "http://localhost:8088/ksql" \
--header "Content-Type: application/json" \
--data-raw "{
    \"ksql\": \"CREATE OR REPLACE STREAM `s-commodity-feedback-three-bad` AS SELECT * FROM `s-commodity-feedback-word` WHERE `word` IN ('angry', 'sad', 'bad') PARTITION BY `location` EMIT CHANGES;\"
}"
```

Print bad word stream

```curl
curl --location --request POST "http://localhost:8088/query" \
--header "Content-Type: application/json" \
--data-raw "{
    \"ksql\": \"PRINT `s-commodity-feedback-three-bad`;\"
}"
```
</details>


{::options parse_block_html="false" /}

----

## Feedback - Group Using Table

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details><summary markdown="span">Click to expand!</summary>
Create good word stream

```curl
curl --location --request POST "http://localhost:8088/ksql" \
--header "Content-Type: application/json" \
--data-raw "{
    \"ksql\": \"CREATE OR REPLACE STREAM `s-commodity-feedback-four-good` AS SELECT * FROM `s-commodity-feedback-word` WHERE `word` IN ('happy', 'good', 'helpful') PARTITION BY `location` EMIT CHANGES;\"
}"
```

Create table for count good words by location

```curl
curl --location --request POST "http://localhost:8088/ksql" \
--header "Content-Type: application/json" \
--data-raw "{
    \"ksql\": \"CREATE OR REPLACE TABLE `tbl-commodity-feedback-four-good-count` AS SELECT `location`, COUNT(`word`) AS `countGoodWord` FROM `s-commodity-feedback-four-good` GROUP BY `location` EMIT CHANGES;\"
}"
```

Push query good word table

```curl
curl --location --request POST "http://localhost:8088/query" \
--header "Content-Type: application/json" \
--data-raw "{
    \"ksql\": \"SELECT * FROM `tbl-commodity-feedback-four-good-count` EMIT CHANGES;\"
}"
```

Create bad word stream

```curl
curl --location --request POST "http://localhost:8088/ksql" \
--header "Content-Type: application/json" \
--data-raw "{
    \"ksql\": \"CREATE OR REPLACE STREAM `s-commodity-feedback-four-bad` AS SELECT * FROM `s-commodity-feedback-word` WHERE `word` IN ('angry', 'sad', 'bad') PARTITION BY `location` EMIT CHANGES;\"
}"
```

Create table for count bad words by location

```curl
curl --location --request POST "http://localhost:8088/ksql" \
--header "Content-Type: application/json" \
--data-raw "{
    \"ksql\": \"CREATE OR REPLACE TABLE `tbl-commodity-feedback-four-bad-count` AS SELECT `location`, COUNT(`word`) AS `countBadWord` FROM `s-commodity-feedback-four-bad` GROUP BY `location` EMIT CHANGES;\"
}"
```

Push query bad word table

```curl
curl --location --request POST "http://localhost:8088/query" \
--header "Content-Type: application/json" \
--data-raw "{
    \"ksql\": \"SELECT * FROM `tbl-commodity-feedback-four-bad-count` EMIT CHANGES;\"
}"
```
</details>


{::options parse_block_html="false" /}

----

## Feedback - Overall Good or Bad

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details><summary markdown="span">Click to expand!</summary>

Create good word stream

```curl
curl --location --request POST "http://localhost:8088/ksql" \
--header "Content-Type: application/json" \
--data-raw "{
    \"ksql\": \"CREATE OR REPLACE STREAM `s-commodity-feedback-six-good` AS SELECT * FROM `s-commodity-feedback-word` WHERE `word` IN ('happy', 'good', 'helpful') EMIT CHANGES;\"
}"
```

Create table for count good words

```curl
curl --location --request POST "http://localhost:8088/ksql" \
--header "Content-Type: application/json" \
--data-raw "{
    \"ksql\": \"CREATE OR REPLACE TABLE `tbl-commodity-feedback-six-good-count-word` AS SELECT `word`, COUNT(`word`) AS `countGoodWord` FROM `s-commodity-feedback-six-good` GROUP BY `word` EMIT CHANGES;\"
}"
```

Push query good word table

```curl
curl --location --request POST "http://localhost:8088/query" \
--header "Content-Type: application/json" \
--data-raw "{
    \"ksql\": \"SELECT * FROM `tbl-commodity-feedback-six-good-count-word` EMIT CHANGES;\"
}"
```

Create bad word stream

```curl
curl --location --request POST "http://localhost:8088/ksql" \
--header "Content-Type: application/json" \
--data-raw "{
    \"ksql\": \"CREATE OR REPLACE STREAM `s-commodity-feedback-six-bad` AS SELECT * FROM `s-commodity-feedback-word` WHERE `word` IN ('angry', 'sad', 'bad') EMIT CHANGES;\"
}"
```

Create table for count bad words

```curl
curl --location --request POST "http://localhost:8088/ksql" \
--header "Content-Type: application/json" \
--data-raw "{
    \"ksql\": \"CREATE OR REPLACE TABLE `tbl-commodity-feedback-six-bad-count-word` AS SELECT `word`, COUNT(`word`) AS `countBadWord` FROM `s-commodity-feedback-six-bad` GROUP BY `word` EMIT CHANGES;\"
}"
```

Push query bad word table

```curl
curl --location --request POST "http://localhost:8088/query" \
--header "Content-Type: application/json" \
--data-raw "{
    \"ksql\": \"SELECT * FROM `tbl-commodity-feedback-six-bad-count` EMIT CHANGES;\"
}"
```

</details>

{::options parse_block_html="false" /}
	
----

## Insert Data Using Ksql
	
{::options parse_block_html="true" /}
	
[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash
-- inserting basic data one
INSERT INTO `s-basic-data-one` (
  `myBoolean`,
  `myString`,
  `myAnotherString`,
  `myFloat`,
  `myDouble`,
  `myBigDecimal`,
  `myInteger`,
  `myLong`
) VALUES (
  false,
  'This is a string',
  'And this is another string',
  52.918,
  58290.581047,
  4421672.5001855,
  1057,
  2900175
);


-- inserting basic data two
INSERT INTO `s-basic-data-two` (
  `myEpochDay`,
  `myMillisOfDay`,
  `myEpochMillis`
) VALUES (
  FROM_DAYS(20967),
  PARSE_TIME('18:47:15', 'HH:mm:ss'),
  FROM_UNIXTIME(1678610274295)
);


-- inserting basic data three
INSERT INTO `s-basic-data-three` (
  `myLocalDate`,
  `myLocalTime`,
  `myLocalDateTime`,
  `myLocalDateCustomFormat`,
  `myLocalTimeCustomFormat`,
  `myLocalDateTimeCustomFormat`
) VALUES (
  '2024-03-07',
  '16:52:09',
  '2028-11-26T19:44:16',
  '27 Aug 2024',
  '02:55:17 PM',
  '19-Dec-2026 05:42:53 AM'
);


-- inserting basic data four (array of string)
INSERT INTO `s-basic-data-four` (
  `myStringArray`
) VALUES (
  ARRAY[
    'Hello', 
    'from', 
    'ksqldb', 
    'I hope you like it', 
    'and enjoy the course'
  ]
);


-- inserting basic data four (list of integer)
INSERT INTO `s-basic-data-four` (
  `myIntegerList`
) VALUES (
  ARRAY[
    1001, 1002, 1003, 1004, 1005, 1006
  ]
);


-- inserting basic data four (set of double)
INSERT INTO `s-basic-data-four` (
  `myDoubleSet`
) VALUES (
  ARRAY[
    582.59, 1964.094, 287.296, 7933.04, 332.694
  ]
);


-- inserting basic data five (1)
INSERT INTO `s-basic-data-five` (
  `myMapAlpha`
) VALUES (
  MAP(
    '973' := 'nine seven three',
    '628' := 'six two eight',
    '510' := 'five one zero'
  )
);


-- inserting basic data five (2)
INSERT INTO `s-basic-data-five` (
  `myMapAlpha`,
  `myMapBeta`  
) VALUES (
  MAP(
    '409' := 'four zero nine',
    '152' := 'one five two',
    '736' := 'seven three six',
    '827' := 'eight two seven'    
  ),
  MAP(
  'd2c1b963-c18c-4c6e-b85f-3ebc44b93cec' := 'The first element',
	 '4edf4394-fd33-4643-9ed8-f3354fe96c28' := 'The second element',
	 '720ecc9e-c81f-4fac-a4d5-752c1d3f3f4f' := 'The third element'
  )
);


-- inserting person
INSERT INTO `s-basic-data-person` (
  `firstName`,
  `lastName`,
  `birthDate`,
  `contacts`,
  `passport`,
  `addresses`
) VALUES (
  'Kate',
  'Bishop',
  '2002-11-25',
  MAP(
    'email' := 'kate.bishop@marvel.com',
    'phone' := '999888777'
  ),
  STRUCT(
    `number` := 'MCU-PASS-957287759',
    `expiryDate` := '2029-08-18'
  ),
  ARRAY[
    STRUCT(
      `streetAddress` := 'Somewhere in New York',
      `country` := 'USA',
      `location` := STRUCT(
                      `latitude` := 40.830063426849705,
                      `longitude` := -74.14751581646931
                    )
    ),
    STRUCT(
      `streetAddress` := 'Tokyo, just there',
      `country` := 'Japan',
      `location` := STRUCT(
                      `latitude` := 35.734078460795104,
                      `longitude` := 139.62821562631277
                    )
    )
  ]
);

```
</details>

	
{::options parse_block_html="true" /}
	
----

## Customer  - Web & Mobile

{::options parse_block_html="true" /}
	
[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash
-- drop streams (if exists)
TERMINATE ALL;
DROP STREAM IF EXISTS `s-commodity-customer-purchase-all`;
DROP STREAM IF EXISTS `s-commodity-customer-purchase-mobile`;
DROP STREAM IF EXISTS `s-commodity-customer-purchase-web`;


-- Create stream from topic mobile
CREATE STREAM `s-commodity-customer-purchase-mobile`(
  `	purchaseNumber` VARCHAR,
  `purchaseAmount` INT,
  `mobileAppVersion` VARCHAR,
  `operatingSystem` VARCHAR,
  `location` STRUCT<
  			     `latitude` DOUBLE,
  			     `longitude` DOUBLE
  			   >
) WITH (
  KAFKA_TOPIC = 't-commodity-customer-purchase-mobile',
  VALUE_FORMAT = 'JSON'
);


-- Create stream from topic web
CREATE STREAM `s-commodity-customer-purchase-web`(
  `purchaseNumber` VARCHAR,
  `purchaseAmount` INT,
  `browser` VARCHAR,
  `operatingSystem` VARCHAR
) WITH (
  KAFKA_TOPIC = 't-commodity-customer-purchase-web',
  VALUE_FORMAT = 'JSON'
);


-- Create merged stream from topic mobile + web
CREATE STREAM `s-commodity-customer-purchase-all` (
  `purchaseNumber` VARCHAR,
  `purchaseAmount` INT,
  `mobileAppVersion` VARCHAR,
  `operatingSystem` VARCHAR,
  `location` STRUCT<
    			  `latitude` DOUBLE,
  	 			  `longitude` DOUBLE
   			 	>,
  `browser` VARCHAR,
  `source` VARCHAR
) WITH (
  KAFKA_TOPIC = 't-ksql-commodity-customer-purchase-all',
  PARTITIONS = 2,
  VALUE_FORMAT = 'JSON'
);


-- Insert into merged from stream mobile
INSERT INTO `s-commodity-customer-purchase-all`
  SELECT `purchaseNumber`,
    `purchaseAmount`,
    `mobileAppVersion`,
    `operatingSystem`,
    `location`,
    CAST(null AS VARCHAR) AS `browser`,
    'mobile' AS `source`
    FROM `s-commodity-customer-purchase-mobile`
  EMIT CHANGES;


-- Insert into merged from stream web
INSERT INTO `s-commodity-customer-purchase-all`
  SELECT `purchaseNumber`,
    `purchaseAmount`,
    CAST(null AS VARCHAR) AS `mobileAppVersion`,
    `operatingSystem`,
    CAST(null AS STRUCT<`latitude` DOUBLE, `longitude` DOUBLE>) AS `location`,
    `browser`,
    'web' AS `source`
    FROM `s-commodity-customer-purchase-web`
  EMIT CHANGES;
  
  
-- Push query from merged stream
SELECT * 
  FROM `s-commodity-customer-purchase-all`
EMIT CHANGES;
  
```
</details>


{::options parse_block_html="false" /}
	
## Customer - Shopping Cart & Wishlist

{::options parse_block_html="true" /}
	
[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash

-- drop stream (if exists)
TERMINATE ALL;
DROP TABLE IF EXISTS `tbl-commodity-customer-preference-all`;
DROP TABLE IF EXISTS `tbl-commodity-customer-preference-shopping-cart`;
DROP TABLE IF EXISTS `tbl-commodity-customer-preference-wishlist`;
DROP TABLE IF EXISTS `tbl-commodity-customer-cogroup-shopping-cart`;
DROP TABLE IF EXISTS `tbl-commodity-customer-cogroup-wishlist`;
DROP STREAM IF EXISTS `s-commodity-customer-preference-wishlist`;
DROP STREAM IF EXISTS `s-commodity-customer-preference-shopping-cart`;


-- Create stream from topic shopping cart
CREATE STREAM `s-commodity-customer-preference-shopping-cart`(
  `customerId` VARCHAR,
  `itemName` VARCHAR,
  `cartAmount` INT,
  `cartDatetime` VARCHAR
) WITH (
  KAFKA_TOPIC = 't-commodity-customer-preference-shopping-cart',
  VALUE_FORMAT = 'JSON'
);


-- Create stream from topic wishlist
CREATE STREAM `s-commodity-customer-preference-wishlist`(
  `customerId` VARCHAR,
  `itemName` VARCHAR,
  `wishlistDatetime` VARCHAR
) WITH (
  KAFKA_TOPIC = 't-commodity-customer-preference-wishlist',
  VALUE_FORMAT = 'JSON'
);


-- Create cogroup stream, taking latest cart date time for each item
CREATE TABLE `tbl-commodity-customer-cogroup-shopping-cart`
WITH (
 KEY_FORMAT = 'JSON'
)
AS
SELECT `customerId`, `itemName`, 
       ARRAY_MAX(
         COLLECT_LIST(`cartDatetime`)
       ) AS `latestCartDatetime`
  FROM `s-commodity-customer-preference-shopping-cart`
GROUP BY `customerId`, `itemName`
EMIT CHANGES;
	
	
-- Create map of <item name, latest add to cart datetime>
CREATE TABLE `tbl-commodity-customer-preference-shopping-cart`
AS
SELECT `customerId`, AS_MAP ( COLLECT_LIST(`itemName`), COLLECT_LIST(`latestCartDatetime`)) AS `cartItems`
  FROM `tbl-commodity-customer-cogroup-shopping-cart`
GROUP BY `customerId`
EMIT CHANGES;
  
  
-- Check the cogrouped & mapped shopping cart
SELECT * 
  FROM `tbl-commodity-customer-preference-shopping-cart`
EMIT CHANGES;


-- Create cogroup stream, taking latest wishlist date time for each item
CREATE TABLE `tbl-commodity-customer-cogroup-wishlist`
WITH (
 KEY_FORMAT = 'JSON'
)
AS
SELECT `customerId`, `itemName`, 
       ARRAY_MAX(
         COLLECT_LIST(`wishlistDatetime`)
       ) AS `latestWishlistDatetime`
  FROM `s-commodity-customer-preference-wishlist`
GROUP BY `customerId`, `itemName`
EMIT CHANGES;
	
	
-- Create map of <item name, latest wishlist datetime>
CREATE TABLE `tbl-commodity-customer-preference-wishlist`
AS
SELECT `customerId`, AS_MAP ( COLLECT_LIST(`itemName`), COLLECT_LIST(`latestWishlistDatetime`)) AS `wishlistItems`
FROM `tbl-commodity-customer-cogroup-wishlist`
GROUP BY `customerId`
EMIT CHANGES;
  
  
-- Check the cogrouped & mapped wishlist
SELECT * 
  FROM `tbl-commodity-customer-preference-wishlist`
EMIT CHANGES;


-- create merged preference from shopping cart + wishlist
CREATE TABLE `tbl-commodity-customer-preference-all`
AS
SELECT `tbl-commodity-customer-preference-shopping-cart`.`customerId` AS `customerId`,
       `cartItems`,
       `wishlistItems`
  FROM `tbl-commodity-customer-preference-shopping-cart`
       JOIN `tbl-commodity-customer-preference-wishlist`
         ON `tbl-commodity-customer-preference-shopping-cart`.`customerId` = `tbl-commodity-customer-preference-wishlist`.`customerId`
EMIT CHANGES;


-- Check the merged preference
SELECT * 
  FROM `tbl-commodity-customer-preference-all`
EMIT CHANGES;

```
</details>


{::options parse_block_html="false" /}
	
----

## Pull Query

See also postman collection sample for pull query.

{::options parse_block_html="true" /}
	
[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash

-- Pull query to stream (1)
SELECT `myBoolean`, `myDouble`, `myString`
  FROM `s-basic-data-one`;


-- Pull query to stream (2)
SELECT *
  FROM `s-basic-data-person`;


-- Pull query to table
SELECT * 
  FROM `tbl-commodity-customer-preference-all`
WHERE `customerId` = 'Linda';

```
</details>


{::options parse_block_html="false" /}
	
----

## Flash Sale

{::options parse_block_html="true" /}
	
[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash

-- drop stream if exists
TERMINATE ALL;
DROP TABLE IF EXISTS `tbl-commodity-flashsale-vote-one-result`;
DROP TABLE IF EXISTS `tbl-commodity-flashsale-vote-user-item`;
DROP STREAM IF EXISTS `s-commodity-flashsale-vote-user-item`;
DROP STREAM IF EXISTS `s-commodity-flashsale-vote`;


-- Create stream from underlying topic
CREATE STREAM `s-commodity-flashsale-vote` (
  `customerId` VARCHAR,
  `itemName` VARCHAR
) WITH (
  KAFKA_TOPIC = 't-commodity-flashsale-vote',
  VALUE_FORMAT = 'JSON'
);


-- Create table to know latest user vote
CREATE TABLE `tbl-commodity-flashsale-vote-user-item`
AS 
SELECT `customerId`, LATEST_BY_OFFSET(`itemName`) AS `itemName`
  FROM `s-commodity-flashsale-vote`
GROUP BY `customerId`;


-- table for item and vote count, based on latest user vote
CREATE TABLE `tbl-commodity-flashsale-vote-one-result`
AS
SELECT `itemName`, COUNT(`customerId`) AS `votesCount`
  FROM `tbl-commodity-flashsale-vote-user-item`
GROUP BY `itemName`
EMIT CHANGES;


-- push query
SELECT *
  FROM `tbl-commodity-flashsale-vote-one-result`
EMIT CHANGES;


-- pull query
SELECT *
  FROM `tbl-commodity-flashsale-vote-one-result`;
  
```
</details>


{::options parse_block_html="false" /}
	
----

## Flash Sale - Timestamp

{::options parse_block_html="true" /}
	
[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash
-- describe extended
DESCRIBE `s-commodity-flashsale-vote` EXTENDED;

-- drop stream if exists
TERMINATE ALL;
DROP TABLE IF EXISTS `tbl-commodity-flashsale-vote-two-result`;
DROP TABLE IF EXISTS `tbl-commodity-flashsale-vote-user-item-timestamp`;


-- Create table to know latest user vote, on certain time range
-- adjust the rowtime parameter to the time you run the lesson
CREATE TABLE `tbl-commodity-flashsale-vote-user-item-timestamp`
AS 
SELECT `customerId`, LATEST_BY_OFFSET(`itemName`) AS `itemName`
  FROM `s-commodity-flashsale-vote`
 WHERE rowtime >= '2024-09-06T22:00:00'
       AND rowtime < '2024-09-06T23:00:00'
GROUP BY `customerId`;


-- table for item and vote count, based on latest user vote, on certain time range
CREATE TABLE `tbl-commodity-flashsale-vote-two-result`
AS
SELECT `itemName`, COUNT(`customerId`) AS `votesCount`
  FROM `tbl-commodity-flashsale-vote-user-item-timestamp`
GROUP BY `itemName`
EMIT CHANGES;


```
</details>

{::options parse_block_html="false" /}
	
----

## Feedback - Average Rating

{::options parse_block_html="true" /}
	
[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash

-- drop stream if exists
TERMINATE ALL;
DROP TABLE IF EXISTS `tbl-commodity-feedback-rating-one`;


-- Create table for average rating by country
CREATE TABLE `tbl-commodity-feedback-rating-one`
AS 
SELECT `location`, AVG(`rating`) as `averageRating`
  FROM `s-commodity-feedback`
GROUP BY `location`
EMIT CHANGES;


-- Sample using HAVING
SET 'auto.offset.reset'='earliest';


SELECT `location`, AVG(`rating`) as `averageRating`
  FROM `s-commodity-feedback`
GROUP BY `location`
HAVING AVG(`rating`) <= 3.5
EMIT CHANGES;

```
</details>


{::options parse_block_html="false" /}
	
----

## Feedback - Detailed Rating

{::options parse_block_html="true" /}
	
[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash

-- drop table if exists
DROP TABLE IF EXISTS `tbl-commodity-feedback-rating-two`;


-- Create table for average rating and histogram
CREATE TABLE `tbl-commodity-feedback-rating-two`
AS 
SELECT `location`, 
       AVG(`rating`) as `averageRating`, 
       HISTOGRAM( CAST(`rating` AS VARCHAR) ) as `histogramRating`
  FROM `s-commodity-feedback`
GROUP BY `location`
EMIT CHANGES;
```

</details>

{::options parse_block_html="false" /}
	
----

## Inventory - Sum & Subtract Records

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash
-- drop stream if exists
TERMINATE ALL;
DROP TABLE IF EXISTS `tbl-commodity-inventory-total-two`;
DROP STREAM IF EXISTS `s-commodity-inventory-movement`;
DROP STREAM IF EXISTS `s-commodity-inventory`;


-- Create stream from underlying topic
CREATE STREAM `s-commodity-inventory` (
  `item` VARCHAR,
  `location` VARCHAR,
  `quantity` INT,
  `transactionTime` VARCHAR,
  `type` VARCHAR
) WITH (
  KAFKA_TOPIC = 't-commodity-inventory',
  VALUE_FORMAT = 'JSON'
);


-- Create stream based on type (ADD is positive, REMOVE is negative)
CREATE STREAM `s-commodity-inventory-movement`
AS
SELECT `item`,
       CASE
         WHEN `type` = 'ADD' THEN `quantity`
         WHEN `type` = 'REMOVE' THEN (-1 * `quantity`)
         ELSE 0
       END AS `quantity`
FROM `s-commodity-inventory`
EMIT CHANGES;


-- Create inventory table
CREATE TABLE `tbl-commodity-inventory-total-two`
AS 
SELECT `item`, SUM(`quantity`) AS `totalQuantity`
  FROM `s-commodity-inventory-movement`
GROUP BY `item`
EMIT CHANGES;

```
</details>

{::options parse_block_html="false" /}

----

## Inventory - Timestamp Extractor

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash
-- Push query showing rowtime (base stream)
SELECT `item`,
       `location`,
       `quantity`,
       `type`,
       `transactionTime`,
       FORMAT_TIMESTAMP( FROM_UNIXTIME(rowtime), 'yyyy-MM-dd''T''HH:mm:ss') AS `defaultRowtime`
FROM `s-commodity-inventory`
EMIT CHANGES;


-- drop stream if exists
TERMINATE ALL;
DROP STREAM IF EXISTS `s-commodity-inventory-four`;


-- Create stream with custom timestamp
CREATE STREAM `s-commodity-inventory-four`
WITH (
  TIMESTAMP = '`transactionTime`',
  TIMESTAMP_FORMAT = 'yyyy-MM-dd''T''HH:mm:ss.SSSZ'
)
AS
SELECT `item`,
       `location`,
       `quantity`,
       `transactionTime`,
       `type`
FROM `s-commodity-inventory`
EMIT CHANGES;


-- Push query showing rowtime (extracted from field)
SELECT `item`,
       `location`,
       `quantity`,
       `type`,
       `transactionTime`,
       FORMAT_TIMESTAMP( FROM_UNIXTIME(rowtime), 'yyyy-MM-dd''T''HH:mm:ss.SSSZ') AS `extractedTime`
FROM `s-commodity-inventory-four`
EMIT CHANGES;


-- describe stream
DESCRIBE `s-commodity-inventory-four` EXTENDED;

```
</details>

{::options parse_block_html="false" /}

----

## Inventory - Tumbling Time Window

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash

-- drop stream if exists
TERMINATE ALL;
DROP TABLE IF EXISTS `tbl-commodity-inventory-total-five`;
DROP STREAM IF EXISTS `s-commodity-inventory-five-movement`;


-- Create stream with custom timestamp and quantity movement
CREATE STREAM `s-commodity-inventory-five-movement`
WITH (
  TIMESTAMP = '`transactionTime`',
  TIMESTAMP_FORMAT = 'yyyy-MM-dd''T''HH:mm:ss.SSSZ'
)
AS
SELECT `item`,
       CASE
         WHEN `type` = 'ADD' THEN `quantity`
         WHEN `type` = 'REMOVE' THEN (-1 * `quantity`)
         ELSE 0
       END AS `quantity`,
       `transactionTime`
FROM `s-commodity-inventory`
EMIT CHANGES;


-- Tumbling window
CREATE TABLE `tbl-commodity-inventory-total-five`
AS
SELECT FORMAT_TIMESTAMP( FROM_UNIXTIME(windowstart), 'yyyy-MM-dd''T''HH:mm:ss.SSSZ') AS `windowStartTime`,
       FORMAT_TIMESTAMP( FROM_UNIXTIME(windowend), 'yyyy-MM-dd''T''HH:mm:ss.SSSZ') AS `windowEndTime`,
       `item`, SUM(`quantity`) `totalQuantity`
  FROM `s-commodity-inventory-five-movement`
WINDOW TUMBLING (SIZE 1 HOUR)
GROUP BY `item`
EMIT CHANGES;

```
</details>


{::options parse_block_html="false" /}

----

## Inventory - Hopping Time Window

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash

-- drop stream if exists
TERMINATE ALL;
DROP TABLE IF EXISTS `tbl-commodity-inventory-total-six`;
DROP STREAM IF EXISTS `s-commodity-inventory-six-movement`;


-- Create stream with custom timestamp and quantity movement
CREATE STREAM `s-commodity-inventory-six-movement`
WITH (
  TIMESTAMP = '`transactionTime`',
  TIMESTAMP_FORMAT = 'yyyy-MM-dd''T''HH:mm:ss.SSSZ'
)
AS
SELECT `item`,
       CASE
         WHEN `type` = 'ADD' THEN `quantity`
         WHEN `type` = 'REMOVE' THEN (-1 * `quantity`)
         ELSE 0
       END AS `quantity`,
       `transactionTime`
FROM `s-commodity-inventory`
EMIT CHANGES;


-- Hopping window
CREATE TABLE `tbl-commodity-inventory-total-six`
AS
SELECT FORMAT_TIMESTAMP( FROM_UNIXTIME(windowstart), 'yyyy-MM-dd''T''HH:mm:ss.SSSZ') AS `windowStartTime`,
       FORMAT_TIMESTAMP( FROM_UNIXTIME(windowend), 'yyyy-MM-dd''T''HH:mm:ss.SSSZ') AS `windowEndTime`,
       `item`, SUM(`quantity`) `totalQuantity`
  FROM `s-commodity-inventory-six-movement`
WINDOW HOPPING (SIZE 1 HOUR, ADVANCE BY 20 MINUTES)
GROUP BY `item`
EMIT CHANGES;

```
</details>

{::options parse_block_html="false" /}


----

## Inventory - Sessioon Window

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash

-- drop stream if exists
TERMINATE ALL;
DROP TABLE IF EXISTS `tbl-commodity-inventory-total-seven`;
DROP STREAM IF EXISTS `s-commodity-inventory-seven-movement`;


-- Create stream with custom timestamp and quantity movement
CREATE STREAM `s-commodity-inventory-seven-movement`
WITH (
  TIMESTAMP = '`transactionTime`',
  TIMESTAMP_FORMAT = 'yyyy-MM-dd''T''HH:mm:ss.SSSZ'
)
AS
SELECT `item`,
       CASE
         WHEN `type` = 'ADD' THEN `quantity`
         WHEN `type` = 'REMOVE' THEN (-1 * `quantity`)
         ELSE 0
       END AS `quantity`,
       `transactionTime`
FROM `s-commodity-inventory`
EMIT CHANGES;


-- Session window
CREATE TABLE `tbl-commodity-inventory-total-seven`
AS
SELECT FORMAT_TIMESTAMP( FROM_UNIXTIME(windowstart), 'yyyy-MM-dd''T''HH:mm:ss.SSSZ') AS `windowStartTime`,
       FORMAT_TIMESTAMP( FROM_UNIXTIME(windowend), 'yyyy-MM-dd''T''HH:mm:ss.SSSZ') AS `windowEndTime`,
       `item`, SUM(`quantity`) `totalQuantity`
  FROM `s-commodity-inventory-seven-movement`
WINDOW SESSION (30 MINUTES)
GROUP BY `item`
EMIT CHANGES;

```
</details>

{::options parse_block_html="false" /}

----

## Inner Join Stream / Stream

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash

-- drop stream if exists
TERMINATE ALL;
DROP STREAM IF EXISTS `s-commodity-join-order-payment-one`;
DROP STREAM IF EXISTS `s-commodity-online-order`;
DROP STREAM IF EXISTS `s-commodity-online-payment`;


-- Create stream online order
CREATE STREAM `s-commodity-online-order` (
  `orderDateTime` VARCHAR,
  `onlineOrderNumber` VARCHAR KEY,
  `totalAmount` INT, 
  `username` VARCHAR
)
WITH (
  TIMESTAMP = '`orderDateTime`',
  TIMESTAMP_FORMAT = 'yyyy-MM-dd''T''HH:mm:ss.SSSZ',
  KAFKA_TOPIC = 't-commodity-online-order',
  VALUE_FORMAT = 'JSON'
);


-- Create stream online payment
CREATE STREAM `s-commodity-online-payment` (
  `paymentDateTime` VARCHAR,
  `onlineOrderNumber` VARCHAR KEY,
  `paymentMethod` VARCHAR,
  `paymentNumber` VARCHAR
)
WITH (
  TIMESTAMP = '`paymentDateTime`',
  TIMESTAMP_FORMAT = 'yyyy-MM-dd''T''HH:mm:ss.SSSZ',
  KAFKA_TOPIC = 't-commodity-online-payment',
  VALUE_FORMAT = 'JSON'
);


-- Inner join with no grace period
CREATE STREAM `s-commodity-join-order-payment-one`
AS
SELECT `s-commodity-online-order`.`onlineOrderNumber` AS `onlineOrderNumber`,
       PARSE_TIMESTAMP(`s-commodity-online-order`.`orderDateTime`, 'yyyy-MM-dd''T''HH:mm:ss.SSSZ') AS `orderDateTime`,
       `s-commodity-online-order`.`totalAmount` AS `totalAmount`,
       `s-commodity-online-order`.`username` AS `username`,
       PARSE_TIMESTAMP(`s-commodity-online-payment`.`paymentDateTime`, 'yyyy-MM-dd''T''HH:mm:ss.SSSZ') AS `paymentDateTime`,
       `s-commodity-online-payment`.`paymentMethod` AS `paymentMethod`,
       `s-commodity-online-payment`.`paymentNumber` AS `paymentNumber`
  FROM `s-commodity-online-order`
    INNER JOIN `s-commodity-online-payment`
      WITHIN 1 HOUR GRACE PERIOD 0 MILLISECOND
      ON `s-commodity-online-order`.`onlineOrderNumber` = `s-commodity-online-payment`.`onlineOrderNumber`
EMIT CHANGES;

```
</details>

{::options parse_block_html="false" /}

----

## Left Join Stream / Stream

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash

-- drop stream if exists
TERMINATE ALL;
DROP STREAM IF EXISTS `s-commodity-join-order-payment-two`;

-- Left join
CREATE STREAM `s-commodity-join-order-payment-two`
AS
SELECT `s-commodity-online-order`.`onlineOrderNumber` AS `onlineOrderNumber`,
       PARSE_TIMESTAMP(`s-commodity-online-order`.`orderDateTime`, 'yyyy-MM-dd''T''HH:mm:ss.SSSZ') AS `orderDateTime`,
       `s-commodity-online-order`.`totalAmount` AS `totalAmount`,
       `s-commodity-online-order`.`username` AS `username`,
       PARSE_TIMESTAMP(`s-commodity-online-payment`.`paymentDateTime`, 'yyyy-MM-dd''T''HH:mm:ss.SSSZ') AS `paymentDateTime`,
       `s-commodity-online-payment`.`paymentMethod` AS `paymentMethod`,
       `s-commodity-online-payment`.`paymentNumber` AS `paymentNumber`
  FROM `s-commodity-online-order`
    LEFT JOIN `s-commodity-online-payment`
      WITHIN 1 HOUR GRACE PERIOD 0 MILLISECOND
      ON `s-commodity-online-order`.`onlineOrderNumber` = `s-commodity-online-payment`.`onlineOrderNumber`
EMIT CHANGES;

```
</details>

{::options parse_block_html="false" /}

----

## Outer Join Stream / Stream

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash

-- drop stream if exists
TERMINATE ALL;
DROP STREAM IF EXISTS `s-commodity-join-order-payment-three`;

-- Full outer join
CREATE STREAM `s-commodity-join-order-payment-three`
AS
SELECT ROWKEY as `syntheticKey`,
       `s-commodity-online-order`.`onlineOrderNumber` AS `onlineOrderNumber`,
       PARSE_TIMESTAMP(`s-commodity-online-order`.`orderDateTime`, 'yyyy-MM-dd''T''HH:mm:ss.SSSZ') AS `orderDateTime`,
       `s-commodity-online-order`.`totalAmount` AS `totalAmount`,
       `s-commodity-online-order`.`username` AS `username`,
       PARSE_TIMESTAMP(`s-commodity-online-payment`.`paymentDateTime`, 'yyyy-MM-dd''T''HH:mm:ss.SSSZ') AS `paymentDateTime`,
       `s-commodity-online-payment`.`paymentMethod` AS `paymentMethod`,
       `s-commodity-online-payment`.`paymentNumber` AS `paymentNumber`
  FROM `s-commodity-online-order`
    FULL JOIN `s-commodity-online-payment`
      WITHIN 1 HOUR
      ON `s-commodity-online-order`.`onlineOrderNumber` = `s-commodity-online-payment`.`onlineOrderNumber`
EMIT CHANGES;

```
</details>

{::options parse_block_html="false" /}

----

## Inner Join Table / Table

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash

-- drop stream if exists
TERMINATE ALL;
DROP TABLE IF EXISTS `t-commodity-web-vote-one-result-color`;
DROP TABLE IF EXISTS `t-commodity-web-vote-one-result-layout`;
DROP TABLE IF EXISTS `tbl-commodity-web-vote-username-color`;
DROP TABLE IF EXISTS `tbl-commodity-web-vote-username-layout`;
DROP STREAM IF EXISTS `s-commodity-web-vote-color`;
DROP STREAM IF EXISTS `s-commodity-web-vote-layout`;


-- Create stream from underlying topic (color)
CREATE STREAM `s-commodity-web-vote-color` (
  `username` VARCHAR,
  `color` VARCHAR,
  `voteDateTime` VARCHAR
) WITH (
  KAFKA_TOPIC = 't-commodity-web-vote-color',
  VALUE_FORMAT = 'JSON'
);


-- Create stream from underlying topic (layout)
CREATE STREAM `s-commodity-web-vote-layout` (
  `username` VARCHAR,
  `layout` VARCHAR,
  `voteDateTime` VARCHAR
) WITH (
  KAFKA_TOPIC = 't-commodity-web-vote-layout',
  VALUE_FORMAT = 'JSON'
);


-- Create table to know latest user vote (color)
CREATE TABLE `tbl-commodity-web-vote-username-color`
AS 
SELECT `username`, LATEST_BY_OFFSET(`color`) AS `color`
  FROM `s-commodity-web-vote-color`
GROUP BY `username` EMIT CHANGES;


-- Create table to know latest user vote (layout)
CREATE TABLE `tbl-commodity-web-vote-username-layout`
AS 
SELECT `username`, LATEST_BY_OFFSET(`layout`) AS `layout`
  FROM `s-commodity-web-vote-layout`
GROUP BY `username` EMIT CHANGES;


-- table for item and vote count, based on latest user vote (color only)
CREATE TABLE `t-commodity-web-vote-one-result-color`
AS
SELECT `color`, 
        COUNT(`tbl-commodity-web-vote-username-color`.`username`) AS `votesCount`
  FROM `tbl-commodity-web-vote-username-color`
    INNER JOIN `tbl-commodity-web-vote-username-layout`
      ON `tbl-commodity-web-vote-username-color`.`username` = `tbl-commodity-web-vote-username-layout`.`username`
GROUP BY `color`
EMIT CHANGES;


-- table for item and vote count, based on latest user vote (layout only)
CREATE TABLE `t-commodity-web-vote-one-result-layout`
AS
SELECT `layout`, 
        COUNT(`tbl-commodity-web-vote-username-layout`.`username`) AS `votesCount`
  FROM `tbl-commodity-web-vote-username-color`
    INNER JOIN `tbl-commodity-web-vote-username-layout`
      ON `tbl-commodity-web-vote-username-color`.`username` = `tbl-commodity-web-vote-username-layout`.`username`
GROUP BY `layout`
EMIT CHANGES;

```
</details>

{::options parse_block_html="false" /}

----

## Left Join Table / Table

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash

-- drop stream if exists
TERMINATE ALL;
DROP TABLE IF EXISTS `t-commodity-web-vote-two-result-color`;
DROP TABLE IF EXISTS `t-commodity-web-vote-two-result-layout`;


-- table for item and vote count, based on latest user vote (color only)
CREATE TABLE `t-commodity-web-vote-two-result-color`
AS
SELECT `color`, 
        COUNT(`tbl-commodity-web-vote-username-color`.`username`) AS `votesCount`
  FROM `tbl-commodity-web-vote-username-color`
    LEFT JOIN `tbl-commodity-web-vote-username-layout`
      ON `tbl-commodity-web-vote-username-color`.`username` = `tbl-commodity-web-vote-username-layout`.`username`
GROUP BY `color`
EMIT CHANGES;


-- table for item and vote count, based on latest user vote (layout only)
CREATE TABLE `t-commodity-web-vote-two-result-layout`
AS
SELECT `layout`, 
        COUNT(`tbl-commodity-web-vote-username-layout`.`username`) AS `votesCount`
  FROM `tbl-commodity-web-vote-username-color`
    LEFT JOIN `tbl-commodity-web-vote-username-layout`
      ON `tbl-commodity-web-vote-username-color`.`username` = `tbl-commodity-web-vote-username-layout`.`username`
GROUP BY `layout`
EMIT CHANGES;

```
</details>

{::options parse_block_html="false" /}

----

## Outer Join Table / Table

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash

-- drop stream if exists
TERMINATE ALL;
DROP TABLE IF EXISTS `t-commodity-web-vote-three-result-color`;
DROP TABLE IF EXISTS `t-commodity-web-vote-three-result-layout`;


-- table for item and vote count, based on latest user vote (color only)
CREATE TABLE `t-commodity-web-vote-three-result-color`
AS
SELECT `color`, 
        COUNT(`tbl-commodity-web-vote-username-color`.`username`) AS `votesCount`
  FROM `tbl-commodity-web-vote-username-color`
    FULL JOIN `tbl-commodity-web-vote-username-layout`
      ON `tbl-commodity-web-vote-username-color`.`username` = `tbl-commodity-web-vote-username-layout`.`username`
GROUP BY `color`
EMIT CHANGES;


-- table for item and vote count, based on latest user vote (layout only)
CREATE TABLE `t-commodity-web-vote-three-result-layout`
AS
SELECT `layout`, 
        COUNT(`tbl-commodity-web-vote-username-layout`.`username`) AS `votesCount`
  FROM `tbl-commodity-web-vote-username-color`
    FULL JOIN `tbl-commodity-web-vote-username-layout`
      ON `tbl-commodity-web-vote-username-color`.`username` = `tbl-commodity-web-vote-username-layout`.`username`
GROUP BY `layout`
EMIT CHANGES;

```
</details>

{::options parse_block_html="false" /}

----

## Inner Join Stream / Table

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash

-- drop stream if exists
TERMINATE ALL;
DROP STREAM IF EXISTS `s-commodity-premium-offer-one`;
DROP TABLE IF EXISTS `tbl-commodity-premium-user`;
DROP STREAM IF EXISTS `s-commodity-premium-purchase`;
DROP STREAM IF EXISTS `s-commodity-premium-user`;


-- Create stream from underlying topic (purchase)
CREATE STREAM `s-commodity-premium-purchase` (
  `username` VARCHAR,
  `purchaseNumber` VARCHAR,
  `item` VARCHAR
) WITH (
  KAFKA_TOPIC = 't-commodity-premium-purchase',
  VALUE_FORMAT = 'JSON'
);


-- Create stream from underlying topic (user)
CREATE STREAM `s-commodity-premium-user` (
  `username` VARCHAR,
  `level` VARCHAR
) WITH (
  KAFKA_TOPIC = 't-commodity-premium-user',
  VALUE_FORMAT = 'JSON'
);


-- table for latest user level
CREATE TABLE `tbl-commodity-premium-user`
AS
SELECT `username`, LATEST_BY_OFFSET(`level`) AS `level`
  FROM `s-commodity-premium-user`
GROUP BY `username`
EMIT CHANGES;


-- join stream / table, filter only 'gold' and 'diamond' users
CREATE STREAM `s-commodity-premium-offer-one`
AS
SELECT `s-commodity-premium-purchase`.`username` AS `username`, 
        `level`, `purchaseNumber`
  FROM `s-commodity-premium-purchase`
    INNER JOIN `tbl-commodity-premium-user`
      ON `s-commodity-premium-purchase`.`username` = `tbl-commodity-premium-user`.`username`
  WHERE LCASE(`level`) IN ('gold', 'diamond')
EMIT CHANGES;

```
</details>

{::options parse_block_html="false" /}

----

## Left Join Stream / Table

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash

-- drop stream if exists
TERMINATE ALL;
DROP STREAM IF EXISTS `s-commodity-premium-offer-two`;


-- join stream / table, filter only 'gold' and 'diamond' users
CREATE STREAM `s-commodity-premium-offer-two`
AS
SELECT `s-commodity-premium-purchase`.`username` AS `username`, 
        `level`, `purchaseNumber`
  FROM `s-commodity-premium-purchase`
    LEFT JOIN `tbl-commodity-premium-user`
      ON `s-commodity-premium-purchase`.`username` = `tbl-commodity-premium-user`.`username`
  WHERE `level` IS NULL 
        OR LCASE(`level`) IN ('gold', 'diamond')
EMIT CHANGES;

```
</details>

{::options parse_block_html="false" /}

----

## Stream / Table Co-partition

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash

-- drop stream if exists
TERMINATE ALL;
DROP STREAM IF EXISTS `s-commodity-subscription-offer-one`;
DROP STREAM IF EXISTS `s-commodity-subscription-offer-two`;
DROP TABLE IF EXISTS `tbl-commodity-subscription-user-repartition`;
DROP TABLE IF EXISTS `tbl-commodity-subscription-user`;
DROP STREAM IF EXISTS `s-commodity-subscription-user`;
DROP STREAM IF EXISTS `s-commodity-subscription-purchase`;


-- Create stream from underlying topic (user)
CREATE STREAM `s-commodity-subscription-user` (
  `username` VARCHAR KEY,
  `duration` VARCHAR
) WITH (
  KAFKA_TOPIC = 't-commodity-subscription-user',
  VALUE_FORMAT = 'JSON'
);


-- Create stream from underlying topic (purchase)
CREATE STREAM `s-commodity-subscription-purchase` (
  `username` VARCHAR KEY,
  `subscriptionNumber` VARCHAR
) WITH (
  KAFKA_TOPIC = 't-commodity-subscription-purchase',
  VALUE_FORMAT = 'JSON'
);


-- table for latest user subscription
CREATE TABLE `tbl-commodity-subscription-user`
AS
SELECT `username`, LATEST_BY_OFFSET(`duration`) AS `duration`
  FROM `s-commodity-subscription-user`
GROUP BY `username`
EMIT CHANGES;


-- see partition number (5 on stream)
DESCRIBE `s-commodity-subscription-purchase` EXTENDED;

-- see partition number (2 on table)
DESCRIBE `tbl-commodity-subscription-user` EXTENDED;


-- join stream / table (different partition number, will fail)
CREATE STREAM `s-commodity-subscription-offer-one`
AS
SELECT `s-commodity-subscription-purchase`.`username` AS `username`,
       `subscriptionNumber`,`duration`
  FROM `s-commodity-subscription-purchase`
    INNER JOIN `tbl-commodity-subscription-user`
      ON `s-commodity-subscription-purchase`.`username` = `tbl-commodity-subscription-user`.`username`
EMIT CHANGES;


-- re-partition table for latest user duration (5 partitions)
CREATE TABLE `tbl-commodity-subscription-user-repartition`
WITH (
  PARTITIONS = 5
)
AS
SELECT `username`, LATEST_BY_OFFSET(`duration`) AS `duration`
  FROM `s-commodity-subscription-user`
GROUP BY `username`
EMIT CHANGES;


-- see partition number (5 on table)
DESCRIBE `tbl-commodity-subscription-user-repartition` EXTENDED;


-- join stream / re-partitioned table (same partition number)
CREATE STREAM `s-commodity-subscription-offer-two`
AS
SELECT `s-commodity-subscription-purchase`.`username` AS `username`,
       `subscriptionNumber`,`duration`
  FROM `s-commodity-subscription-purchase`
    INNER JOIN `tbl-commodity-subscription-user-repartition`
      ON `s-commodity-subscription-purchase`.`username` = `tbl-commodity-subscription-user-repartition`.`username`
EMIT CHANGES;

```
</details>
  
{::options parse_block_html="false" /}

----

## User Defined Function

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash

-- show functions
SHOW FUNCTIONS;


-- describe function
DESCRIBE FUNCTION LOAN_INSTALLMENT;


-- drop stream if exists
TERMINATE ALL;
DROP STREAM IF EXISTS `s-commodity-loan-request` DELETE TOPIC;


-- Create new stream and new topic
CREATE STREAM `s-commodity-loan-request` (
  `username` VARCHAR,
  `principalLoanAmount` DOUBLE,
  `annualInterestRate` DOUBLE,
  `loanPeriodMonth` INT
) WITH (
  KAFKA_TOPIC = 't-commodity-loan-request',
  PARTITIONS = 2,
  VALUE_FORMAT = 'JSON'
);


-- insert data
INSERT INTO `s-commodity-loan-request` (
  `username`,
  `principalLoanAmount`,
  `annualInterestRate`,
  `loanPeriodMonth`
) VALUES (
  'danny',
  1000,
  12,
  12
);


INSERT INTO `s-commodity-loan-request` (
  `username`,
  `principalLoanAmount`,
  `annualInterestRate`,
  `loanPeriodMonth`
) VALUES (
  'melvin',
  1500,
  10.5,
  24
);


INSERT INTO `s-commodity-loan-request` (
  `username`,
  `principalLoanAmount`,
  `annualInterestRate`,
  `loanPeriodMonth`
) VALUES (
  'thomas',
  3500,
  11.2,
  36
);

-- try the UDF. Make sure udf jar already uploaded
SET 'auto.offset.reset'='earliest';


SELECT `username`, `principalLoanAmount`, `annualInterestRate`, `loanPeriodMonth`,
       LOAN_INSTALLMENT(`principalLoanAmount`, `annualInterestRate`, `loanPeriodMonth`) AS `monthlyLoanInstallment`
  FROM `s-commodity-loan-request`
EMIT CHANGES;

```
</details>
  
{::options parse_block_html="false" /}

----

## User Defined Tabular Function

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash
-- drop stream if exists
DROP STREAM IF EXISTS `s-commodity-loan-submission` DELETE TOPIC;

-- create stream with struct
CREATE STREAM `s-commodity-loan-submission` (
  `loanSubmission` STRUCT<
  					  `principalLoanAmount` DOUBLE, 
			 			  `annualInterestRate` DOUBLE,   
			 			  `loanPeriodMonth` INT,   
			 			  `loanApprovedDate` VARCHAR  
			 		  >
) WITH (
  KAFKA_TOPIC = 't-commodity-loan-submission',
  PARTITIONS = 2,
  VALUE_FORMAT = 'JSON'
);


-- insert dummy data
INSERT INTO `s-commodity-loan-submission` (
  `loanSubmission`
) VALUES (
  STRUCT(
    `principalLoanAmount` := 6000,
    `annualInterestRate` := 11.5,
    `loanPeriodMonth` := 24,
    `loanApprovedDate` := '2024-11-21'
  )
);


-- run query
SELECT LOAN_INSTALLMENT_SCHEDULE(`loanSubmission`)
  FROM `s-commodity-loan-submission`;

```

</details>

{::options parse_block_html="false" /}

----


## User Defined Aggregation Function

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash
-- drop stream if exists
TERMINATE ALL;
DROP STREAM IF EXISTS `s-commodity-loan-payment-latency` DELETE TOPIC;
DROP STREAM IF EXISTS `s-commodity-loan-payment` DELETE TOPIC;

-- create stream for payment
CREATE STREAM `s-commodity-loan-payment` (
  `loanNumber` VARCHAR,
  `installmentDueDate` VARCHAR,
  `installmentPaidDate` VARCHAR
) WITH (
  KAFKA_TOPIC = 't-commodity-loan-payment',
  PARTITIONS = 2,
  VALUE_FORMAT = 'JSON'
);


-- create stream with payment latency. Positive latency means late payment (bad).
CREATE STREAM `s-commodity-loan-payment-latency`
AS
SELECT `loanNumber`, 
       `installmentDueDate`,
       `installmentPaidDate`,
       UNIX_DATE(PARSE_DATE(`installmentPaidDate`, 'yyyy-MM-dd')) -
       UNIX_DATE(PARSE_DATE(`installmentDueDate`, 'yyyy-MM-dd')) AS `paymentLatency`
  FROM `s-commodity-loan-payment`;


-- insert dummy data
INSERT INTO `s-commodity-loan-payment` (
  `loanNumber`,
  `installmentDueDate`,
  `installmentPaidDate`
) VALUES (
  'LOAN-111',
  '2024-04-17',
  '2024-04-15'
);


INSERT INTO `s-commodity-loan-payment` (
  `loanNumber`,
  `installmentDueDate`,
  `installmentPaidDate`
) VALUES (
  'LOAN-111',
  '2024-05-17',
  '2024-05-05'
);


INSERT INTO `s-commodity-loan-payment` (
  `loanNumber`,
  `installmentDueDate`,
  `installmentPaidDate`
) VALUES (
  'LOAN-111',
  '2024-06-17',
  '2024-06-09'
);


INSERT INTO `s-commodity-loan-payment` (
  `loanNumber`,
  `installmentDueDate`,
  `installmentPaidDate`
) VALUES (
  'LOAN-111',
  '2024-07-17',
  '2024-07-17'
);


INSERT INTO `s-commodity-loan-payment` (
  `loanNumber`,
  `installmentDueDate`,
  `installmentPaidDate`
) VALUES (
  'LOAN-111',
  '2024-08-17',
  '2024-08-15'
);


-- insert dummy data 2
INSERT INTO `s-commodity-loan-payment` (
  `loanNumber`,
  `installmentDueDate`,
  `installmentPaidDate`
) VALUES (
  'LOAN-222',
  '2024-04-14',
  '2024-04-15'
);


INSERT INTO `s-commodity-loan-payment` (
  `loanNumber`,
  `installmentDueDate`,
  `installmentPaidDate`
) VALUES (
  'LOAN-222',
  '2024-05-14',
  '2024-05-05'
);


INSERT INTO `s-commodity-loan-payment` (
  `loanNumber`,
  `installmentDueDate`,
  `installmentPaidDate`
) VALUES (
  'LOAN-222',
  '2024-06-14',
  '2024-06-19'
);


INSERT INTO `s-commodity-loan-payment` (
  `loanNumber`,
  `installmentDueDate`,
  `installmentPaidDate`
) VALUES (
  'LOAN-222',
  '2024-07-14',
  '2024-07-22'
);


INSERT INTO `s-commodity-loan-payment` (
  `loanNumber`,
  `installmentDueDate`,
  `installmentPaidDate`
) VALUES (
  'LOAN-222',
  '2024-08-14',
  '2024-08-15'
);

-- run query
SET 'auto.offset.reset'='earliest';

SELECT `loanNumber`, LOAN_RATING(`paymentLatency`) AS `loanRating`
  FROM `s-commodity-loan-payment-latency`
GROUP BY `loanNumber`
EMIT CHANGES;

```

</details>

{::options parse_block_html="false" /}

----

## Avro on ksqldb

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash
-- drop stream if exists
TERMINATE ALL;
DROP STREAM IF EXISTS `s-avro01`;

-- create stream from topic
CREATE STREAM `s-avro01`
WITH (
  KAFKA_TOPIC = 'sc-avro01',
  VALUE_FORMAT = 'AVRO'
);


-- describe stream
DESCRIBE `s-avro01`;


-- push query
SELECT *
  FROM `s-avro01`
EMIT CHANGES;


-- insert some dummy data, will fail
INSERT INTO `s-avro01` (
  fullName,
  maritalStatus,
  active
) VALUES (
  'Clark Kent',
  'MARRIED',
  true
);

```

</details>

{::options parse_block_html="false" /}

----

## Writing Avro Schema

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash
-- drop stream if exists
TERMINATE ALL;
DROP STREAM IF EXISTS `s-avro-member` DELETE TOPIC;


-- create stream from topic
CREATE STREAM `s-avro-member` (
  `email` VARCHAR,
  `username` VARCHAR,
  `birthDate` VARCHAR,
  `membership` VARCHAR
)
WITH (
  KAFKA_TOPIC = 'sc-avro-member',
  PARTITIONS = 1,
  VALUE_FORMAT = 'AVRO'
);


-- insert some dummy data
INSERT INTO `s-avro-member` (
  `email`,
  `username`,
  `birthDate`,
  `membership`
) VALUES (
  'thor@asgard.com',
  'god_of_thunder',
  '1900-05-19',
  'black'
);


INSERT INTO `s-avro-member` (
  `email`,
  `username`,
  `birthDate`,
  `membership`
) VALUES (
  'loki@asgard.com',
  'iamloki',
  '1914-11-05',
  'black'
);


INSERT INTO `s-avro-member` (
  `email`,
  `username`,
  `birthDate`,
  `membership`
) VALUES (
  'kang@universe.com',
  'kang.the.conqueror',
  '1912-10-05',
  'white'
);


INSERT INTO `s-avro-member` (
  `email`,
  `username`,
  `birthDate`,
  `membership`
) VALUES (
  'zeus@olympus.com',
  'therealgodofthunder',
  '1852-01-05',
  'white'
);


INSERT INTO `s-avro-member` (
  `email`,
  `username`,
  `birthDate`,
  `membership`
) VALUES (
  'athena@olympus.com',
  'prettybutdeadly',
  '1922-08-25',
  'blue'
);


-- query stream
SELECT * 
  FROM `s-avro-member`
EMIT CHANGES;


-- stream for black membership only
DROP STREAM IF EXISTS `s-avro-member-black`;


CREATE STREAM `s-avro-member-black`
WITH (
  VALUE_FORMAT = 'AVRO'
)
AS
SELECT * 
  FROM `s-avro-member`
 WHERE LCASE(`membership`) = 'black';


DESCRIBE `s-avro-member-black`;


-- table for count membership only
DROP TABLE IF EXISTS `tbl-avro-member-count`;


CREATE TABLE `tbl-avro-member-count`
WITH (
  VALUE_FORMAT = 'AVRO'
)
AS
SELECT `membership`, COUNT(`email`) AS `countMember`
  FROM `s-avro-member`
GROUP BY `membership`
EMIT CHANGES;


DESCRIBE `tbl-avro-member-count`; 
```

</details>

{::options parse_block_html="false" /}

----

## Avro Json Conversion

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash
-- drop stream if exists
TERMINATE ALL;
DROP STREAM IF EXISTS `s-avro-member-json` DELETE TOPIC;


-- create stream from topic
CREATE STREAM `s-avro-member-json`
WITH (
  VALUE_FORMAT = 'JSON'    
)
AS
SELECT * 
  FROM `s-avro-member`
EMIT CHANGES;


INSERT INTO `s-avro-member` (
  `email`,
  `username`,
  `birthDate`,
  `membership`
) VALUES (
  'kara@dc.com',
  'supergirl',
  '1993-11-05',
  'black'
);


-- create new json stream 
CREATE STREAM `s-power-json` (
  `power` VARCHAR,
  `level` INT
) WITH (
  VALUE_FORMAT = 'JSON',
  KAFKA_TOPIC = 't-power-json',
  PARTITIONS = 1
);


-- dummy data
INSERT INTO `s-power-json` (
  `power`,
  `level`
) VALUES (
  'healing',
  6
);

INSERT INTO `s-power-json` (
  `power`,
  `level`
) VALUES (
  'energy projection',
  8
);

INSERT INTO `s-power-json` (
  `power`,
  `level`
) VALUES (
  'mind control',
  7
);


-- create avro stream 
CREATE STREAM `s-power-avro` 
WITH (
  VALUE_FORMAT = 'AVRO'
)
AS
SELECT * 
  FROM `s-power-json`
EMIT CHANGES;
```

</details>

{::options parse_block_html="false" /}

----

## KsqlDB & Kafka Connect

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash
-- show existing connectors
SHOW CONNECTORS;


-- show connector detail
DESCRIBE CONNECTOR `source-spooldir-csv`;


-- create dummy source connector
CREATE SOURCE CONNECTOR `source-spooldir-dummy-csv` 
WITH (
    'connector.class'='com.github.jcustenborder.kafka.connect.spooldir.SpoolDirCsvSourceConnector',
    'topic'='t-spooldir-csv-demo',
    'input.file.pattern'='dummy-.*.csv',
    'input.path'='/data/inputs',
    'error.path'='/data/errors',
    'finished.path'='/data/processed',
    'schema.generation.enabled'='true',
    'csv.first.row.as.header'='true',
    'empty.poll.wait.ms'='10000'
);


-- create dummy sink connector
CREATE SINK CONNECTOR `sink-postgresql-dummy-csv`
WITH (
    'connector.class'='io.confluent.connect.jdbc.JdbcSinkConnector',
    'topics'='t-spooldir-csv-demo',
    'confluent.topic.bootstrap.servers'='192.168.0.7:9092',
    'connection.url'='jdbc:postgresql://192.168.0.7:5432/postgres',
    'connection.user'='postgres',
    'connection.password'='postgres',
    'table.name.format'='kafka_employees',
    'auto.create'=true,
    'auto.evolve'=true,
    'pk.mode'='record_value',
    'pk.fields'='employee_id',
    'insert.mode'='upsert'
);


-- drop dummy connector
DROP CONNECTOR IF EXISTS `source-spooldir-dummy-csv`;

DROP CONNECTOR IF EXISTS `sink-postgresql-dummy-csv`;

```

</details>

{::options parse_block_html="false" /}

----

## Java Client

{::options parse_block_html="true" /}

[Back to top](/spring-kafka-bootcamp/ksqldb)
<details>
  <summary markdown="span">Click to expand! </summary>

```bash

-- Set the offset to earliest
SET 'auto.offset.reset'='earliest';

-- Push query
SELECT * 
  FROM `s-java-client`
EMIT CHANGES;

```

</details>

{::options parse_block_html="false" /}

----

[Back to index](/spring-kafka-bootcamp)
