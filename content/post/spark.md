---
title: "Spark"
date: 2024-06-12T13:13:17+05:30
draft: false
---

Apache Spark is an open-source unified analytics engine for large-scale data processing. Spark provides an interface for programming clusters with implicit data parallelism and fault tolerance. It is built on top of Hadoop Map Reduce module. It extends MapReduce module to efficiently use more types of computation interactive queries and stream processing.
It has its own cluster management. It utilizes Hadoop in two ways: storage and processing. It helps run an application in a Hadoop cluster, multiple times faster in memory. It stores  intermediate processing data in memory which saves read/write operations. It has built-in APIs in Java, Python or Scala. It supports SQL queries, streaming data, machine learning and Graph algorithms.

## What is Spark Streaming?
* It leverages Spark Core's fast scheduling capability to perform streaming analytics.
* Ingests data in mini-batches.
* Reforms RDD (Resilient Distributed Datasets) transformations on it.

## Architecture
* It has master-slave architecture
* Its cluster consists of a single master and multiple slaves
* It depends on two abstractions:
    1. RDD
    2. DAG