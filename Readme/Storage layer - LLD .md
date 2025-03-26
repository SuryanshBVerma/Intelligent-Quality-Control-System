# Storage Layer LLD

![Storage Service LLD Diagram](https://github.com/SuryanshBVerma/Intelligent-Quality-Control-System/blob/main/LLD%20-%20storage.jpg)

## Component Overview

This low-level design details the storage subsystem of our industrial monitoring solution, which handles persistent data storage for time-series metrics, processed images/videos, and system logs with optimized retrieval capabilities.

## Core Components

### 1. Primary Storage Services

#### Amazon Timestream (Time-Series Database)
- **Purpose**: High-performance storage for sensor metrics
- **Data Characteristics**:
  - High-velocity time-stamped data (1ms granularity)
  - Retention: 30 days hot storage, 1 year cold storage
  - Compression: 10:1 average compression ratio
- **Schema Example**:
  ```sql
  CREATE TABLE machine_metrics (
    machine_id VARCHAR(36),
    metric_name VARCHAR(32),
    value DOUBLE,
    time TIMESTAMP,
    quality INTEGER
  )
  ```
#### AWS S3 (Object Storage)
- **Purpose**: Storage for vision data and system artifacts

- **Bucket Structure**
```
    s3://industrial-monitoring/
    ├── raw-images/
    │   ├── {machine_id}/{YYYY-MM-DD}/{HH-MM-SS}.jpg
    ├── processed-images/
    │   ├── {defect_type}/{YYYY-MM-DD}/annotated_{timestamp}.png
    ├── model-artifacts/
    │   ├── cnn/{version}/
    │   ├── rnn/{version}/
    └── system-logs/
        ├── application/{YYYY-MM-DD}.log
        ├── access/{YYYY-MM-DD}.log
```

#### Kafka (Message Bus)
- **Purpose**: Temporary storage and event buffering

- **Configuration**:

    - **Retention**: 7 days default

    - **Replication**: 3x across availability zones

    - **Partitions**: 16 per topic (scalable)

**Key Topics**:

- **raw.sensor-data** (1hr retention)

- **processed.vision-data** (24hr retention)

- **system.alerts** (7day retention)

