# ⚡ Apache Kafka Setup on AWS EC2 (Ubuntu) — End to End Guide

This guide explains how to install and configure **Kafka + ZooKeeper** on an **Ubuntu EC2 instance**, create topics, run a producer and consumer, and integrate with S3 using Python.

---

## 🧠 Prerequisites

- AWS EC2 instance (Ubuntu)
- `.pem` key file for SSH access
- Kafka 3.7.0 or above
- Java 11 or above
- Python 3.8+ (for optional producer/consumer)

---

## 🔐 Step 1: SSH into EC2 Instance

```bash
ssh -i "D:\AWS Credentials\Helooo.pem" ubuntu@16.16.4.141
⚙️ Step 2: Install Java and Dependencies
bash
Copy code
sudo apt update -y
sudo apt install openjdk-11-jdk -y
java -version
📦 Step 3: Download and Extract Kafka
bash
Copy code
cd /opt
sudo wget https://downloads.apache.org/kafka/3.7.0/kafka_2.13-3.7.0.tgz
sudo tar -xvzf kafka_2.13-3.7.0.tgz
sudo mv kafka_2.13-3.7.0 kafka
sudo mkdir -p /opt/kafka/logs
🧩 Step 4: Configure Kafka Server Properties
bash
Copy code
sudo nano /opt/kafka/config/server.properties
Add or edit the following lines:

bash
Copy code
broker.id=1
listeners=PLAINTEXT://16.16.4.141:9092
advertised.listeners=PLAINTEXT://16.16.4.141:9092
log.dirs=/opt/kafka/logs
zookeeper.connect=localhost:2181
Save with Ctrl + O, then exit with Ctrl + X.

🐘 Step 5: Start ZooKeeper
bash
Copy code
nohup /opt/kafka/bin/zookeeper-server-start.sh /opt/kafka/config/zookeeper.properties > /opt/kafka/zookeeper.log 2>&1 &
Check it’s running:

bash
Copy code
jps
🚀 Step 6: Start Kafka Server
bash
Copy code
nohup /opt/kafka/bin/kafka-server-start.sh /opt/kafka/config/server.properties > /opt/kafka/kafka.log 2>&1 &
🧵 Step 7: Create and List Topics
Create a topic:

bash
Copy code
/opt/kafka/bin/kafka-topics.sh --create --topic demo_test --bootstrap-server 16.16.4.141:9092 --partitions 1 --replication-factor 1
List topics:

bash
Copy code
/opt/kafka/bin/kafka-topics.sh --list --bootstrap-server 16.16.4.141:9092
Describe a topic:

bash
Copy code
/opt/kafka/bin/kafka-topics.sh --describe --topic demo_test --bootstrap-server 16.16.4.141:9092
💬 Step 8: Start Producer
bash
Copy code
/opt/kafka/bin/kafka-console-producer.sh --topic demo_test --bootstrap-server 16.16.4.141:9092
Type any message and press Enter to send it.

📥 Step 9: Start Consumer
Open another terminal and run:

bash
Copy code
/opt/kafka/bin/kafka-console-consumer.sh --topic demo_test --from-beginning --bootstrap-server 16.1

