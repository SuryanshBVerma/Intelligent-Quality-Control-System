# Notification Service LLD

![Notification Service LLD Diagram](LLD-Notification.jpg)

## Component Overview

This low-level design details the notification subsystem of our industrial monitoring solution, which processes AI-generated alerts and delivers targeted notifications through multiple channels based on configurable business rules.

## Core Components

### 1. AI Processing Input Layer
- **Processed Data from AI Models**:
  - Structured JSON payloads containing:
    - Defect classifications (from CNN vision processing)
    - Anomaly scores (from RNN sensor analysis)
    - Confidence levels and timestamps
  - Example payload:
    ```json
    {
      "event_id": "defect-3872",
      "machine_id": "press-12",
      "defect_type": "crack",
      "confidence": 0.92,
      "timestamp": "2023-07-15T14:32:18Z",
      "sensor_metrics": {
        "vibration": 8.7,
        "temperature": 142.3
      }
    }
    ```

### 2. Rules Engine
- **Rule-Based Engine**:
  - Evaluates incoming events against configured rules
  - Sample rule:
    ```python
    IF defect_type == "crack" AND confidence > 0.85 THEN
      severity = "CRITICAL"
      notify_channels = ["SMS", "PAGER"]
      escalation_path = ["shift_manager", "maintenance_lead"]
    ```

- **AVISIOT Rules Engine** (Advanced Visual IoT):
  - Specialized for industrial IoT patterns
  - Features:
    - Temporal pattern matching (e.g., "3 high-vibration events in 5 minutes")
    - Equipment-specific rule sets
    - Maintenance history integration

### 3. AI-Enhanced Processing
- **Amazon SageMaker**:
  - Secondary AI processing for:
    - Notification priority prediction
    - Optimal channel selection
    - Recipient recommendation
  - Uses:
    - Historical response data
    - Operator availability patterns
    - Shift schedule context

### 4. Notification Dispatcher
- **Multi-Channel Support**:
  - SMS (via Twilio)
  - Email (SMTP)
  - Mobile push (Firebase)
  - Pager systems
  - On-site alarms

- **Features**:
  - Escalation policies
  - Acknowledgement tracking
  - Delivery receipts
  - Failover mechanisms

## Data Flow

1. **Input Reception**:
   - Receives processed events from Kafka topics
   - Validates message schema
   - Adds system metadata

2. **Rules Evaluation**:
   - Matches events against rule sets
   - Determines:
     - Severity level
     - Notification channels
     - Recipient lists
     - Response requirements

3. **AI Enhancement**:
   - Augments rules with predictive insights
   - Optimizes delivery timing
   - Personalizes message content

4. **Notification Delivery**:
   - Formats messages per channel
   - Handles retries and failures
   - Logs delivery outcomes

5. **Feedback Loop**:
   - Captures acknowledgements
   - Tracks response times
   - Updates AI models

## Configuration Options

### Rule Configuration
```yaml
rules:
  - name: high_temp_alert
    condition: temperature > threshold
    params:
      threshold: 150
    actions:
      - notify: "shift_operator"
        channels: ["SMS", "dashboard"]
        message: "High temperature alert on {machine_id}"