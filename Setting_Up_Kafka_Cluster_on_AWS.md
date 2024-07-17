# Setting Up a Kafka Cluster on AWS EC2 Instances with Kafka Connect and Debezium for MSSQL

## Prerequisites

1. **EC2 Instances**: Ensure you have three EC2 instances running with appropriate security groups (open ports 2181 for ZooKeeper, 9092 for Kafka brokers, and 8083 for Kafka Connect).
2. **Java Installed**: Kafka requires Java. Install OpenJDK on each instance:
   ```bash
   sudo apt update
   sudo apt install openjdk-11-jdk -y
   java -version
   ```

## Step 1: Download and Install Kafka

1. **Download Kafka**:
   ```bash
   wget https://downloads.apache.org/kafka/3.2.0/kafka_2.13-3.2.0.tgz
   tar -xzf kafka_2.13-3.2.0.tgz
   cd kafka_2.13-3.2.0
   ```

2. **Update `server.properties`**:
   On each EC2 instance, edit `config/server.properties`:
   ```bash
   nano config/server.properties
   ```
   Modify the following lines:
   ```properties
   broker.id=<unique_id_for_each_instance> # Use 0, 1, 2 for your three instances
   listeners=PLAINTEXT://<instance_private_ip>:9092
   zookeeper.connect=<private_ip_instance1>:2181,<private_ip_instance2>:2181,<private_ip_instance3>:2181
   ```

## Step 2: Start ZooKeeper and Kafka Brokers

1. **Start ZooKeeper**:
   On each instance, start ZooKeeper:
   ```bash
   bin/zookeeper-server-start.sh config/zookeeper.properties
   ```

2. **Start Kafka**:
   On each instance, start Kafka:
   ```bash
   bin/kafka-server-start.sh config/server.properties
   ```

## Step 3: Download and Install Kafka Connect

1. **Download Kafka Connect**:
   Kafka Connect is part of the Kafka distribution. Configure `connect-standalone.properties`:
   ```bash
   nano config/connect-standalone.properties
   ```
   Update the properties:
   ```properties
   bootstrap.servers=<instance_private_ip1>:9092,<instance_private_ip2>:9092,<instance_private_ip3>:9092
   key.converter=org.apache.kafka.connect.json.JsonConverter
   value.converter=org.apache.kafka.connect.json.JsonConverter
   key.converter.schemas.enable=false
   value.converter.schemas.enable=false
   ```

## Step 4: Setup Debezium for MSSQL

1. **Download Debezium Connector**:
   ```bash
   wget https://repo1.maven.org/maven2/io/debezium/debezium-connector-sqlserver/1.7.1.Final/debezium-connector-sqlserver-1.7.1.Final-plugin.tar.gz
   tar -xzf debezium-connector-sqlserver-1.7.1.Final-plugin.tar.gz
   cp -r debezium-connector-sqlserver /path/to/kafka/libs/
   ```

2. **Create MSSQL Connector Configuration**:
   Create a new file `debezium-sqlserver-connector.properties`:
   ```properties
   name=dbserver1
   connector.class=io.debezium.connector.sqlserver.SqlServerConnector
   database.hostname=<mssql_hostname>
   database.port=1433
   database.user=<db_user>
   database.password=<db_password>
   database.dbname=<db_name>
   database.server.name=<logical_server_name>
   table.include.list=<database_name>.dbo.<table_name>
   database.history.kafka.bootstrap.servers=<instance_private_ip1>:9092
   database.history.kafka.topic=schema-changes.<logical_server_name>
   ```

## Step 5: Start Kafka Connect with Debezium

1. **Start Kafka Connect**:
   ```bash
   bin/connect-standalone.sh config/connect-standalone.properties config/debezium-sqlserver-connector.properties
   ```

## Step 6: Verify and Test

1. **Produce and Consume Messages**:
   - Use `kafka-console-producer.sh` and `kafka-console-consumer.sh` to test message flow.
   - Check if Kafka Connect is running and consuming changes from MSSQL.

## Step 7: Configure Systemd for Automatic Start on Reboot

### Create Systemd Service Files

1. **Create ZooKeeper Service File**:
   Create a file named `zookeeper.service`:
   ```bash
   sudo nano /etc/systemd/system/zookeeper.service
   ```
   Add the following content:
   ```ini
   [Unit]
   Description=Apache ZooKeeper Server
   After=network.target

   [Service]
   Type=simple
   ExecStart=/path/to/kafka/bin/zookeeper-server-start.sh /path/to/kafka/config/zookeeper.properties
   ExecStop=/path/to/kafka/bin/zookeeper-server-stop.sh
   Restart=on-abnormal

   [Install]
   WantedBy=multi-user.target
   ```

2. **Create Kafka Broker Service File**:
   Create a file named `kafka.service`:
   ```bash
   sudo nano /etc/systemd/system/kafka.service
   ```
   Add the following content:
   ```ini
   [Unit]
   Description=Apache Kafka Server
   After=zookeeper.service

   [Service]
   Type=simple
   ExecStart=/path/to/kafka/bin/kafka-server-start.sh /path/to/kafka/config/server.properties
   ExecStop=/path/to/kafka/bin/kafka-server-stop.sh
   Restart=on-abnormal

   [Install]
   WantedBy=multi-user.target
   ```

3. **Create Kafka Connect Service File**:
   Create a file named `kafka-connect.service`:
   ```bash
   sudo nano /etc/systemd/system/kafka-connect.service
   ```
   Add the following content:
   ```ini
   [Unit]
   Description=Kafka Connect Service
   After=kafka.service

   [Service]
   Type=simple
   ExecStart=/path/to/kafka/bin/connect-standalone.sh /path/to/kafka/config/connect-standalone.properties /path/to/kafka/config/debezium-sqlserver-connector.properties
   ExecStop=/bin/kill -TERM $MAINPID
   Restart=on-abnormal

   [Install]
   WantedBy=multi-user.target
   ```

### Reload Systemd and Enable Services

1. **Reload Systemd**:
   ```bash
   sudo systemctl daemon-reload
   ```

2. **Enable and Start ZooKeeper**:
   ```bash
   sudo systemctl enable zookeeper
   sudo systemctl start zookeeper
   ```

3. **Enable and Start Kafka**:
   ```bash
   sudo systemctl enable kafka
   sudo systemctl start kafka
   ```

4. **Enable and Start Kafka Connect**:
   ```bash
   sudo systemctl enable kafka-connect
   sudo systemctl start kafka-connect
   ```

### Verify Services

Check the status of each service to ensure they are running correctly:
```bash
sudo systemctl status zookeeper
sudo systemctl status kafka
sudo systemctl status kafka-connect
```

### Reboot and Verify

Reboot your EC2 instance to ensure that the services start automatically:
```bash
sudo reboot
```

After the instance reboots, verify the status of each service again:
```bash
sudo systemctl status zookeeper
sudo systemctl status kafka
sudo systemctl status kafka-connect
```

This setup ensures that ZooKeeper, Kafka, and Kafka Connect services will start automatically upon a system reboot, maintaining the operation of your Kafka ecosystem.
```

You can now copy all the content above and paste it into your Markdown repository as a single file.
