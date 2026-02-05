# Apache Iceberg Demo with Spark and MinIO

This project provides a complete, easy-to-use Docker environment for exploring and demonstrating **Apache Iceberg** features using **Apache Spark**. It includes a REST catalog, a MinIO S3-compatible object storage, and a PostgreSQL database for catalog persistence.

## Project Architecture

The environment consists of the following services orchestrated via Docker Compose:

*   **`spark-iceberg`**: Apache Spark container pre-configured with Iceberg support and Jupyter Notebook.
*   **`iceberg-rest`**: The Iceberg REST Catalog service.
*   **`minio`**: S3-compatible object storage for data warehouse.
*   **`postgres-iceberg`**: PostgreSQL database backend for the Iceberg REST Catalog.

## Prerequisites

*   [Docker](https://docs.docker.com/get-docker/)
*   [Docker Compose](https://docs.docker.com/compose/install/)

## Quick Start

1.  **Clone the repository**:
    ```bash
    git clone <repository-url>
    cd icebergdemo
    ```

2.  **Start the environment**:
    ```bash
    docker-compose up -d
    ```

3.  **Access the services**:

    | Service | URL | Description |
    | :--- | :--- | :--- |
    | **Jupyter Notebook** | `http://localhost:8888` | Interactive Spark environment (No token required by default) |
    | **Spark UI** | `http://localhost:8080` | Monitor Spark jobs and applications |
    | **MinIO Console** | `http://localhost:9001` | Manage S3 buckets and files |
    | **Iceberg REST** | `http://localhost:8181` | REST Catalog endpoint |

4.  **Run the Demo**:
    - Open Jupyter Notebook at `http://localhost:8888`.
    - Navigate to the `notebooks` directory.
    - Open `demo.ipynb` to start exploring Iceberg features.

## Configuration Details

### Credentials

The environment uses the following default credentials (configured in `docker-compose.yml`):

*   **AWS Access Key / JDBC User**: `admin`
*   **AWS Secret Key / JDBC Password**: `password`
*   **Postgres DB**: `iceberg`
*   **Region**: `us-east-1`

### Storage Paths

*   **Warehouse**: `s3://warehouse/`
*   **MinIO Endpoint**: `http://minio:9000` (internal), `http://localhost:9000` (host)
*   **Postgres Data**: `./data/postgres` (Local mapping)
*   **MinIO Data**: `./data/minio` (Local mapping)
*   **Notebooks**: `./notebooks` (Local mapping)

## Spark Configuration

The Spark environment is pre-configured via `spark-defaults.conf`. Key settings include:

```properties
spark.sql.extensions                   org.apache.iceberg.spark.extensions.IcebergSparkSessionExtensions
spark.sql.catalog.demo                 org.apache.iceberg.spark.SparkCatalog
spark.sql.catalog.demo.type            rest
spark.sql.catalog.demo.uri             http://rest:8181
spark.sql.catalog.demo.io-impl         org.apache.iceberg.aws.s3.S3FileIO
spark.sql.catalog.demo.warehouse       s3://warehouse/wh/
```

## Useful Commands

*   **Stop the environment**:
    ```bash
    docker-compose down
    ```

*   **View logs**:
    ```bash
    docker-compose logs -f
    ```
