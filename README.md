# 🚗 Real-Time Dynamic Parking Pricing System

A real-time dynamic parking pricing system that computes adaptive parking fees based on live demand indicators such as occupancy, queue length, traffic conditions, special events, and vehicle characteristics. The project combines demand modeling with real-time data processing concepts using Pathway and Python to simulate an intelligent parking management system.

---

## 📌 Project Overview

Traditional parking systems often use fixed pricing irrespective of demand, leading to inefficient space utilization and congestion. This project implements a demand-driven pricing engine that dynamically adjusts parking prices using multiple real-world factors.

The system:
- Calculates a composite demand score from multiple features.
- Generates adaptive parking prices based on current demand.
- Applies price smoothing to avoid abrupt fluctuations.
- Simulates real-time pricing updates.
- Visualizes pricing trends for operational insights.

---

## ✨ Features

- 📈 Multi-factor demand score calculation
- 🚗 Dynamic parking price generation
- 📊 Occupancy and demand analytics
- 🔄 Real-time pricing simulation
- ⚡ Stream processing concepts using Pathway
- 📉 Price smoothing for stable pricing
- 📍 Parking-lot level pricing analysis

---

## 🛠️ Tech Stack

- Python
- Pathway
- Pandas
- NumPy
- Matplotlib
- Google Colab

---

## 📂 Dataset

The dataset contains over **18,000 parking records** with the following attributes:

| Feature | Description |
|----------|-------------|
| Timestamp | Time of observation |
| SystemCodeNumber | Parking lot identifier |
| Capacity | Total parking capacity |
| Occupancy | Current occupied spaces |
| QueueLength | Vehicles waiting |
| TrafficLevel | Nearby traffic condition |
| IsSpecialDay | Event/Holiday indicator |
| VehicleTypeWeight | Vehicle demand weight |
| Latitude | Parking location |
| Longitude | Parking location |

---

## ⚙️ Pricing Algorithm

### Step 1 — Demand Score

The system computes a normalized demand score using multiple operational factors.

Demand Score incorporates:

- Occupancy Ratio
- Queue Length
- Traffic Level
- Special Day Indicator
- Vehicle Type Weight

Weighted demand model:

Occupancy Ratio → 45%

Queue Length → 20%

Traffic Level → 15%

Vehicle Type → 10%

Special Day → 10%

---

### Step 2 — Dynamic Pricing

Parking price is calculated using the demand score:

```
Dynamic Price = Base Price + (Max Price − Base Price) × Demand Score
```

where

- Base Price = ₹20
- Maximum Price = ₹60

---

### Step 3 — Price Smoothing

To prevent sudden fluctuations, prices are smoothed using exponential smoothing:

```
Smoothed Price =
Previous Price +
α(Current Price − Previous Price)
```

where

```
α = 0.30
```

---

## 📊 Workflow

```
Parking Dataset
        │
        ▼
Data Cleaning
        │
        ▼
Demand Score Calculation
        │
        ▼
Dynamic Pricing Engine
        │
        ▼
Price Smoothing
        │
        ▼
Real-Time Simulation
        │
        ▼
Visualization
```

---

## 📈 Visualizations

The project includes:

- Occupancy Distribution
- Dynamic Price Trend
- Smoothed Price Trend
- Demand Score Analysis

---

## ▶️ How to Run

1. Open the notebook in Google Colab.
2. Install the required libraries:

```bash
pip install pathway pandas numpy matplotlib
```

3. Upload the parking dataset.

4. Execute all notebook cells sequentially.

---

## 📁 Project Structure

```
Dynamic-Parking-Pricing/
│
├── Dynamic_Parking_Pricing.ipynb
├── parking_stream.csv
├── README.md
└── images/
```

---

## 🚀 Future Improvements

- Live dashboard using Streamlit
- Kafka-based real-time streaming
- Competitor-aware pricing using geospatial distance
- Time-series demand forecasting
- REST API deployment
- Docker containerization

---

## 📚 Key Learnings

- Real-time data processing concepts
- Demand-based pricing strategies
- Feature engineering
- Data visualization
- Stream processing with Pathway
- Pricing model design

---

## 👤 Author

**Aaratrika Bhattacharya**

IIT Kharagpur
