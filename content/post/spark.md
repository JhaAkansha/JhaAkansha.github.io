---
title: "Spark"
date: 2024-06-12T13:13:17+05:30
---

# Apache Spark

Apache Spark is an open-source unified analytics engine designed for large-scale data processing. It provides an interface for programming entire clusters with built-in data parallelism and fault tolerance.

Built on top of the Hadoop MapReduce framework, Spark enhances it by supporting a wider range of computations, including interactive queries and stream processing. It features its own cluster management system, but can also utilize Hadoop for storage and processing.

Spark executes applications much faster—especially in-memory—by caching intermediate data, reducing disk I/O overhead. It offers built-in APIs for Java, Python, and Scala, and supports SQL queries, real-time streaming, machine learning, and graph computation.

---
## What is Spark Streaming?

Spark Streaming is a component of Spark that enables scalable, high-throughput, fault-tolerant stream processing of live data streams.

- Leverages Spark Core’s fast scheduling capability.
- Ingests data in **mini-batches**.
- Applies **RDD transformations** to these mini-batches.

---

## Architecture of Spark

Apache Spark follows a **master-slave architecture**, consisting of a single master node and multiple slave (worker) nodes. It is designed around two key abstractions:

1. **RDD (Resilient Distributed Dataset)** — A distributed collection of data that can be processed in parallel.
2. **DAG (Directed Acyclic Graph)** — Represents a logical execution plan composed of transformations.

---

### DAG (Directed Acyclic Graph)

The DAG is a directed graph where:

- Each **node** is an RDD partition.
- Each **edge** represents a transformation on the data.

The DAG execution model is composed of:

1. **Driver Program**: The process that runs the `main()` function and creates the `SparkContext` object.
2. **Cluster Manager**: Allocates resources across applications (e.g., YARN, Mesos, or Spark's built-in manager).
3. **Worker Nodes**: Execute application code assigned by the cluster manager.
4. **Executor**: A process launched on a worker node to run tasks and store data in memory or disk. Each Spark application has its own executors.
5. **Task**: A unit of work that is sent to one executor.

---

### RDD (Resilient Distributed Dataset)

The **RDD** is the foundational data structure in Spark. It is:

- **Immutable** and **distributed**.
- A collection of records stored across multiple nodes.
- Able to contain any type of object: Python, Java, Scala, or even user-defined types.

#### Key Characteristics

- **Partitioned** across the cluster.
- **Read-only** collections.
- **Fault-tolerant** using lineage (recomputation).
- Stored **in-memory** when possible; otherwise spilled to disk.
- **Checkpointing** is used to save RDDs to stable storage to prevent recomputation on node failure.

#### Types of RDDs

1. **Parallelized Collections**  
   Created by invoking `SparkContext.parallelize()` method.

2. **Hadoop Datasets**  
   Created from external storage systems like HDFS, S3, etc.

---

### RDD Operations

RDDs support two kinds of operations:

1. **Transformations**  
   - Lazily evaluated.
   - Create a new RDD from an existing one.
   - Examples: `map()`, `filter()`, `flatMap()`

2. **Actions**  
   - Trigger execution of RDD transformations.
   - Return values to the driver or write to external storage.
   - Examples: `collect()`, `count()`, `saveAsTextFile()`

---

Apache Spark combines the flexibility of functional programming with the power of distributed computing, making it a go-to engine for big data analytics, real-time processing, and complex workflows.

