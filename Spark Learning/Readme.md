# DataBricks Notes Summary

## Spark

Spark is an engine for performing data operations related to data engineering, machine learning, and other domains using a single node or multinode clusters.

### Key Features:
- **In-memory processing**
- **Lazy evaluation** (only on actions)
- **Fault-tolerant** (due to replications)
- **Immutable DataFrames**
- **Partitioning and persistence**

### Spark Context and Spark Session:
- **Spark Context**: Entry point for Spark to create RDD, dataset API, DataFrame, and more.
- **Spark Session**: Manages Spark context, streaming, SQL, and Hive Context. It allows multiple sessions within one Spark application, sharing underlying resources.

## Worker Node/Slave Node Characteristics:
- Worker nodes consist of containers (executors) that manage heap memory, off-heap memory, cores, and garbage collection.
- **Single-node clusters** do not have worker nodes; all jobs run on the driver node.

## Executor and Task:
- **Executor**: A process running on a worker node that is responsible for executing tasks and managing memory.
- **Task**: The smallest unit of work, executed in parallel by executors.

## On-Heap vs. Off-Heap Memory:
- **On-Heap Memory**: Used for caching RDDs, DataFrames, UDFs, and operations (e.g., joins, aggregations).
- **Off-Heap Memory**: Used when on-heap memory is insufficient, stored in serialized format for performance.

## Cluster-Based Architecture:
- Primarily used in production environments.
- The process involves YARN Resource Manager (RM) and Application Master (AM), with the driver managing the tasks.

## Client-Based Architecture:
- Used for development purposes.
- The driver is on the client machine, which poses risks like power failure.

## Executor and Driver Memory Allocation:
- **Executor memory**: Allocates memory based on the task requirements (e.g., 4GB for small tasks, 16GB for larger ones).
- **Driver memory**: Manages task scheduling, interactive queries (1-2GB for small tasks, 4-8GB for larger tasks).

## Wide and Narrow Transformations:
- **Narrow Transformations**: Do not require shuffling (e.g., `map`, `filter`).
- **Wide Transformations**: Require shuffling of data across partitions (e.g., `groupBy`, `join`).

## Apache Spark DAG (Directed Acyclic Graph):
- DAG represents the logical execution plan of a Spark application, describing the sequence of transformations and actions on RDDs/DataFrames.

## Slowly Changing Dimensions (SCDs):
- **Type 0**: Fixed Dimension (no changes allowed).
- **Type 1**: No history, overwrites old values.
- **Type 2**: Row versioning, keeps full history.
- **Type 3**: Previous value column, tracks limited history.
- **Type 4**: Separate history table.
- **Type 6**: Hybrid approach, combining Types 1, 2, and 3.

## Magic Commands in Databricks:
- Special commands prefixed with `%` or `%%` used for various notebook tasks (e.g., `%fs`, `%sql`).

## Delta Lake:
- Extends Apache Parquet, supporting ACID transactions, schema enforcement, and time travel. Optimized for both streaming and batch processing.

## Optimization Techniques:
- **Optimize**: Merges small files to reduce processing time and cost.
- **Z-Order**: Optimizes query performance by organizing data based on specific columns (e.g., `order_id`, `customer_id`).
- **Vacuum**: Removes unnecessary files from Delta Lake to save storage.

## Broadcast Join:
- Optimizes joins by broadcasting smaller datasets to each node to avoid shuffling.

## Partitioning and Bucketing:
- **Partitioning**: Divides data into smaller parts based on column values.
- **Bucketing**: Organizes data into a fixed number of buckets based on the hash of a column, improving join performance.

## Data Warehouse vs. Data Lake:
- **Data Warehouse**: Schema on write, supports ACID, suitable for structured data.
- **Data Lake**: Schema on read, supports semi-structured and unstructured data.

## Delta Lakehouse:
- Combines features of data lakes, data warehouses, and Delta Lake, supporting structured, semi-structured, and unstructured data.

## Autoloader:
- Handles automatic data ingestion from cloud storage, supporting schema inference, datatype hints, and handling bad data with rescue capabilities.

## Spark Structured Streaming:
- Supports **manual** and **automatic schema evolution**.
- Used for both batch and streaming processing, handling schema changes dynamically.

## SQL Warehouse and Instance Pools:
- **SQL Warehouse**: Used for querying data without disturbing running clusters.
- **Instance Pools**: Reduces cluster start time by maintaining a pool of VMs ready for use.

## Example Scenarios:
- For processing large datasets (e.g., 15TB), create multiple clusters and distribute the load among them for optimal performance.

## Delta Live Tables (DLT):
- A declarative ETL framework that simplifies SCD implementation and provides lineage tracking using Unity Catalog.

---

This summarizes the main topics covered in your Databricks notes, using Markdown formatting. Let me know if you need any adjustments!

