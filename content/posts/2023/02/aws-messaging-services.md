---
title: "AWS Messaging Services"
date: 2023-02-16T19:21:00+01:00
draft: false
summary: Comparison of messaging services available in AWS.
---

![A ship in the sea](/2023/02/unsplash-envelopes.jpg "Envelopes we all want to receive")

Hello world!

In this post I'm exploring the various AWS services for services massaging. By this, I mean services that allow the different parts of our application (aka: services) to communicate with each other.

## Why do we need them?

Those messaging solutions are very helpful for many reasons:

- **Decoupling of applications**: Messaging systems are often used to decouple different components of an application, which makes them more resilient and scalable. By using a messaging system like AWS SQS or SNS, each component can operate independently, and if one component fails, it will not bring down the entire application;
- **Real-time processing**: Messaging systems can handle large volumes of data in real-time. This can be useful for use cases such as financial trading, social media, e-commerce and IoT applications;
- **Asynchronous processing**: Messaging systems are asynchronous, which means they can process messages independently of other services. This can be useful for batch processing, long-running jobs, and other scenarios where processing time is not critical;
- **Scalability**: Messaging systems can be scaled horizontally, which means they can handle large amounts of traffic without being affected by bottlenecks or SPOF (single points of failure). This can be useful for applications that need to handle high volumes of traffic or have unpredictable traffic patterns;
- **Event-driven architectures**: Messaging systems like AWS EventBridge are used to build event-driven architectures, which enable applications to respond to events in real-time. This can be useful for use cases such as serverless computing, microservices, and event-driven applications;

> tl;dr: Messaging systems provide a reliable, scalable, and flexible way to handle different types of communication and data processing needs in modern cloud applications.

Now that you have a good understading of why you might need a messaging system, let's have a quick look at whet AWS offers as messaging services:

## AWS messaging services

In this list, I do not take into account services that target humans, like **Amazon Pinpoint**, which is a fully managed service that enables sending targeted messages to mobile app users. Also I'll not talk about **AWS SES** which is AWS solution to send emails.

I'll only focus on services that make applications talk to each other.

### Amazon SNS

[Amazon Simple Notification Service (SNS)](https://aws.amazon.com/sns/) is a fully managed pub/sub messaging service that enables message delivery to multiple recipients or subscribers. It supports a wide range of messaging protocols such as HTTP, email, SMS, and mobile push notifications.

Amazon SNS is based on the **publish/subscribe (pub/sub) messaging paradigm**, which allows for decoupling of message producers and consumers (known as subscribers). Subscribers subscribe to a topic, and can all together get any message published by a publisher that sends something in this topic.

![How SNS works](/2023/02/Product-Page-Diagram_Amazon-SNS_Event-Driven-SNS-Compute.png)

Some of the key building blocks of Amazon SNS are:

- Topics: Topics are logical access points that allow publishers to send messages to one or many subscribers. Publishers can send messages to a topic, and subscribers can receive messages from a topic.
- Subscribers: Subscribers are endpoints that receive messages published to a topic. Subscribers can be AWS services, HTTP(S) endpoints, email addresses, mobile devices, and more.
- Publishers: Publishers are applications or services that send messages to a topic. Publishers can be AWS services, mobile apps, web apps, and more.

Amazon SNS is a flexible and scalable messaging service that can be used for a variety of use cases. It can be a good choice for applications that require real-time message delivery, fanout scenarios, or decoupling of components. It can also be useful for applications that require multi-region support or high scalability:

- Application notifications: Which can be useful for alerting users about new content, changes to their account, or other important events.
- Fanout scenarios to multiple subscribers: This can be useful for distributing messages to multiple endpoints, such as for real-time data processing or chat applications.
- Scalability: This can be useful for applications that need to handle unpredictable traffic patterns or bursty workloads.
- Decoupling of components: This makes different components of an application more resilient and scalable. By using Amazon SNS, each component can operate independently, and if one component fails, it will not bring down the entire application.
- Multi-region support: Finally, Amazon SNS supports message delivery to multiple regions, which can be useful for building applications that require high availability and disaster recovery capabilities.

### Amazon SQS

[Amazon Simple Queue Service (SQS)](https://aws.amazon.com/sqs/) is a fully managed message queuing service that enables decoupling of the components of a cloud application. It provides reliable, highly available, and scalable queuing of messages.

Unlike Amazon SNS, which is base on the pub/sub paradigm, Amazon SQS is based on the **queueing paradigm**, which allows for reliable and scalable messaging between producers and consumers through waiting queues.

![How SQS works](/2023/02/Product-page-diagram_Amazon-SQS.png)

Building blocks of Amazon SQS are:

- Queues: Queues are _named buffers_ that hold messages sent by producers. Queues can be used to decouple producers and consumers, and to provide reliable and scalable message delivery.
- Messages: Messages are payloads sent by producers to queues. Messages can contain any type of data, including text, binary data, or metadata.
- Producers: Producers are applications or services that send messages to a queue. Producers can be AWS services, mobile apps, web apps, and more.
- Consumers: Consumers are applications or services that receive and process messages from a queue. Consumers can be AWS services, EC2 instances, serverless functions,... etc

AWS SQS can be a good choice for applications that require asynchronous processing, load leveling, scaling, or reliability. It can also be useful for applications that require multi-region support or high scalability. Use cases include:

- Asynchronous processing: Amazon SQS can be used to decouple different components of an application, which allows for asynchronous processing of messages. This can be useful for long-running tasks, batch processing, or any scenario where processing time is not critical.
- Load leveling: This can be used to smooth out spikes in traffic by temporarily storing messages in a queue until they can be processed by a consumer. This can be useful for applications that have unpredictable traffic patterns or bursty workloads.
- Scaling: The horizontal scalability of Amazon SQS can be useful for applications that need to handle high volumes of traffic or have unpredictable traffic patterns.
- Reliability: Amazon SQS provides reliable message delivery by storing messages in a queue until they are processed by a consumer. This can be useful for applications that require _guaranteed delivery of messages_.
- Multi-region support: Like Amazon SNS, deliverability to multiple regions by Amazon SQS can be useful for building applications that require high availability and disaster recovery capabilities.

### Amazon MQ

[Amazon MQ](https://aws.amazon.com/amazon-mq/) is a managed message broker service that provides support for a variety of messaging protocols, including AMQP, MQTT, STOMP, OpenWire, and WebSocket. Amazon MQ is based on the **message broker paradigm**, which allows for reliable and secure messaging between producers and consumers. Here are some of the key building blocks of Amazon MQ:

![How Amazon MQ works](/2023/02/Product-Page-Diagram_Amazon-MQ.png)

- Brokers: They are message servers that handle the storage, routing, and delivery of messages. Amazon MQ supports both ActiveMQ and RabbitMQ brokers.
- Queues: They are named buffers that hold messages sent by producers. Queues can be used to decouple producers and consumers, and to provide reliable and scalable message delivery.
- Topics: They are logical access points that allow publishers to send messages to one or many subscribers. Publishers can send messages to a topic, and subscribers can receive messages from a topic.
- Producers: They are applications or services that send messages to a broker. Producers can use a variety of messaging protocols to communicate with a broker, including AMQP, MQTT, STOMP, OpenWire, and WebSocket.
- Consumers: They are applications or services that receive and process messages from a broker. Consumers can use a variety of messaging protocols to communicate with a broker, including AMQP, MQTT, STOMP, OpenWire, and WebSocket.

Amazon MQ is a flexible and scalable message broker service that can be used for a variety of use cases. It can be a good choice for applications that require reliable and secure messaging, support for different messaging protocols, and scalability. It can also be useful for applications that require messaging between on-premises and cloud-based environments or for IoT messaging.

In more details, use cases are:

- Enterprise messaging: Amazon MQ can be used to provide reliable and secure messaging between different components or services in an enterprise environment: Applications that require high availability, scalability, and security.
- IoT messaging: Amazon MQ is very good for IoT devices and applications.
- Cloud messaging: Amazon MQ can be used to provide messaging for cloud-based applications and services.
- Hybrid messaging: Amazon MQ can be used to provide messaging between on-premises and cloud-based applications and services. This can be useful for scenarios where applications and services need to communicate over a secure and reliable messaging protocol across different environments.
- Protocol support: Finally, Amazon MQ supports a variety of messaging protocols, which can be useful for applications and services that require specific messaging protocols or need to communicate with legacy systems.

### Amazon MSK

With [Amazon Managed Streaming for Apache Kafka (Amazon MSK)](https://aws.amazon.com/msk/), we can easily create, configure, and manage Apache Kafka clusters in the AWS cloud. We can also use Amazon MSK to integrate Kafka with other AWS services such as Amazon S3, Amazon Kinesis, and Amazon Elasticsearch Service.

![How Amazon MSK works](/2023/02/Product-page-diagram_MSK-How-It-Works.png)

Key concepts in MSK are:

- Clusters: A cluster is a set of broker nodes that run Apache Kafka. MSK automatically manages the configuration and deployment of Kafka brokers within a cluster, making it easy to scale and operate.
- Topics: A topic is a category or feed name to which messages are published. Kafka organizes messages into topics, and each message published to a topic is delivered to all consumers that subscribe to that topic.
- Partitions: A partition is a unit of parallelism in Kafka. _Each partition is an ordered, immutable sequence of messages that can be consumed by one or more consumers_.
- Producers: A producer is an application that publishes messages to a Kafka topic.
- Consumers: Finally, a consumer is an application that reads messages from one or more Kafka topics.

Amazon MSK can be used for a variety of use cases, such as real-time data processing, data streaming, and event-driven architectures:

- Real-time streaming data processing: Amazon MSK is ideal for processing large amounts of streaming data in real-time, such as log data, sensor data, and other time-series data.
- Event-driven architecture: Amazon MSK can be used to build event-driven applications that respond to events in real-time, such as customer transactions, user clicks, and other user events.
- Hybrid cloud integration: Amazon MSK can be used to integrate applications running on-premises with applications running in the cloud, making it easy to build hybrid cloud architectures.
- Data streaming for machine learning: Amazon MSK can be used to stream data for machine learning models, such as training data or real-time predictions.

The next two services are using Kafka under the hood...

### Amazon Kinesis Data Streams

[Amazon Kinesis Data Streams](https://aws.amazon.com/kinesis/data-streams/) is a fully managed service that enables real-time processing of streaming data at scale. It provides durable and scalable message streaming and can be used to build custom data processing applications.

It is based on the **streaming data paradigm**, which allows for real-time data ingestion and processing of high-throughput data streams.

![How Amazon Kinesis Data Streams works](/2023/02/Product-Page-Diagram_Amazon-Kinesis-Data-Streams.png)

Key building blocks are:

- Streams: Which are ordered sequences of records that producers send to Kinesis Data Streams. Streams can be used to store and process large volumes of data in real time.
- Shards: Which are partitions within a stream that enable parallel processing of data. Each shard has a fixed capacity for data storage and a maximum rate of data ingestion.
- Producers: Which are applications or services that send records to a stream. Producers can be AWS services, mobile apps, web apps, and more.
- Consumers: Which are applications or services that read and process records from a stream. Consumers can be AWS services like EC2 instances, serverless functions, and more.

Use cases for Amazon Kinesis Data Streams are:

- Real-time analytics: Kinesis Data Streams can be used to process and analyze large volumes of streaming data in real time. This can be useful for applications that require real-time insights into data, such as fraud detection, anomaly detection, and more.
- Data processing: Kinesis Data Streams can be used to process and transform data in real time. This can be useful for applications that require real-time data processing, such as ETL (extract, transform, load) processes, data normalization, and more.
- Log and event processing: Kinesis Data Streams can be used to process logs and events in real time. This can be useful for applications that require real-time analysis of logs and events, such as security monitoring, system monitoring, and more.
- Streaming data ingestion: Kinesis Data Streams can be used to ingest large volumes of streaming data in real time. This can be useful for applications that require real-time data ingestion, such as IoT data, clickstream data, and more.
- Scaling: And finally, Kinesis Data Streams can scale horizontally to handle large volumes of data and traffic, which can be useful for applications that need to handle high volumes of traffic or have unpredictable traffic patterns.

Overall, Amazon Kinesis Data Streams is a fantastic service that is highly scalable and flexible streaming data service. It can be a good choice for applications that require real-time processing and analysis of streaming data, as well as applications that need to ingest and process large volumes of streaming data. This is perfect for applications that require high scalability and have unpredictable traffic patterns.

### Amazon Kinesis Data Firehose

[Amazon Kinesis Data Firehose](https://aws.amazon.com/kinesis/data-firehose/) is a fully managed service that enables loading streaming data into data stores or analytics tools such as Amazon S3, Amazon Redshift, Amazon Elasticsearch Service, and Splunk.

![How Amazon Kinesis Data Firehose works](/2023/02/Product-page-diagram_Amazon-KDF_HIW-V2-Updated-Diagram.png)

Key building blocks are:

- Delivery streams: Which are the main building block of Amazon Kinesis Data Firehose. They are used to specify the destination where data should be delivered. Delivery streams can be configured to deliver data to Amazon S3, Amazon Redshift, Amazon Elasticsearch Service, and Splunk.
- Producers: Which are applications or services that send data to the delivery stream. Producers can be AWS services, mobile apps, web apps, and more.
- Data transformation: Amazon Kinesis Data Firehose can transform the data before delivering it to the destination. _The transformation can be done using AWS Lambda or AWS Glue_.

Use cases include:

- Data lake: Kinesis Data Firehose can be used to deliver streaming data to Amazon S3, which can act as a data lake. This can be useful for applications that require storage of large volumes of data for further analysis and processing.
- Business intelligence: Amazon Kinesis Data Firehose can be used to deliver streaming data to Amazon Redshift, which can be used for business intelligence applications. This can be useful for applications that require real-time insights into data, such as sales trends, customer behavior, ...etc.
- Search and analytics: Which is done by treaming data to Amazon Elasticsearch Service, which can be used for search and analytics applications. This can be useful for applications that require real-time analysis of data, such as log analysis, and security monitoring.
- Operational intelligence: Finally, Amazon Kinesis Data Firehose can be used to deliver streaming data to Splunk, which can be used for operational intelligence applications. This can be useful for applications that require real-time analysis of data, such as system monitoring, and application performance monitoring.

#### Differences between Amazon Kinesis Data Streams and Kinesis Data Firehose

Both of them are AWS services that are used for processing and delivering streaming data. However they have many differences:

- Data transformation: Kinesis Data Firehose can transform the data before delivering it to the destination. The transformation can be done using AWS Lambda or AWS Glue. In contrast, Kinesis Data Streams does not natively support data transformation. Data transformation must be done by the application or service that reads the data from the Kinesis Data Stream.
- Data delivery: Kinesis Data Firehose delivers data to destinations such as Amazon S3, Amazon Redshift, Amazon Elasticsearch Service, and Splunk in near real-time. In contrast, Kinesis Data Streams does not natively deliver data to destinations. Data must be read from the stream by an application or service, and then delivered to the destination.
- Data persistence: Kinesis Data Streams stores the streaming data for a configurable retention period, allowing applications to replay and process the data multiple times. In contrast, Kinesis Data Firehose does not store the data after it has been delivered to the destination.
- Data ordering: Kinesis Data Streams guarantees the ordering of data within a partition. In contrast, Kinesis Data Firehose does not guarantee the ordering of data, since it can buffer and deliver data in batches.
- Cost: Kinesis Data Firehose is a fully managed service that charges based on the volume of data delivered to the destination. In contrast, Kinesis Data Streams charges based on the amount of data ingested, as well as the number of shards used for the stream.

To summarize the comparison, Kinesis Data Streams is a more flexible service that provides finer-grained control over the data processing pipeline. It is suitable for applications that require real-time processing and analysis of streaming data, as well as applications that require data persistence and ordering guarantees.

On the other hand, Kinesis Data Firehose is a more managed service that simplifies the data delivery process. It is suitable for applications that require near real-time delivery of streaming data to a destination, as well as applications that require data transformation before delivery.

### Amazon EventBridge

[Amazon EventBridge](https://aws.amazon.com/eventbridge/) is a fully managed event bus service that enables building event-driven architectures. It provides a serverless event bus that can route events between AWS services, SaaS applications, and on-premises applications.

![How Amazon EventBridge works](/2023/02/Product-Page-Diagram_Amazon-EventBridge.png)

The key building blocks of Amazon EventBridge are:

- Event buses: Are the main building block of Amazon EventBridge. They are used to collect and route events from different sources to different targets. Event buses can be created for different applications or teams, and can be used to separate and organize events.
- Events: Are the data generated by various sources such as AWS services, custom applications, or third-party services. Events can be used to trigger actions or workflows based on specific conditions.
- Rules: Are used to match events from different sources to specific targets. Rules can be based on _event patterns, event content, or custom logic_.
- Targets: Are the endpoints where the events are sent for processing. Targets can be AWS services, custom applications, or third-party services.

Some use cases for Amazon EventBridge include:

- Serverless applications: Amazon EventBridge can be used to build serverless applications that react to events from various sources. For example, you can use EventBridge to trigger an AWS Lambda function when a file is uploaded to Amazon S3.
- Event-driven architecture: Amazon EventBridge can be used to build event-driven architectures that connect different services and applications. This can be useful for applications that require real-time integration of data from different sources.
- Automation: Amazon EventBridge can be used to automate workflows and processes based on events from different sources. For example, you can use EventBridge to trigger an AWS Step Functions workflow when an event occurs.
- Security and compliance: Amazon EventBridge can be used to monitor and respond to security events and compliance violations. This can be useful for applications that require real-time monitoring and response to security threats.

Amazon EventBridge is a highly scalable and flexible service that can be used to connect applications and services using data from different sources. It can be a good choice for applications that require real-time integration of data from different sources, as well as applications that require automation of workflows and processes based on specific events.

## Summary

| Service | Key Features | Use Cases |
|---------|--------------|-----------|
| Amazon SNS | Publish/subscribe messaging model, supports multiple protocols, high throughput, topic-based filtering, mobile push notifications | Fan-out broadcasts, real-time notifications, serverless architectures |
| Amazon SQS | Queue-based messaging model, supports multiple message types, scalable, high availability, dead-letter queues, visibility timeout | Decoupling application components, asynchronous processing, message retries, event-driven architecture |
| Amazon MQ | Message broker based on Apache ActiveMQ, supports multiple messaging protocols, high availability, message persistence, message filtering | Migration of existing applications to the cloud, integration of on-premises applications with AWS |
| Amazon MSK | Managed Apache Kafka service, supports multiple protocols, high throughput, data persistence, data replication, configurable retention | Real-time data streaming, data processing, event-driven architecture |
| Amazon Kinesis Data Streams | Streaming data service, real-time data processing, scalable, high throughput, data partitioning, data retention | Real-time analytics, machine learning, IoT data ingestion, log processing |
| Amazon Kinesis Data Firehose | Delivery service for streaming data, data transformation, delivery to multiple destinations, near real-time delivery, fully managed | Near real-time data delivery to Amazon S3, Amazon Redshift, and other destinations, data transformation before delivery |
| Amazon EventBridge | Serverless event bus, event-driven architecture, multiple event sources and targets, rules engine, customizable | Automation, real-time monitoring, serverless applications, security and compliance monitoring |

## Take away

AWS offers a variety of messaging services, each with its own unique features and use cases. Choosing the right messaging service depends on our specific needs and requirements:

If we need a publish/subscribe messaging model and want to broadcast messages to multiple subscribers, Amazon SNS is a good option.

If we need a queue-based messaging model for decoupling application components or asynchronous processing, Amazon SQS is a good choice.

For migration of existing applications to the cloud or integration with on-premises applications, Amazon MQ is a good option.

If we need a managed Apache Kafka service for real-time data streaming and processing, Amazon MSK is the path to go.

If we need a streaming data service for real-time analytics, machine learning, IoT data ingestion, or log processing, Amazon Kinesis Data Streams and Kinesis Data Firehose are both good options, with Kinesis Data Streams providing more control over data processing and partitioning.

Finally, if we're building event-driven architectures and need a customizable, serverless event bus, Amazon EventBridge is a good choice.

Overall, each messaging service has its own strengths and weaknesses.

---

Cheers,

Hassen
