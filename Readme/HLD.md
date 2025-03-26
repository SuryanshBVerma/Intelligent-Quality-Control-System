# High Level Design

![High Level Design Diagram](https://github.com/SuryanshBVerma/Intelligent-Quality-Control-System/blob/main/HLD.jpg)

## System Overview

This high-level design describes an industrial monitoring system that combines computer vision and IoT sensor data to detect equipment anomalies and production defects in real-time. The system processes multi-modal data streams, applies AI analysis, and triggers appropriate responses through notifications, storage, and control functions.

## Architecture Components

### 1. Data Collection Layer
- **Vision Data**:
  - High-resolution cameras capturing production line images
  - Continuous video streams at 5-30 FPS (configurable)
  - Supports both RGB and thermal imaging

- **Sensor Data**:
  - IoT sensors monitoring equipment parameters
  - Time-series data (vibration, temperature, pressure, etc.)
  - Sampling rates from 1Hz to 1kHz depending on criticality

### 2. Data Preprocessing Pipeline
- **Vision Pipeline**:
  - Image normalization and enhancement
  - Frame selection and buffering
  - Region-of-interest extraction

- **Sensor Pipeline**:
  - Data validation and cleaning
  - Outlier detection
  - Time-window aggregation

### 3. AI Processing Core
- **CNN (Convolutional Neural Network)**:
  - Processes vision data for defect detection
  - Model architectures: YOLO (v8) for real-time processing
  - Outputs: Defect classification and localization

- **RNN (Recurrent Neural Network)**:
  - Analyzes time-series sensor data
  - LSTM networks for anomaly detection
  - Outputs: Equipment health scores and failure predictions

### 4. Messaging and Integration
- **Kafka Message Bus**:
  - Central event distribution hub
  - Topics for different event types and priorities
  - Guaranteed message delivery

### 5. Output Services
- **Notification Service (Twilio)**:
  - SMS/voice alerts for critical events
  - Escalation policies based on severity
  - Multi-channel notification support

- **Storage Services**:
  - **TSDB (Time Series Database)**:
    - Optimized for sensor metrics
    - High-frequency data storage
    - Efficient time-range queries
  - **Object Storage**:
    - Stores processed images and videos
    - Metadata indexing for search

- **Cloud Functions (AWS Lambda)**:
  - Event-driven processing
  - Machine control signal generation
  - Data transformation pipelines

### 6. User Interface
- Real-time monitoring dashboard
- Historical data visualization
- Alert management console
- System configuration interface

## Data Flow

1. **Ingestion**:
   - Raw data collected from vision and sensor sources
   - Initial validation and timestamping

2. **Preprocessing**:
   - Data type-specific transformations
   - Quality checks and error handling

3. **AI Processing**:
   - Parallel processing of vision and sensor streams
   - Model inference with confidence scoring
   - Event generation for detected issues

4. **Distribution**:
   - Events published to appropriate Kafka topics
   - Fan-out to multiple consumers

5. **Action**:
   - Notifications dispatched via Twilio
   - Data persisted in optimized storage
   - Control signals generated via Lambda

6. **Visualization**:
   - Real-time updates in UI
   - Historical trend analysis

