# Programming Model: Annotation-based 👉 Functional

Version 1: https://docs.spring.io/spring-cloud-stream/docs/1.0.0.RC2/reference/html/_programming_model.html
* Bound interfaces
* Bound channels

Version 2: https://docs.spring.io/spring-cloud-stream/docs/Elmhurst.RELEASE/reference/html/_programming_model.html 
* bindings
* bindable components - MessageChannel for outbound and SubscribableChannel for inbound (PollableMessageSource for a pollable consumer)
* @EnableBinding, @Qualifier, @StreamListener

Version 3: https://docs.spring.io/spring-cloud-stream/docs/3.1.3/reference/html/spring-cloud-stream.html#_programming_model
* First class support for fully functional programming model

# Streaming Processing with Apache Kafka Streams
https://spring.io/blog/2019/12/09/stream-processing-with-spring-cloud-stream-and-apache-kafka-streams-part-6-state-stores-and-interactive-queries

# Issues on Functional Programming using latest Kafka Streams binder v3.1.2
1. Could not start stream: 'listener' cannot be null - https://stackoverflow.com/questions/66881210/spring-cloud-stream-kafka-streams-binder-kafkaexception-could-not-start-stream
2. After solving the above one, a new one appears: "The following method did not exist: org.springframework.cloud.stream.binder.kafka.streams.function.KafkaStreamsBindableProxyFactory.populateBindingTargetFactories" - which is true for the latest v3.1.2 although the method is (committed on Apr 22, 2021) here at https://github.com/spring-cloud/spring-cloud-stream/blob/b2bb645cf1c841f0a2dfdc540c1949aaa638fe34/spring-cloud-stream/src/main/java/org/springframework/cloud/stream/binding/AbstractBindableProxyFactory.java, which seems to be included in v3.1.3 in the future
3. Spring Cloud Stream v3.1.3 released on May 26, 2021, which resolved the above issues.👍🎉😊


# Apache Kafka Streams
## From Apache Kafka
1. [Kafka Streams](https://kafka.apache.org/documentation/streams/) 
    ```
    a client library for processing and analyzing data stored in Kafka
    ```
2. Stream
    * represents an unbounded, continuously updating data set
    * is an ordered, replayable, and fault-tolerant sequence of immutable data records (key-value pair)
3. Stream Processing Application
    * is any program that makes use of the Kafka Streams library
    * defines its computational logic through one or more processor topologies
4. Processor Topology
    * a graph of stream processors (nodes) that are connected by streams (edges)
    * two ways to define
      * [Streams DSL](https://kafka.apache.org/28/documentation/streams/developer-guide/dsl-api.html) - provides the most common data transformation operations
        * stateless, like map, filter
        * stateful, like join, aggregations
      * [Processor API](https://kafka.apache.org/28/documentation/streams/developer-guide/processor-api.html) - allows developers to
        * define and connect custom processors
        * interact with [state stores](https://kafka.apache.org/28/documentation/streams/core-concepts#streams_state)
5. [Stream Processor](https://kafka.apache.org/28/documentation/streams/developer-guide/processor-api#defining-a-stream-processor) - a node in the processor topology
    * Source Processor - a special type of stream processor that does not have any upstream processors
    * Sink Processor - a special type of stream processor that does not have down-stream processors
6. Time - timestamp embedded automatically into Kafka message is Event time when Kafka's configuration of message.timestamp.type is CreateTime (default) or Ingestion time when LogAppendTime
    * Event time - when an event or data record occurred
    * Ingestion time -  when an event or data record is stored in a topic partition by a Kafka broker
    * Processing time - when the event or data record happens to be processed by the stream processing application 
    * Stream time - Kafka Streams assigns a timestamp to every data record when it arrives at the processor via the TimestampExtractor interface
    * Publish time - Kafka Streams assigns a timestamp to every new record when writing it to Kafka
7. Stream-Table Duality - Kafka Streams models it explicitly via the following three interfaces
    * [KStream](https://kafka.apache.org/28/documentation/streams/developer-guide/dsl-api#streams_concepts_kstream)
    * [KTable](https://kafka.apache.org/28/documentation/streams/developer-guide/dsl-api#streams_concepts_ktable)
    * [GlobalKTable](https://kafka.apache.org/28/documentation/streams/developer-guide/dsl-api#streams_concepts_globalktable)
8. Aggregations - An aggregation operation takes one input stream or table, and yields a new table by combining multiple input records into a single output record.
9. Windowing - control how to group records with the same key for stateful operations into so-called windows
    * Grace period - controls how long Kafka Streams will wait for out-of-order data records for a given window
10. States
    * State stores - used by stream processing applications to store and query data
    * Interactive queries - direct read-only queries of the state stores by methods, threads, processes or applications external to the stream processing application that created the state stores
11. Exactly-once Guarantee
    * For configuration in Spring Cloud Stream, see [here](https://stackoverflow.com/questions/54952918/spring-cloud-stream-vs-kafka-stream-for-exactly-once-feature)
12. Out-of-Order Handling
    * Wait for longer time while bookkeeping their states during the wait time, i.e. making trade-off decisions between latency, cost, and correctness
    * For Joins in Kafka Streams, some of the out-of-order data cannot be handled by increasing on latency and cost yet

## From Confluent
1. [Intro to Streams API](https://www.youtube.com/watch?v=Z3JKCLG3VP4) on YouTube
    * High-level declarative, functional API  - [Streams DSL](https://docs.confluent.io/platform/current/streams/developer-guide/dsl-api.html)
    * Low-level imperative API - [Processor API](https://docs.confluent.io/platform/current/streams/developer-guide/processor-api.html)
2. [Streams and Tables in Apache Kafka](https://www.confluent.io/blog/kafka-streams-tables-part-1-event-streaming/)
3. [Kafka Streams Interactive Queries](https://docs.confluent.io/platform/current/streams/developer-guide/interactive-queries.html#streams-developer-guide-interactive-queries)
