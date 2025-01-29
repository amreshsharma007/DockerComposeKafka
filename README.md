
# Kafka Docker Compose Setup

This repository contains a `docker-compose.yml` file for setting up a Kafka environment using Docker. It includes the following services:

- **Zookeeper**
- **Kafka**
- **Kafka-UI**

## Prerequisites

- [Docker](https://www.docker.com/get-started) installed on your system.
- [Docker Compose](https://docs.docker.com/compose/install/) installed.

## Services Overview

### Zookeeper
- **Image**: `confluentinc/cp-zookeeper:latest`
- **Ports**: `2181:2181`
- **Environment Variables**:
  - `ZOOKEEPER_CLIENT_PORT=2181`
  - `ZOOKEEPER_TICK_TIME=2000`

### Kafka
- **Image**: `confluentinc/cp-kafka:latest`
- **Depends On**: `zookeeper`
- **Ports**:
  - `9092:9092`
  - `29092:29092`
- **Environment Variables**:
  - `KAFKA_BROKER_ID=1`
  - `KAFKA_ZOOKEEPER_CONNECT=zookeeper:2181`
  - `KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://kafka:9092,PLAINTEXT_HOST://localhost:29092`
  - `KAFKA_LISTENER_SECURITY_PROTOCOL_MAP=PLAINTEXT:PLAINTEXT,PLAINTEXT_HOST:PLAINTEXT`
  - `KAFKA_INTER_BROKER_LISTENER_NAME=PLAINTEXT`
  - `KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR=1`

### Kafka-UI
- **Image**: `provectuslabs/kafka-ui:latest`
- **Container Name**: `kafka-ui`
- **Network Mode**: `host`
- **Depends On**: `kafka`, `zookeeper`
- **Environment Variables**:
  - `SERVER_PORT=8080`
  - `KAFKA_CLUSTERS_0_NAME=Localhost`
  - `KAFKA_CLUSTERS_0_BOOTSTRAPSERVERS=localhost:29092`
  - `DYNAMIC_CONFIG_ENABLED=true`

> **Note**: The `ports` section for `kafka-ui` is commented out in the provided configuration. Uncomment and modify it if needed.

## Usage

1. Clone this repository:
   ```bash
   git clone <repository-url>
   cd <repository-folder>
   ```

2. Start the services:
   ```bash
   docker-compose up -d
   ```

3. Access Kafka-UI:
   - Navigate to `http://localhost:8080` in your web browser (if `ports` are uncommented and configured correctly).

4. Stop the services:
   ```bash
   docker-compose down
   ```

## Notes

- Ensure the ports `2181`, `9092`, `29092`, and `8080` (if enabled) are not in use by other applications.
- Adjust the environment variables or configurations based on your requirements.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
