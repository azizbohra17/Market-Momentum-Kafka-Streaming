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
