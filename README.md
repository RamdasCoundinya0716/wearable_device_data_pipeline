````markdown
# 🏋️‍♂️ Wearable Device Lakehouse Platform (Whoop-like)

This project simulates a lakehouse data platform for a fitness wearable company that tracks user profiles, heart rate (BPM), gym visits, and workout activity. The architecture follows the **Medallion (Bronze-Silver-Gold)** pattern using **Databricks Lakehouse** and **Azure Data Lake Storage Gen2**, incorporating **streaming ingestion via Kafka**, **Delta Lake**, and **CI/CD automation**.

---

## 📘 Project Scope

Users purchase a fitness wearable device that transmits various activity and health metrics to the company's cloud infrastructure:

### Registered Device Info
Stored in a cloud database (batch ingestion):
- `user_id`
- `device_id`
- `mac_address`
- `registration_timestamp`

### User Profile Events (CDC via Kafka)
Captured from mobile app as key-value events in Kafka topic:
```json
{
  "user_id": "string",
  "update_type": "create/update/delete",
  "timestamp": "datetime",
  "date_of_birth": "date",
  "sex": "string",
  "gender": "string",
  "first_name": "string",
  "last_name": "string",
  "address": {
    "street": "string",
    "city": "string",
    "state": "string",
    "country": "string",
    "zip": "string"
  }
}
````

### Heart Rate Stream (BPM via Kafka)

Continuously streamed from the device:

* `device_id`
* `time`
* `heart_rate`

### Gym Visit Events

Sent as structured events (batch or stream):

* `mac_address`
* `gym`
* `login_timestamp`
* `logout_timestamp`

### Workout Actions (Start/Stop via Kafka)

Separate topics for start/stop:

* `user_id`
* `workout_id`
* `action` (`start`/`stop`)
* `session_id`

---

## 🏗️ Architecture Design

### 🏛️ Lakehouse Architecture (Medallion)

* `Bronze`: Raw ingested data (stream/batch)
* `Silver`: Cleansed and structured data
* `Gold`: Aggregated and analytics-ready datasets

### 🔐 Storage Layout in ADLS Gen2

```
/sbit-meta-store        # Metadata storage
/sbit-managed-dev       # Raw + processed data
    /bronze
    /silver
    /gold
/sbit-unmanaged-dev     # Checkpoints and temp data
```

### Why ADLS Gen2?

* Optimized for Delta Lake & Spark workloads
* High availability & durability
* Hierarchical directory structure
* Fine-grained access control with Unity Catalog

---

## ⚙️ Data Ingestion & Processing

### Ingestion

* **Batch**: Registered devices and gym login data
* **Streaming**: Kafka topics for BPM, user profile events, workout start/stop

### Processing

* Decoupled ingestion and transformation pipelines
* Delta Live Tables and Structured Streaming for real-time updates
* Scalable notebook jobs on Databricks clusters

---

## 📊 Analytical Datasets (Gold Layer)

1. **Workout BPM Summary**

   * Max, Min, and Avg heart rate per session

2. **Gym Summary**

   * Time spent in gym (login to logout)
   * Actual workout duration (start to stop)

---

## 🧪 Testing & Deployment

* 🔄 **CI/CD Pipelines**: Automated deployment to test and prod environments
* 🧪 **Integration Testing**: For data ingestion and transformations
* 🌐 **Environment Management**: Isolated dev/test/prod using Unity Catalog permissions

---

## ✅ Design Goals & Best Practices

* Secure platform with Unity Catalog for RBAC
* Medallion architecture to ensure clean data layers
* Batch + Streaming support using Delta
* Decoupled pipeline design for modular development
* Reusable, testable and automatable notebooks

---

## 🚀 Future Enhancements

* Alerting for abnormal BPM patterns
* Predictive analytics on fitness trends
* API gateway for BI dashboards and external apps

---

## 🧠 Author

**Ramdas Coudinya VK**
📧 [ramdasvk4@gmail.com](mailto:ramdasvk4@gmail.com)
🔗 [LinkedIn](https://www.linkedin.com/in/ramdascoudinya) | [GitHub](https://github.com/RamdasCoundinya0716)
