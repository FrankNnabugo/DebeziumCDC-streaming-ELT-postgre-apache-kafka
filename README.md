Streaming ELT pipeline from postgres to kafka, to elastic search using Debezium as source and sink connector..

Start Containers:
docker compose -f infra/docker/docker-compose.yml up 

Wait until:
Kafka is healthy
Kafka Connect is running
Elasticsearch is ready
postgres is running

Verify Kafka Connect
curl http://localhost:8083

List connectors
curl http://localhost:8083/connectors


Get connector status
curl http://localhost:8083/connectors/postgres-connector/status
curl http://localhost:8083/connectors/elastic-sink/status

Register Debezium Source Connector
Run script/connector.sh

Create Table in Postgres:

docker exec -it postgres psql -U postgres

CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255),
    email VARCHAR(255)
);

Insert Data:
INSERT INTO users(name, email)
VALUES ('Frank', 'frank@gmail.com');


Verify Kafka Receives Events:
docker exec -it kafka kafka-console-consumer \
--bootstrap-server localhost:9092 \
--topic cdc.public.users \
--from-beginning


Register Elasticsearch Sink Connector
Run script/sink.sh

Query elastic search:
curl localhost:9200/_cat/indices?v

Kibana:
UI for Elasticsearch.

Open:
http://localhost:5601
