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

## Architecture of Spark
* It has master-slave architecture
* Its cluster consists of a single master and multiple slaves
* It depends on two abstractions:
    1. RDD - It is a group os data items that can be stored in memory on worker nodes.
    2. DAG - Used to manage data flow. It is a finite directed graph that performs a sequence of computations on data.
    ### DAG
    Each node is an RDD partition and the edge is tranformation on top of data. It consists of:
    1. Driver Program - It is process that runs the main() function. It creates a SparkContext object.
    2. Cluster Manager - It allocates resources across applications
    3. Worker Nodes - These are slave nodes which run the application codes in clusters.
    4. Task - A unit of work that will be sent to one executor
    5. Executor - A process launched for an application on a worker node. It runs tasks and keeps data in memory on disk storage across them. It reads and writes data to the external sources. Every application contains its executor.

    ### RDD
    It is the fundamental structure of Spark. It is immutable, distributed collections of objects. It contains any type of Python, Java, Scala objects, including user-defined classes.
    