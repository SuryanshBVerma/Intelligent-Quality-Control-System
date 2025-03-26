# Context Diagram

This system provides real-time monitoring of production line equipment by combining computer vision and IoT sensor data to detect defects and anomalies, triggering alerts and automated responses.

## Context Diagram

![Context Diagram]([Context-Process.png](http://github.com/SuryanshBVerma/Intelligent-Quality-Control-System/blob/main/Context%20Diagram.png))

## System Overview

This industrial monitoring solution integrates:
- **Vision System**: For visual inspection of production line components
- **IoT Sensors**: For real-time equipment monitoring
- **AI Processing**: To detect defects and anomalies
- **Alerting System**: For immediate operator notification
- **Data Storage**: For historical analysis and reporting

## Key Components

### 1. Data Sources
- **Vision System**: High-resolution cameras capturing continuous images of production line components
- **IoT Sensors**: Embedded sensors monitoring equipment parameters (temperature, vibration, pressure, etc.)

### 2. Data Collection Layer
- Receives raw image streams from vision system
- Collects time-series data from multiple IoT sensors
- Normalizes different data formats for processing

### 3. Preprocessing Pipelines
- **Vision Pipeline**:
  - Image normalization and enhancement
  - Defect segmentation
  - Feature extraction
- **Sensor Pipeline**:
  - Data cleaning and smoothing
  - Anomaly feature detection
  - Time-series analysis

### 4. AI Processing
- **Computer Vision Models**: CNN-based models for defect detection in images
- **Anomaly Detection**: RNN/LSTM models for identifying abnormal patterns in sensor data
- Generates two event types:
  - Defect Events (from vision system)
  - Anomaly Events (from sensor data)

### 5. Event Processing
- All events published to Kafka message bus
- Priority classification (Normal/High-priority)
- Routing to appropriate downstream systems

### 6. Output Services
- **Twilio**: Sends SMS/voice alerts to:
  - Factory Operators (for immediate action)
  - Maintenance Admins (for scheduled repairs)
- **AWS Lambda**: Executes automated responses:
  - Machine control signals to adjust production line
  - Triggers maintenance workflows
- **Data Storage**:
  - Amazon Timestream: For time-series metrics (queryable)
  - AWS S3: For log data and raw images
- **User Interface**: Dashboard for:
  - Real-time monitoring
  - Historical data analysis
  - Alert management

## Workflow

1. **Data Acquisition**:
   - Cameras capture images of products/machines
   - Sensors collect equipment metrics

2. **Processing**:
   - Images analyzed for visual defects
   - Sensor data checked for anomalies

3. **Event Handling**:
   - Normal events logged for historical analysis
   - Critical events trigger alerts and actions

4. **Response**:
   - Operators receive immediate notifications
   - Machines automatically adjust when possible
   - Maintenance tickets created for serious issues

## Integration Points

- **Production Line Machines**:
  - Receives control signals from Lambda functions
  - May provide additional diagnostic data

- **User Roles**:
  - **Factory Operators**: Primary alert recipients
  - **Maintenance Admins**: Secondary notifications for planning

## Deployment Architecture

- Cloud-based processing (AWS)
- Edge devices for data collection
- Hybrid on-premises/cloud deployment possible

## Monitoring Capabilities

1. **Real-time**:
   - Equipment status
   - Defect rates
   - Alert dashboard

2. **Historical**:
   - Trend analysis
   - Failure prediction
   - Quality control reporting

