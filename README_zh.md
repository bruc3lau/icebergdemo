# Apache Iceberg Demo with Spark and MinIO

本项目提供了一个完整且易于使用的 Docker 环境，用于使用 **Apache Spark** 探索和演示 **Apache Iceberg** 的功能。它包含一个 REST Catalog、一个兼容 S3 的对象存储 (MinIO) 以及一个用于持久化 Catalog 的 PostgreSQL 数据库。

## 项目架构

该环境由 Docker Compose 编排的以下服务组成：

*   **`spark-iceberg`**: 预配置了 Iceberg 支持和 Jupyter Notebook 的 Apache Spark 容器。
*   **`iceberg-rest`**: Iceberg REST Catalog 服务。
*   **`minio`**: 用于数据仓库的 S3 兼容对象存储。
*   **`postgres-iceberg`**: Iceberg REST Catalog 的 PostgreSQL 数据库后端。

## 前置条件

*   [Docker](https://docs.docker.com/get-docker/)
*   [Docker Compose](https://docs.docker.com/compose/install/)

## 快速开始

1.  **克隆仓库**:
    ```bash
    git clone <repository-url>
    cd icebergdemo
    ```

2.  **启动环境**:
    ```bash
    docker-compose up -d
    ```

3.  **访问服务**:

    | 服务 | URL | 描述 |
    | :--- | :--- | :--- |
    | **Jupyter Notebook** | `http://localhost:8888` | 交互式 Spark 环境 (默认无需 Token) |
    | **Spark UI** | `http://localhost:8080` | 监控 Spark 作业和应用 |
    | **MinIO Console** | `http://localhost:9001` | 管理 S3 存储桶和文件 |
    | **Iceberg REST** | `http://localhost:8181` | REST Catalog 端点 |

4.  **运行演示**:
    - 在浏览器打开 Jupyter Notebook: `http://localhost:8888`。
    - 进入 `notebooks` 目录。
    - 打开 `demo.ipynb` 开始探索 Iceberg 功能。

## 配置详情

### 凭证

环境使用以下默认凭证 (在 `docker-compose.yml` 中配置):

*   **AWS Access Key / JDBC User**: `admin`
*   **AWS Secret Key / JDBC Password**: `password`
*   **Postgres DB**: `iceberg`
*   **Region**: `us-east-1`

### 存储路径

*   **Warehouse**: `s3://warehouse/`
*   **MinIO Endpoint**: `http://minio:9000` (内部), `http://localhost:9000` (宿主机)
*   **Postgres Data**: `./data/postgres` (本地映射)
*   **MinIO Data**: `./data/minio` (本地映射)
*   **Notebooks**: `./notebooks` (本地映射)

## Spark 配置

Spark 环境通过 `spark-defaults.conf` 进行了预配置。关键设置包括:

```properties
spark.sql.extensions                   org.apache.iceberg.spark.extensions.IcebergSparkSessionExtensions
spark.sql.catalog.demo                 org.apache.iceberg.spark.SparkCatalog
spark.sql.catalog.demo.type            rest
spark.sql.catalog.demo.uri             http://rest:8181
spark.sql.catalog.demo.io-impl         org.apache.iceberg.aws.s3.S3FileIO
spark.sql.catalog.demo.warehouse       s3://warehouse/wh/
```

## 常用命令

*   **停止环境**:
    ```bash
    docker-compose down
    ```

*   **查看日志**:
    ```bash
    docker-compose logs -f
    ```
