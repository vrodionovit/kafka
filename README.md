# Bootstrap

![image](https://user-images.githubusercontent.com/109636425/217402180-9f633569-14c1-4e41-8df1-d63786a48189.png)

## Useful command:

### Run producer and consumer

```bash
sh kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic nifi-test-01 
sh kafka-console-producer.sh --bootstrap-server localhost:9092 --topic nifi-test-01 
```

### Run offsetexplorer with JAAS config:

If your cluster is configured for SASL (plaintext or SSL) you must either specify the JAAS config in the UI or pass in your JAAS config file to Offset Explorer when you start it. The exact contents of the JAAS file depend on the configuration of your cluster, please refer to the Kafka documentation.

```bash
offsetexplorer -J-Djava.security.auth.login.config=/home/kafka/kafka/config/client.conf
```

### Manual start Zookeeper or Kafka:

Zookeeper:

```bash
export KAFKA_OPTS=-Djava.security.auth.login.config=/home/kafka/kafka/config/zookeeper_jaas.con
sh /home/kafka/kafka/bin/zookeeper-server-start.sh /home/kafka/kafka/config/zookeeper.properties
```

Kafka:

```bash
export KAFKA_OPTS=-Djava.security.auth.login.config=/home/kafka/kafka/config/kafka_jaas.conf
sh /home/kafka/kafka/bin/kafka-server-start.sh /home/kafka/kafka/config/kafka_server_sasl.properties
```

# Flask app

Application works with psycopg2 to connect to postgresql. Flask by default run the application on 5000 port. 

## Credentials:

```
🔑 Basic http authenticate:
username: admin
pass: 0L46eH8@a4MY

```

## Testing the App:

```bash
curl -X GET "localhost:5000" --header 'Authorization: Basic YWRtaW46MEw0NmVIOEBhNE1Z'
```

## Testing the API:

```bash
curl -X GET "localhost:5000/api/daily_incidents" --header 'Authorization: Basic YWRtaW46MEw0NmVIOEBhNE1Z'
curl -X GET "localhost:5000/api/daily_forecast" --header 'Authorization: Basic YWRtaW46MEw0NmVIOEBhNE1Z'
```

# Apache NIFI

Apache Nifi works with **https** and runs on 8443 port.

## Credentials:

```
🔑 NIFI authenticate:
username: kafka
pass: cuibonocuiprodest
```
# Apache Kafka

Apache kafka runs on several ports and protocols:

```
🌐 PLAINTEXT://localhost:9092 — Un-authenticated, non-encrypted channel
SASL_PLAINTEXT://localhost:9093 — SASL authenticated, non-encrypted channel
```

Authentication with SASL using JAAS:

```
🔑 KafkaServer {
 org.apache.kafka.common.security.plain.PlainLoginModule required
  username="cluster"
  password="cluster"
  user_cluster="clusterpasswd"
  user_kafka="kafkapasswd"
  serviceName="kafka";
};

KafkaClient {
  org.apache.kafka.common.security.plain.PlainLoginModule required
  username="kafka"  
  password="kafkapasswd"
  serviceName="zookeeper";
};

Client {
 org.apache.kafka.common.security.plain.PlainLoginModule required  
  username="kafka"  
  password="kafkapasswd"
  serviceName="zookeeper";
};
```

# Kafka UI — AKHQ

Configuration file:

```yaml
akhq:
  connections:
    local:
      properties:
        bootstrap.servers: "localhost:9093"
        security.protocol: SASL_PLAINTEXT
        sasl.enabled.mechanisms: PLAIN
        sasl.mechanism: PLAIN
        sasl.jaas.config: org.apache.kafka.common.security.plain.PlainLoginModule required username="kafka" password="kafkapasswd";
```

# Offet Explorer — Kafka tools

Configuration file:
```
KafkaClient {
  org.apache.kafka.common.security.plain.PlainLoginModule required
  username="kafka"  
  password="kafkapasswd"
  serviceName="zookeeper"
  sasl.enabled.mechanisms=PLAIN;
};

Client {
 org.apache.kafka.common.security.plain.PlainLoginModule required  
  username="kafka"  
  password="kafkapasswd"
  serviceName="zookeeper"
  sasl.enabled.mechanisms=PLAIN;
};
```


```
kafka-dev: kaf

root: cuibonocuiprodest

kafka: toor
```
# Access

```yaml

Apache Kafka brockers:
    ports: 9092, 9093, 9094, 9095
    protocol: PLAINTEXT, SASL_PLAINTEXT, SSL, SASL_SSL

Flask API:
    ports: 5000
    protocol: HTTP

NIFI:
    ports: 8443
    protocol: HTTPS

Zookeeper:
    ports: 2181
    protocol: NONE

Kafka tools:
    ports: 8080
    protocol: HTTP
```
