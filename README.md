# Connecting Apache Spark with an External metastore [Apache Hive]

#### Purpose
The purpose of connecting Apache Spark with an external Hive metastore is to leverage the unified data management capabilities of Hive within Spark’s ecosystem. This integration allows Spark applications to access metadata and data stored in Hive tables, enabling seamless querying and data manipulation. It facilitates complex data analytics by utilizing the rich features of Hive, such as schema management, partitioning, and indexing. This setup is beneficial for organizations looking to maintain a centralized data warehouse for their big data platforms, providing a consolidated environment for managing large-scale datasets and performing data transformations efficiently.

#### Prerequisites:
* Apache hive 3.1.3
* Apache Spark 3.5.0
* Java version 8
* Python 3.8.+<br/>
#### Here I am using open source spark with hive and setting up on Linux VM.

## Hive Installation
1. Download Apache Hive 3.1.3 from this [Link](https://hive.apache.org/general/downloads/).
2. To download use command
   ''''python
   wget https://archive.apache.org/dist/hive/hive-3.1.3/apache-hive-3.1.3-bin.tar.gz
   '''



