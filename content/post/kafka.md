---
title: "Kafka"
date: 2024-06-16T20:14:01+05:30
draft: false
---

# Apache Kafka

Apache Kafka is an open-source distributed event streaming platform. It has a publish-subscribe messaging system which allows exchanging of data between applications, servers and processors as well. Kafka is written in Java and Scala. It is scalable and fault-tolerant and has topic-based partition FIFO queues.

## Why Apache Kafka
1. **High Throughput**: Deliver messages at network limited throughput using a cluster of machines with latencies as low as 2ms.
2. **Scalable**: Scale production clusters up to a thousand brokers, trillions of messages per day, petabytes of data, hundreds of thousands of partitions. Elastically expand and contract storage and processing.
3. **Permanent Storage**: Store streams of data safely in distributed, durable, fault-tolerant cluster.
4. **High Availability**: Stretch clusters efficiently over availability zones or connect separate clusters across geographic regions.
5. **Built-in Stream Processing**: Process streams of events with joins, aggregations, filters, transformations, and more, using event-time and exactly-once processing.
6. **Connect to almost anything**: Kafka’s out-of-the-box Connect interface integrates with hundreds of event sources and event sinks including Postgres, JMS, Elasticsearch, AWS S3, and more.
8. Resilient architecturve which has resolved unusual complications in data sharing.

## What is event streaming?
Event streaming is the process of capturing data in real-time from event sources like databases, sensors, mobile devices etc. in the form of stream of events, sotring these events durably for later retrieval. Kafka allows manipulating, processing and reacting to the event streams in real-time as well as retrospectively, routing the event streams to different destination technologies as needed. It ensures a continuous flow and interpretation of data so that the right info is at the right place at the right time.