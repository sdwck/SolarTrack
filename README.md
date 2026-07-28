<div align="center">
  <h1>☀️ SolarTrack Core API</h1>
  <p><b>Production-ready IoT Telemetry & Monitoring Backend</b></p>
  <p>A scalable, high-throughput backend system for collecting, processing, and aggregating real-time telemetry from solar panels and battery inverters.</p>
</div>

<p align="center">
  <a href="https://solartrack.runasp.net/"><b>Explore the Live Demo</b></a> •
  <a href="https://github.com/sdwck/SolarTrackUI"><b>Frontend Repository</b></a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/.NET-8.0-512BD4?style=for-the-badge&logo=dotnet" alt=".NET" />
  <img src="https://img.shields.io/badge/C%23-239120?style=for-the-badge&logo=c-sharp&logoColor=white" alt="C#" />
  <img src="https://img.shields.io/badge/MQTT-660066?style=for-the-badge&logo=mqtt&logoColor=white" alt="MQTT" />
  <img src="https://img.shields.io/badge/Architecture-Clean-success?style=for-the-badge" alt="Clean Architecture" />
</p>

---

## 📖 Overview

SolarTrack is built to solve the challenge of ingesting continuous hardware sensor data without dropping packets or overloading the database. It provides a robust API for the frontend dashboards and a background ingestion pipeline for hardware communication.

### 🏗 Architecture

The project strictly follows **Clean Architecture** principles to separate domain logic from infrastructure concerns:

*   **`SolarPanel.API`** — Presentation layer exposing REST endpoints for the web client.
*   **`SolarPanel.Application`** — Business logic, Use Cases, and CQRS handlers.
*   **`SolarPanel.Core`** — Enterprise domain entities, Value Objects, and repository interfaces.
*   **`SolarPanel.Infrastructure`** — Data persistence (EF Core), MQTT Broker integration, and external services.

```mermaid
flowchart LR
    classDef hardware fill:#0f172a,stroke:#6366f1,stroke-width:2px,color:#f4f4f5;
    classDef api fill:#064e3b,stroke:#10b981,stroke-width:2px,color:#f4f4f5;
    classDef db fill:#312e81,stroke:#818cf8,stroke-width:2px,color:#f4f4f5;
    classDef ui fill:#7f1d1d,stroke:#f87171,stroke-width:2px,color:#f4f4f5;

    HW["Inverters & Panels<br/>(IoT Sensors)"]:::hardware -- MQTT --> BROKER["MQTT Broker"]:::hardware
    BROKER -- Subscribe --> WORKER["SolarTrack Background<br/>Ingestion Service"]:::api
    WORKER -- Persist & Aggregate --> DB[(PostgreSQL)]:::db
    
    DB -. Query .-> API["SolarTrack REST API"]:::api
    API -- In-Memory Cache --> API
    API -- JSON --> UI["React Dashboard<br/>(SolarTrackUI)"]:::ui
```

## ⚡ Key Features

*   **High-Throughput MQTT Pipeline:** Asynchronously consumes hardware telemetry without blocking the main HTTP threads.
*   **In-Memory Caching Strategy:** Aggregated metrics (daily production, efficiency) are cached in-memory to ensure millisecond-level response times for the frontend dashboard.
*   **Historical Data Aggregation:** Automatically groups raw time-series data into readable formats (hourly, daily, monthly) for chart rendering.
*   **Maintenance & Alerts Logic:** Backend evaluation of threshold breaches (e.g., over-voltage, temperature spikes) to trigger system alerts.

## 💻 Related Repositories

This backend powers the **SolarTrack UI**. You can find the React + TypeScript frontend code here:  
👉 **[github.com/sdwck/SolarTrackUI](https://github.com/sdwck/SolarTrackUI)**

---

## 📸 System Previews

<details>
<summary><b>Click to expand Dashboard Screenshots</b></summary>
<br>

| Main Dashboard | Power Flow Visualization |
| :---: | :---: |
| <img src="https://github.com/user-attachments/assets/9900938a-b8a5-4a0e-8931-442437e883af" width="400" /> | <img src="https://github.com/user-attachments/assets/bad23d59-3e9b-48d3-beb6-11b9931cce24" width="400" /> |

| System Health & Diagnostics | Battery Analytics |
| :---: | :---: |
| <img src="https://github.com/user-attachments/assets/aeb9dfb6-418d-4463-b573-53553b8fc604" width="400" /> | <img src="https://github.com/user-attachments/assets/2fcf7aab-7171-4a36-9400-ab794e5d7236" width="400" /> |

| Analytics & Comparisons | Energy History |
| :---: | :---: |
| <img src="https://github.com/user-attachments/assets/0db82540-c650-4945-889c-f412f2e2cccf" width="400" /> | <img src="https://github.com/user-attachments/assets/6779b139-7992-461c-8551-9beb171c2fb7" width="400" /> |

| Maintenance Management |
| :---: |
| <img src="https://github.com/user-attachments/assets/4a038814-7b3f-40f2-99f1-0d8bef16a188" width="400" /> |

</details>
