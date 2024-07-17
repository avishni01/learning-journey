# Debezium.io Guide

## What is Debezium?

Debezium is an open-source distributed platform for change data capture (CDC). It captures row-level changes in your databases and streams them in real-time to Kafka topics, allowing you to react to database changes as they happen. It supports various databases like MySQL, PostgreSQL, MongoDB, SQL Server, and more.

## Use Cases

- **Data Replication**: Keeping multiple databases in sync by capturing changes in one database and applying them to others.
- **Event-Driven Architectures**: Triggering microservices or other downstream systems based on database changes.
- **Analytics**: Feeding real-time data changes to data lakes or analytics platforms for up-to-date insights.
- **Cache Invalidation**: Invalidating cache entries in systems like Redis when underlying database data changes.

## Installation Process

### Prerequisites

- Apache Kafka
- ZooKeeper (for Kafka)
- Database (e.g., MySQL, PostgreSQL)

### Step-by-Step Installation

1. **Set up Kafka and ZooKeeper**
   - Download and extract Kafka.
   - Start ZooKeeper:
     ```sh
     bin/zookeeper-server-start.sh config/zookeeper.properties
     ```
   - Start Kafka server:
     ```sh
     bin/kafka-server-start.sh config/server.properties
     ```

2. **Download Debezium Connector**
   - Go to [Debezium Connector Downloads](https://debezium.io/download/) and download the connector for your database.

3. **Install the Connector**
   - Extract the connector into the Kafka `libs` directory.

4. **Configure the Connector**
   - Create a configuration file (e.g., `debezium-mysql-connector.json`) with the following content:
     ```json
     {
       "name": "mysql-connector",
       "config": {
         "connector.class": "io.debezium.connector.mysql.MySqlConnector",
         "tasks.max": "1",
         "database.hostname": "localhost",
         "database.port": "3306",
         "database.user": "debezium",
         "database.password": "dbz",
         "database.server.id": "184054",
         "database.server.name": "fullfillment",
         "database.whitelist": "inventory",
         "database.history.kafka.bootstrap.servers": "localhost:9092",
         "database.history.kafka.topic": "schema-changes.inventory"
       }
     }
     ```

5. **Start the Connector**
   - Start Kafka Connect with the standalone mode:
     ```sh
     bin/connect-standalone.sh config/connect-standalone.properties path/to/debezium-mysql-connector.json
     ```

## Important Configurations

1. **connector.class**: Specifies the Debezium connector class for your database (e.g., `io.debezium.connector.mysql.MySqlConnector`).
2. **tasks.max**: Defines the maximum number of tasks that should be created for this connector.
3. **database.hostname**: The hostname of the database server.
4. **database.port**: The port on which the database server is listening.
5. **database.user**: The username to connect to the database.
6. **database.password**: The password to connect to the database.
7. **database.server.id**: A unique identifier for this connector to avoid conflicts.
8. **database.server.name**: Logical name for the database server/cluster.
9. **database.whitelist**: Comma-separated list of databases to capture.
10. **database.history.kafka.bootstrap.servers**: Kafka broker addresses for storing schema history.
11. **database.history.kafka.topic**: Topic to store schema history.

### Commonly Used Configurations

1. **snapshot.mode**: Defines how the connector should perform snapshots of the database tables (e.g., `initial`, `schema_only`, `never`).
2. **include.schema.changes**: Whether to include schema change events in the output (true/false).
3. **table.whitelist**: Specifies the tables to be monitored within the whitelisted databases.
4. **database.include.list**: Specifies the databases to be monitored, similar to `database.whitelist`.
5. **table.include.list**: More granular table inclusion within the monitored databases.
6. **database.history.kafka.retries**: Number of retry attempts for writing schema changes to Kafka.

## Conclusion

Debezium is a powerful tool for real-time data integration and change data capture. By streaming database changes to Kafka, it enables various use cases such as data replication, event-driven architectures, and real-time analytics. Proper installation and configuration are crucial to leverage its full potential, and understanding the key configurations helps in optimizing its usage.

For detailed information, always refer to the [Debezium documentation](https://debezium.io/documentation/).

# Working with Debezium and MSSQL

## MSSQL Setup for Debezium

### Prerequisites

- MSSQL Server
- Apache Kafka
- Debezium MSSQL Connector

### Configuration

1. **Configure MSSQL Server for CDC**
   - Enable CDC on the database:
     ```sql
     EXEC sys.sp_cdc_enable_db;
     ```
   - Enable CDC on the tables you want to monitor:
     ```sql
     EXEC sys.sp_cdc_enable_table
       @source_schema = N'dbo',
       @source_name   = N'YourTableName',
       @role_name     = NULL;
     ```

2. **Create Debezium Configuration**
   - Create a configuration file (e.g., `debezium-mssql-connector.json`) with the following content:
     ```json
     {
       "name": "mssql-connector",
       "config": {
         "connector.class": "io.debezium.connector.sqlserver.SqlServerConnector",
         "tasks.max": "1",
         "database.hostname": "localhost",
         "database.port": "1433",
         "database.user": "debezium",
         "database.password": "dbz",
         "database.server.name": "server1",
         "database.names": "mydb",
         "table.include.list": "dbo.YourTableName",
         "database.history.kafka.bootstrap.servers": "localhost:9092",
         "database.history.kafka.topic": "schema-changes.mydb"
       }
     }
     ```

3. **Start the Connector**
   - Start Kafka Connect with the standalone mode:
     ```sh
     bin/connect-standalone.sh config/connect-standalone.properties path/to/debezium-mssql-connector.json
     ```

## Troubleshooting Techniques

1. **Check Kafka Connect Logs**
   - Kafka Connect logs provide valuable insights into the connector's operations and errors. The logs can be found in the Kafka `logs` directory.

2. **Verify CDC Configuration in MSSQL**
   - Ensure that CDC is enabled correctly on the database and tables:
     ```sql
     SELECT * FROM sys.change_tracking_tables;
     ```

3. **Connector Configuration**
   - Verify the connector configuration file for any misconfigurations. Ensure that the database credentials, server name, and table names are correct.

4. **Network Connectivity**
   - Ensure that the MSSQL server is reachable from the Kafka Connect server. Use tools like `telnet` or `nc` to test the connection.

5. **Kafka Topics**
   - Check that the Kafka topics specified in the configuration exist and are receiving messages. Use Kafka tools or the Kafka console consumer to inspect the topics.

## Internal Logs Location and Configuration

1. **Kafka Connect Logs**
   - Logs are typically located in the Kafka `logs` directory:
     ```
     kafka/logs/
     ```

2. **Debezium Connector Logs**
   - Configure logging for Debezium by editing the `log4j.properties` file in the Kafka `config` directory:
     ```properties
     log4j.logger.io.debezium=DEBUG, stdout
     log4j.logger.io.debezium.connector.sqlserver=DEBUG, stdout
     ```

## Best Practices

1. **Resource Allocation**
   - Ensure sufficient resources (CPU, memory) for Kafka Connect workers, especially when dealing with large volumes of data.

2. **Secure Connections**
   - Use secure connections (SSL/TLS) for communication between Kafka Connect and MSSQL, and between Kafka Connect and Kafka brokers.

3. **Error Handling**
   - Implement proper error handling and retry mechanisms in your consumer applications to handle any data inconsistencies or connection issues.

4. **Monitoring and Alerts**
   - Set up monitoring and alerting for Kafka Connect, Debezium connectors, and MSSQL to detect and resolve issues promptly.

5. **Schema Evolution**
   - Handle schema changes carefully. Use the Debezium schema registry integration to manage schema versions and ensure compatibility.

6. **Performance Tuning**
   - Tune the connector configurations such as `poll.interval.ms`, `max.batch.size`, and `buffer.memory` based on your workload and performance requirements.

## Conclusion

Debezium provides a robust and flexible solution for capturing and streaming database changes. Proper setup, configuration, and monitoring are crucial for ensuring reliable and efficient operation. By following best practices and utilizing troubleshooting techniques, you can effectively manage your Debezium deployments with MSSQL.

For more detailed information, refer to the [Debezium documentation](https://debezium.io/documentation/).

