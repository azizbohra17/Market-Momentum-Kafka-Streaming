# Market Momentum: Real-Time Stock Data Analysis

## Project Overview

Market Momentum is a data engineering initiative focused on the real-time analysis of stock market data using Apache Kafka and a suite of AWS services. With an emphasis on cloud configuration and resource optimization, this project prioritizes efficient cloud resource utilization over extensive coding.

### Key Components
- **Apache Kafka**: For real-time data streaming.
- **AWS EC2**: To host Kafka brokers and manage producer/consumer communications.
- **AWS S3**: For durable data storage.
- **AWS Glue**: For data cataloging.
- **AWS Athena**: For running SQL queries on stored data.
- **Boto3 SDK**: For interacting with AWS services using Python.
- **Zookeeper**: For managing Kafka's cluster coordination.

![image](https://github.com/azizbohra17/Market-Momentum-Kafka-Streaming/assets/47524749/9863bf44-f85d-45a0-ac58-5c7f5aa0931d)

## Installation & Configuration

### Prerequisites
- An AWS account with access to EC2, S3, Glue, and Athena services.
- An EC2 instance (preferably Amazon Linux 2 Kernel 5.10 AMI 2.0.20240223.0 x86_64 HVM gp2 to avoid known issues with the latest AMI).
- Apache Kafka 3.3.1
- Python 3.11
- Boto3 SDK for Python

### Setting Up Kafka on EC2
SSH into your EC2 instance and set up Kafka by following these steps:

```bash
# SSH into your EC2 instance
ssh -i /path/to/your/key.pem ec2-user@your-ec2-public-ip

# Install Java 1.8 (required by Kafka)
java -version
sudo yum install java-1.8.0-openjdk
java -version

# Download and extract Kafka
wget https://downloads.apache.org/kafka/3.3.1/kafka_2.12-3.3.1.tgz
tar -xvf kafka_2.12-3.3.1.tgz
cd kafka_2.12-3.3.1
```
### Alternate way, if you encounter issues during installation of java
Installing Java 1.8 on EC2

Java 1.8 was not directly available, but I found a solution on Stack Overflow:

```bash
$ yum search java | grep "1.8"
Last metadata expiration check: 1:00:33 ago on Tue Aug  8 18:28:56 2023.
java-1.8.0-amazon-corretto.x86_64 : Amazon Corretto runtime environment
java-1.8.0-amazon-corretto-devel.x86_64 : Amazon Corretto development environment
```

Then, I installed it using:

```bash

$ sudo yum install java-1.8.0-amazon-corretto.x86_64 
# This installed the correct version:

$ java -version
openjdk version "1.8.0_382"
```
For more details, refer to the [Stack Overflow post](https://stackoverflow.com/questions/76862527/yum-what-java-8-versions-are-available-to-install).


# Configuring Kafka and Zookeeper

Start Zookeeper and Kafka services with the following configurations:

```bash
# Start Zookeeper
bin/zookeeper-server-start.sh config/zookeeper.properties

# Open another terminal session and SSH into your EC2 instance
ssh -i /path/to/your/key.pem ec2-user@your-ec2-public-ip

# Start Kafka server with custom heap options to manage memory
export KAFKA_HEAP_OPTS="-Xmx256M -Xms128M"
cd kafka_2.12-3.3.1
bin/kafka-server-start.sh config/server.properties

# Modify the server.properties to set ADVERTISED_LISTENERS to your EC2 public IP
sudo nano config/server.properties
```

# Creating Topics and Starting Producer/Consumer

Create Kafka topics and initiate the producer and consumer services:

```bash
# Create a Kafka topic and replace [topic_name] with your specific topic name
bin/kafka-topics.sh --create --topic [topic_name] --bootstrap-server your-ec2-public-ip:9092 --replication-factor 1 --partitions 1

# Start Kafka producer
bin/kafka-console-producer.sh --topic [topic_name] --bootstrap-server your-ec2-public-ip:9092

# In a new terminal session, start Kafka consumer
bin/kafka-console-consumer.sh --topic [topic_name] --bootstrap-server your-ec2-public-ip:9092
```

# Challenges and Resolutions

- Overcame Kafka's incompatibility with Python 3.12 by implementing Python 3.11.
- Addressed memory requirements essential for running Kafka server on a free tier EC2 instance.
- Mitigated communication issues between producer and consumer on Amazon Linux 2023 AMI by switching to Amazon Linux 2 Kernel 5.10 AMI.
- Configured Zookeeper timeout settings to maintain robustness in the pipeline.
- Optimized data transfer to work within the free tier's memory constraints, ensuring smooth data flow from producer to consumer (S3 bucket) without overloading the broker.
