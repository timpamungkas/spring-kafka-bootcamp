# Spring Kafka Stream

  * [Create Kafka Topics](#create-kafka-topics)
  * [Kafka Stream Config](#kafka-stream-config)
  * [Kafka Console Consumer Scripts](#kafka-console-consumer-scripts)
  * [Inventory - Timestamp Extractor](#inventory---timestamp-extractor)

----

## Create Kafka Topics

{::options parse_block_html="true" /}

<details><summary markdown="span">Click to expand!</summary>
Go to kafka console.  
If you are using docker, type `docker exec -it [kafka-container-name] bash`  
For container name, you can get it by typing `docker ps`
 
```bash
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-order
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-order-masked

kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-promotion
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-promotion-uppercase

kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-pattern-one
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-reward-one
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-storage-one

kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-pattern-two-plastic
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-pattern-two-notplastic
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-reward-two
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-storage-two

kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-pattern-three-plastic
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-pattern-three-notplastic
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-reward-three
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-storage-three

kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-pattern-four-plastic
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-pattern-four-notplastic
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-reward-four
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-storage-four

kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-pattern-five-plastic
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-pattern-five-notplastic
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-reward-five
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-storage-five
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-fraud-five

kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-pattern-six-plastic
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-pattern-six-notplastic
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-reward-six
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-storage-six
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-fraud-six

kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-feedback
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-feedback-one-good 

kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-feedback-two-good 

kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-feedback-three-good 
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-feedback-three-bad 

kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-feedback-four-good 
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-feedback-four-bad 
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-feedback-four-good-count
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-feedback-four-bad-count

kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-feedback-five-good 
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-feedback-five-bad 
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-feedback-five-good-count
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-feedback-five-bad-count

kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-feedback-six-good 
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-feedback-six-bad 
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-feedback-six-good-count
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-feedback-six-bad-count
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-feedback-six-good-count-word
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-feedback-six-bad-count-word

kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-flashsale-vote
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-flashsale-vote-user-item --config "cleanup.policy=compact" --config "delete.retention.ms=2000"  --config "segment.ms=2000" --config "min.cleanable.dirty.ratio=0.01" --config "min.compaction.lag.ms=2000"
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-flashsale-vote-one-result
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-flashsale-vote-two-result

kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-feedback-rating-one
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-feedback-rating-two

kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-customer-purchase-web
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-customer-purchase-mobile
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-customer-purchase-all  

kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-customer-preference-shopping-cart
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-customer-preference-wishlist
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-customer-preference-all  

  
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-inventory
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-inventory-total-one
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-inventory-total-two
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-inventory-total-three
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-inventory-four
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-inventory-five
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-inventory-six

kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-online-order
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-online-payment
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-join-order-payment-one
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-join-order-payment-two
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-join-order-payment-three

kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-web-vote-color
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-web-vote-layout
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-web-vote-one-username-color --config "cleanup.policy=compact" --config "delete.retention.ms=2000"  --config "segment.ms=2000" --config "min.cleanable.dirty.ratio=0.01" --config "min.compaction.lag.ms=2000"
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-web-vote-one-username-layout --config "cleanup.policy=compact" --config "delete.retention.ms=2000"  --config "segment.ms=2000" --config "min.cleanable.dirty.ratio=0.01" --config "min.compaction.lag.ms=2000"
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-web-vote-one-result
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-web-vote-two-username-color --config "cleanup.policy=compact" --config "delete.retention.ms=2000"  --config "segment.ms=2000" --config "min.cleanable.dirty.ratio=0.01" --config "min.compaction.lag.ms=2000"
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-web-vote-two-username-layout --config "cleanup.policy=compact" --config "delete.retention.ms=2000"  --config "segment.ms=2000" --config "min.cleanable.dirty.ratio=0.01" --config "min.compaction.lag.ms=2000"
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-web-vote-two-result
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-web-vote-three-username-color --config "cleanup.policy=compact" --config "delete.retention.ms=2000"  --config "segment.ms=2000" --config "min.cleanable.dirty.ratio=0.01" --config "min.compaction.lag.ms=2000"
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-web-vote-three-username-layout --config "cleanup.policy=compact" --config "delete.retention.ms=2000"  --config "segment.ms=2000" --config "min.cleanable.dirty.ratio=0.01" --config "min.compaction.lag.ms=2000"
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-web-vote-three-result
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-web-vote-four-result

kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-premium-purchase
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-premium-user --config "cleanup.policy=compact" --config "delete.retention.ms=2000"  --config "segment.ms=2000" --config "min.cleanable.dirty.ratio=0.01" --config "min.compaction.lag.ms=2000"
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-premium-offer-one
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-premium-offer-two
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-premium-offer-three

kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 5 --replication-factor 1 --topic t-commodity-subscription-purchase
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 2 --replication-factor 1 --topic t-commodity-subscription-user --config "cleanup.policy=compact" --config "delete.retention.ms=2000" --config "segment.ms=2000" --config "min.cleanable.dirty.ratio=0.01" --config "min.compaction.lag.ms=2000"
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-subscription-offer-one
kafka-topics.sh --bootstrap-server localhost:9092 --create --partitions 1 --replication-factor 1 --topic t-commodity-subscription-offer-two
```
</details>
<br/>

{::options parse_block_html="false" /}

----

## Kafka Stream Config

See [here](https://kafka.apache.org/documentation/#streamsconfigs) for complete reference

----

## Kafka Console Consumer Scripts

{::options parse_block_html="true" /}

<details><summary markdown="span">Click to expand!</summary>
Go to kafka console.  
If you are using docker, type `docker exec -it [kafka-container-name] bash`  
For container name, you can get it by typing `docker ps`

```bash
# Consumer - Masked Credit Card Stream  
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic t-commodity-order-masked

  
# Consumer - CommodityOne Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic t-commodity-order
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-order-masked
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-pattern-one
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-reward-one
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-storage-one


# Consumer - CommodityTwo Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic t-commodity-order
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-pattern-two-plastic
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-pattern-two-notplastic
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-reward-two
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-storage-two

  
# Consumer - CommodityThree Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic t-commodity-order
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-pattern-three-plastic
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-pattern-three-notplastic
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-reward-three
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-storage-three


# Consumer - CommodityFour Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic t-commodity-order
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-pattern-four-plastic
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-pattern-four-notplastic
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-reward-four
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-storage-four


# Consumer - CommoditySix Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic t-commodity-order
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-pattern-six-plastic
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-pattern-six-notplastic
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-reward-six
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-storage-six
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --property value.deserializer=org.apache.kafka.common.serialization.IntegerDeserializer --topic t-commodity-fraud-six


# Consumer - FeedbackOne Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-feedback-one-good


# Consumer - FeedbackTwo Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-feedback-two-good


# Consumer - FeedbackThree Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-feedback-three-good
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-feedback-three-bad


# Consumer - FeedbackFour Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-feedback-four-good
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-feedback-four-bad
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer --topic t-commodity-feedback-four-good-count
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer --topic t-commodity-feedback-four-bad-count


# Consumer - FeedbackFive Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-feedback-five-good
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-feedback-five-bad
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer --topic t-commodity-feedback-five-good-count
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer --topic t-commodity-feedback-five-bad-count


# Consumer - FeedbackSix Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-feedback-six-good
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-feedback-six-bad
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer --topic t-commodity-feedback-six-good-count
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer --topic t-commodity-feedback-six-bad-count
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer --topic t-commodity-feedback-six-good-count-word
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer --topic t-commodity-feedback-six-bad-count-word

  
# Consumer - CustomerPurchase Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-customer-purchase-all

  
# Consumer - CustomerPreference Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-customer-preference-all


# Consumer - FlashSaleVoteOne Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-flashsale-vote
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-flashsale-vote-user-item
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer --topic t-commodity-flashsale-vote-one-result


# Consumer - FlashSaleVoteTwo Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-flashsale-vote
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-flashsale-vote-user-item
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer --topic t-commodity-flashsale-vote-two-result


# Consumer - FeedbackRatingOne Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-feedback-rating-one


# Consumer - FeedbackRatingTwo Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-feedback-rating-two


# Consumer - InventoryOne Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer --topic t-commodity-inventory-total-one


# Consumer - InventoryTwo Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer --topic t-commodity-inventory-total-two


# Consumer - InventoryThree Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --property value.deserializer=org.apache.kafka.common.serialization.LongDeserializer --topic t-commodity-inventory-total-three


# Consumer - InventoryFour Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --property print.timestamp=true --topic t-commodity-inventory-four


# Consumer - InventoryFive Stream
# No console consumer, using print


# Consumer - InventorySix Stream
# No console consumer, using print


# Consumer - OnlineOrderPaymentOne Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --property print.timestamp=true --topic t-commodity-online-order
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --property print.timestamp=true --topic t-commodity-online-payment
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --property print.timestamp=true --topic t-commodity-join-order-payment-one


# Consumer - OnlineOrderPaymentTwo Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --property print.timestamp=true --topic t-commodity-join-order-payment-two


# Consumer - OnlineOrderPaymentThree Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --property print.timestamp=true --topic t-commodity-join-order-payment-three


#Consumer - WebVoteOne Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --property print.timestamp=true --topic t-commodity-web-vote-one-result


#Consumer - WebVoteTwo Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --property print.timestamp=true --topic t-commodity-web-vote-two-result


#Consumer - WebVoteThree Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --property print.timestamp=true --topic t-commodity-web-vote-three-result


#Consumer - WebVoteFour Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --property print.timestamp=true --topic t-commodity-web-vote-four-result


#Consumer - PremiumOfferOne Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-premium-offer-one


#Consumer - PremiumOfferTwo Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-premium-offer-two


#Consumer - PremiumOfferThree Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-premium-offer-three
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-premium-user-filtered


#Consumer - SubscriptionOfferOne Stream
# None


#Consumer - SubscriptionOfferTwo Stream
kafka-console-consumer.sh --bootstrap-server localhost:9092 --property print.key=true --topic t-commodity-subscription-offer-two
```
</details>
<br/>

{::options parse_block_html="false" /}

----

## Inventory - Timestamp Extractor
  - [Reference to built-in timestamp extractor](https://kafka.apache.org/documentation/#streamsconfigs_default.timestamp.extractor)

----

[Back to index](/spring-kafka-bootcamp)
