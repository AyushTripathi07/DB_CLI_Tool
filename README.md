# 🔄 Database Backup CLI Tool

A powerful, flexible command-line utility designed to automate and simplify database backup operations across multiple database systems. This tool helps you maintain data integrity with scheduled backups, cloud storage integration, and comprehensive logging.

[![Python Version](https://img.shields.io/badge/python-3.6%2B-blue)](https://www.python.org/)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![Docker Support](https://img.shields.io/badge/docker-supported-brightgreen)](https://www.docker.com/)

## ✨ Features

- **Multi-Database Support**: Backup and restore operations for:
  - MySQL
  - PostgreSQL
  - MongoDB
  - SQLite
  - Neo4j
- **Docker Integration**: Simplified deployment and consistent environment across platforms
- **Cloud Storage Integration**:
  - AWS S3
  - Google Cloud Storage
  - Microsoft Azure
  - BackBlaze B2 (S3-compatible)
- **Advanced Options**:
  - Backup compression
  - Scheduled periodic backups
  - On-demand backup retrieval
  - Slack notifications
  - Comprehensive logging

## 📋 Table of Contents

- [Installation](#-installation)
  - [Standard Installation](#standard-installation)
  - [Docker Installation](#docker-installation)
- [Configuration](#-configuration)
- [Usage](#-usage)
  - [Basic Commands](#basic-commands)
  - [Command Arguments](#command-arguments)
  - [Examples](#examples)
- [Use Cases](#-use-cases)
- [Logging](#-logging)
- [Contributing](#-contributing)
- [License](#-license)

## 🚀 Installation

### Standard Installation

1. **Clone the repository**:
   ```bash
   git clone {repo_url}
   cd DB_CLI_Tool
   ```

2. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

### Docker Installation

1. **Pull the Docker image**:
   ```bash
   docker pull saiganesh03/db-backup-utility
   ```

2. **Or build from Dockerfile**:
   ```bash
   docker build -t db-backup-utility .
   ```

## ⚙️ Configuration

Create a `config.json` file with your database and cloud storage credentials:

```json
{
    "database": {
        "host": "localhost",
        "port": 3306,
        "user": "your_username",
        "password": "your_password",
        "database": "your_database"
    },
    "aws": {
        "access_key": "your_access_key",
        "secret_key": "your_secret_key",
        "region": "us-west-2"
    },
    "backblaze": {
        "account_id": "your_account_id",
        "application_key": "your_application_key",
        "bucket_name": "your_bucket_name",
        "endpoint": "s3.us-west-000.backblazeb2.com"
    },
    "gcs": {
        "service_account_key": "path_to_your_service_account_key.json"
    },
    "azure": {
        "connection_string": "your_connection_string"
    },
    "slack_webhook": "https://hooks.slack.com/services/T00000000/B00000000/XXXXXXXXXXXXXXXXXXXXXXXX",
    "schedule": {
        "frequency": "daily",
        "time": "03:00",
        "retention_days": 30
    }
}
```

## 💻 Usage

### Basic Commands

```bash
# For standard installation
python backup_cli.py [operation] [options]

# For Docker installation
docker run -v /path/to/config.json:/app/config.json -v /backup/destination:/backup db-backup-utility [operation] [options]
```

### Command Arguments

| Argument | Description |
|----------|-------------|
| `operation` | Choose from `backup`, `restore`, or `schedule` |
| `--db-type` | Database type (`mysql`, `postgresql`, `mongodb`, `sqlite`, `neo4j`) |
| `--config` | Path to configuration file |
| `--output` | Output path for backup file |
| `--compress` | Enable backup compression |
| `--backup-file` | Path to backup file for restoration |
| `--cloud` | Cloud storage provider (`s3`, `gcs`, `azure`, `backblaze`) |
| `--bucket` | Cloud storage bucket name |
| `--fetch` | Retrieve a specific backup from cloud storage |
| `--schedule` | Set up recurring backups (hourly, daily, weekly) |
| `--log-file` | Path to log file (default: `backup.log`) |

### Examples

#### 1. Create a MySQL backup and upload to S3

```bash
python backup_cli.py backup --db-type mysql --config config.json --output /backups/mysql_backup.sql --compress --cloud s3 --bucket my-database-backups
```

#### 2. Restore a PostgreSQL database

```bash
python backup_cli.py restore --db-type postgresql --config config.json --backup-file /backups/postgres_backup.sql
```

#### 3. Backup a Neo4j database with Docker

```bash
docker run -v /path/to/config.json:/app/config.json -v /backup/destination:/backup db-backup-utility backup --db-type neo4j --config /app/config.json --output /backup/neo4j_backup.dump --compress
```

#### 4. Schedule daily MongoDB backups

```bash
python backup_cli.py schedule --db-type mongodb --config config.json --output /backups/mongo_backup_%date%.bson --compress --cloud backblaze --bucket mongo-backups --frequency daily --time 02:00
```

#### 5. Fetch a specific backup from BackBlaze

```bash
python backup_cli.py fetch --cloud backblaze --bucket my-backups --config config.json --backup-file postgres_backup_2023-03-15.sql.gz --output /restored/
```

## 🔍 Use Cases

1. **Disaster Recovery Planning**: Maintain regular, compressed backups in multiple cloud locations to prepare for potential data loss.

2. **Database Migration**: Easily create backups before migrating databases between environments.

3. **Automated DevOps Workflows**: Integrate with CI/CD pipelines to automate backup processes before deployments.

4. **Multi-Environment Support**: Use Docker containers to maintain consistent backup procedures across development, staging, and production.

5. **Compliance Requirements**: Meet data retention policies with scheduled backups and configurable retention periods.

## 📝 Logging

The tool records all operations in a detailed log file (default: `backup.log`). This log includes:

- Timestamp for each operation
- Success/failure status
- Error messages and stack traces for troubleshooting
- Storage locations and file sizes
- Duration of backup/restore operations

Example log output:
```
2023-03-15 02:00:01 INFO - Starting backup of database 'production_db' (PostgreSQL)
2023-03-15 02:01:45 INFO - Backup completed: 156MB uncompressed
2023-03-15 02:01:50 INFO - Compression reduced size to 42MB (73% reduction)
2023-03-15 02:02:30 INFO - Successfully uploaded to BackBlaze bucket 'postgres-backups'
2023-03-15 02:02:31 INFO - Slack notification sent
```

## 👥 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request
