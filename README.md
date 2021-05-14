# Programming Model

Version 1: https://docs.spring.io/spring-cloud-stream/docs/1.0.0.RC2/reference/html/_programming_model.html
* Bound interfaces
* Bound channels

Version 2: https://docs.spring.io/spring-cloud-stream/docs/Elmhurst.RELEASE/reference/html/_programming_model.html 
* bindings
* bindable components - MessageChannel for outbound and SubscribableChannel for inbound (PollableMessageSource for a pollable consumer)
* @EnableBinding, @Qualifier, @StreamListener

Version 3: https://docs.spring.io/spring-cloud-stream/docs/current-snapshot/reference/html/_programming_model.html
* First class support for fully functional programming model

# Streaming Processing with Apache Kafka Streams
https://spring.io/blog/2019/12/09/stream-processing-with-spring-cloud-stream-and-apache-kafka-streams-part-6-state-stores-and-interactive-queries

# Issues on Functional Programming using latest Kafka Streams binder v3.1.2
1. Could not start stream: 'listener' cannot be null - https://stackoverflow.com/questions/66881210/spring-cloud-stream-kafka-streams-binder-kafkaexception-could-not-start-stream
2. After solving the above one, a new one appears: "The following method did not exist: org.springframework.cloud.stream.binder.kafka.streams.function.KafkaStreamsBindableProxyFactory.populateBindingTargetFactories" - which is true for the latest v3.1.2 although the method is (committed on Apr 22, 2021) here at https://github.com/spring-cloud/spring-cloud-stream/blob/b2bb645cf1c841f0a2dfdc540c1949aaa638fe34/spring-cloud-stream/src/main/java/org/springframework/cloud/stream/binding/AbstractBindableProxyFactory.java, which seems to be included in v3.1.3 in the future 


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


## From Confluent
1. [Intro to Streams API](https://www.youtube.com/watch?v=Z3JKCLG3VP4) on YouTube
    * High-level declarative, functional API  - [Streams DSL](https://docs.confluent.io/platform/current/streams/developer-guide/dsl-api.html)
    * Low-level imperative API - [Processor API](https://docs.confluent.io/platform/current/streams/developer-guide/processor-api.html)
2. [Streams and Tables in Apache Kafka](https://www.confluent.io/blog/kafka-streams-tables-part-1-event-streaming/)
3. [Kafka Streams Interactive Queries](https://docs.confluent.io/platform/current/streams/developer-guide/interactive-queries.html#streams-developer-guide-interactive-queries)
