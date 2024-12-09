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
   ```python
   wget https://archive.apache.org/dist/hive/hive-3.1.3/apache-hive-3.1.3-bin.tar.gz
   ```

3. Extract the file.
   ```python
   tar -xvf apache-hive-3.1.3-bin.tar.gz
   ```
5. Add Environment variables
   ```python
    export HIVE_HOME=/home/pallavi/apache-hive-3.1.3-bin
    export HIVE_CONF_DIR=/home/pallavi/apache-hive-3.1.3-bin/conf
    export PATH=$PATH:$HIVE_HOME/bin
   ```
7. Create folders like below inside hive
   ```python
      cd apache-hive-3.1.3-bin
      mkdir data
      mkdir data/warehouse
      mkdir tmp
      mkdir tmp/hive
   ```
#### Provide read/write permissions for the above folder

    ```python
      chmod 777 tmp and chmod 777 tmp/hive
      chmod 777 data/warehouse
    ```
9. Add a configuration file as hive-site.xml inside cd apache-hive-3.1.3-bin/conf
    ```python
    ///hive-site.xml
    <?xml version="1.0"?>
      <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
      <configuration>
      <property>
        <name>hive.server2.enable.doAs</name>
        <value>false</value>
      </property>
      <property>
          <name>hive.exec.scratchdir</name>
          <value>/home/pallavi/apache-hive-3.1.3-bin/tmp/hive</value>
          <description>HDFS root scratch dir for Hive jobs which gets created</description>
        </property>
      <property>
          <name>hive.metastore.warehouse.dir</name>
          <value>/home/pallavi/apache-hive-3.1.3-bin/data/warehouse</value>
          <description>location of default database for the warehouse</description>
        </property>
      <property>
          <name>hive.execution.engine</name>
          <value>spark</value>
      </property>
      <property>
          <name>spark.master</name>
          <value>spark://10.0.0.8:9075</value>
          <description>The master URL for Spark in local mode</description>
      </property>
      <property>
          <name>spark.serializer</name>
          <value>org.apache.spark.serializer.KryoSerializer</value>
      </property>
      <property>
          <name>hive.metastore.uris</name>
          <value>thrift://10.0.0.8:9038</value>
      </property>
      <property>
          <name>metastore.metastore.event.db.notification.api.auth</name>
          <value>false</value>
      </property>
      <property>
          <name>hive.metastore.execute.setugi</name>
          <value>true</value>
      </property>
      </configuration>

      ```

7. Start the metastore service in hive
   ```python
    hive --service metastore -p 9038
   ```
#### This starts the metastore at 10.0.0.8 on 9038 port , if we don’t specify the host and port by default it listens to 0.0.0.0:9038.  


## Apache Spark Installation
Refer the [Link](https://github.com/DataSturdy/FrameWorks/tree/main/Superset) to see how to install open source Apache spark to linux.

#### After setting up the spark connect it with the external hive-metastore by following the steps

* Start the master and worker in spark
  ```python
  ./sbin/start-master.sh -h 10.0.0.8 -p 9075 --webui-port 9066
  ./sbin/start-worker.sh spark://10.0.0.8:9075 -p 9076 --webui-port 9068
  ```

* Configure the Spark with hive and start the sparksession
  ```python
  spark-sql --master spark://10.0.0.8:9075 --conf spark.sql.catalogImplementation=hive --conf spark.sql.warehouse.dir=/home/pallavi/apache-hive-3.1.3-bin/data/warehouse --conf spark.hadoop.hive.metastore.uris=thrift://10.0.0.8:9038 --conf spark.sql.hive.metastore.jars=maven --conf spark.sql.hive.metastore.version=3.1.3
  ```

* After the session started you can verify by giving
  ```python
  Show databses
  Create database spark_db
  CREATE TABLE users ( id INT, name STRING, age INT );
  Insert Into users values (1, 'Anu', 23);
  show tables
  ```

### Now you are successfully connected with External hive-metastore where you can store metadata of the tables permanently and access this metadata using other other tools.


### Note
* In hive we can create tables but cannot perform insert, update, delete operations. So we can use Engine such as Apache Spark or Tez.
* Check the version compatiblity between Apache Hive and Apache Spark.
  






  




