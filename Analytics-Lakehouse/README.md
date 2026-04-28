# Analytics Lakehouse (E-commerce)

Local analytics lakehouse stack for an e-commerce scenario using Postgres, Spark + Iceberg, Trino, MinIO, Airflow, and MailHog.

## Prerequisites

- Docker Engine
- Docker Compose (plugin or standalone)

## Quick Start (required command sequence)

Run these commands from this directory:

```bash
docker-compose up -d --build
./lakehouse-preparer.sh
docker compose -f airflow.yaml build
docker compose -f airflow.yaml up airflow-init
docker compose -f airflow.yaml up -d
```

## Airflow: Trino Connection

The project expects a Trino connection in Airflow with the following values:

- Connection Id: trino_default
- Connection Type: Trino
- Host: trino
- Port: 8443
- Login: test
- Password: password
- Schema: iceberg
- Extra:

```json
{"verify": false, "protocol": "https"}
```

## What the scripts do (high level)

- `docker-compose up -d --build`: starts the lakehouse services (Postgres, Spark, Iceberg REST catalog, MinIO, Trino, loadgen, MailHog).
- `./lakehouse-preparer.sh`: prepares the lakehouse (downloads jars, runs notebook, loads data into Iceberg, builds silver tables, and creates gold tables in Trino).
- `docker compose -f airflow.yaml build`: builds the Airflow image with Trino provider.
- `docker compose -f airflow.yaml up airflow-init`: initializes the Airflow metadata database and creates the default admin user and Trino connection.
- `docker compose -f airflow.yaml up -d`: starts Airflow webserver and scheduler after init.

