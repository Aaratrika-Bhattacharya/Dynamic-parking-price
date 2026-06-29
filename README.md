# 🅿️ Real-Time Dynamic Parking Pricing System

A real-time parking price optimization engine built with **Pathway**, **Random Forest**, and **Python** — simulating live occupancy streams across 14 parking lots and forecasting future demand with per-lot ML models.

---

## 📌 Overview

Static parking prices fail to account for real-world demand fluctuations. This project builds a **streaming data pipeline** that dynamically adjusts parking prices based on live occupancy, queue length, traffic, and vehicle type — and forecasts future demand using machine learning.

---

## 🏗️ Architecture

```
CSV Dataset (18K+ records)
        │
        ▼
┌───────────────────┐
│  Pathway Stream   │  ← Simulates real-time row-by-row ingestion
│  (mode=streaming) │
└────────┬──────────┘
         │
         ▼
┌───────────────────┐
│  Feature Engine   │  ← OccupancyRate, lag features, time features
└────────┬──────────┘
         │
    ┌────┴────┐
    ▼         ▼
┌────────┐ ┌──────────────────┐
│Dynamic │ │  Random Forest   │
│Pricing │ │  Demand Forecast │
│Formula │ │  (per lot)       │
└────────┘ └──────────────────┘
         │
         ▼
┌───────────────────┐
│  Output CSV +     │
│  Matplotlib       │
│  Dashboard        │
└───────────────────┘
```

---

## 🧠 Pricing Model

Price is computed using a **nonlinear surge formula**:

```
Price = BASE_PRICE
      × (1 + 2.0 × OccupancyRate²)   ← exponential surge at high load
      × (1 + 0.05 × QueueLength)      ← urgency premium
      × (1 + 0.10 × TrafficLevel)     ← congestion premium
      × (1.15 if SpecialDay else 1.0) ← event premium
      × VehicleTypeWeight             ← vehicle class adjustment
```

Clipped between **₹5 (min)** and **₹50 (max)**.

---

## 📊 Dataset

| Field | Description |
|---|---|
| `Timestamp` | Observation datetime |
| `SystemCodeNumber` | Parking lot ID (14 unique lots) |
| `Capacity` | Total spaces in the lot |
| `Occupancy` | Current occupied spaces |
| `QueueLength` | Vehicles waiting to enter |
| `TrafficLevel` | External traffic indicator (0/1) |
| `IsSpecialDay` | Public holiday or event flag (0/1) |
| `VehicleTypeWeight` | Weight multiplier by vehicle class |
| `Latitude / Longitude` | Lot geolocation |

**Size:** ~18,000 records across 14 lots

---

## ⚙️ Tech Stack

| Component | Technology |
|---|---|
| Streaming pipeline | [Pathway](https://pathway.com/) |
| Demand forecasting | scikit-learn RandomForestRegressor |
| Data processing | Pandas, NumPy |
| Visualization | Matplotlib |
| Environment | Google Colab |

---

## 🚀 How to Run

### 1. Install dependencies
```bash
pip install pathway pandas numpy matplotlib scikit-learn
```

### 2. Upload dataset
Upload `cleaned_dataset.csv` via Colab's file upload or mount Google Drive.

### 3. Run the notebook cells in order

| Step | Description |
|---|---|
| Steps 1–2 | Install + upload |
| Steps 3–4 | EDA + feature engineering |
| Step 5 | Define pricing formula |
| Steps 6–7 | Pathway static pipeline → output CSV |
| Steps 8–9 | Train per-lot RF models + forecast prices |
| Step 10 | Matplotlib dashboard |
| Step 11 | Live streaming simulation (Pathway watch mode) |
| Step 12 | Summary stats across all lots |

### 4. Verify streaming output
```python
import pandas as pd
live_result = pd.read_csv("./live_output.csv")
print(f"Rows processed: {len(live_result)}")
print(live_result[["Timestamp","SystemCodeNumber","OccupancyRate","DynamicPrice"]].tail(5))
```

---

## 📈 Sample Output

```
Lot                   Avg Occ Rate   Avg Price   Max Price   Forecast MAE
BHMBCCMKT01           0.412          ₹14.23      ₹38.50      0.0312
Others-CCCPS135a      0.631          ₹21.07      ₹48.90      0.0287
Shopping              0.589          ₹19.44      ₹45.20      0.0341
Broad Street          0.471          ₹15.80      ₹41.60      0.0298
```

---

## 📁 Project Structure

```
├── cleaned_dataset.csv          # Raw input dataset
├── pathway_priced_output.csv    # Pathway static pipeline output
├── live_parking_stream.csv      # Live streaming input (auto-generated)
├── live_output.csv              # Live streaming output (auto-generated)
├── parking_summary.csv          # Per-lot summary statistics
├── parking_dashboard.png        # Visualization output
└── dynamic_parking_pricing.ipynb  # Main Colab notebook
```

---

## 🔑 Key Design Decisions

**Pathway streaming modes** — `mode="static"` and `mode="streaming"` share the same pipeline code, making the system trivially switchable between batch and real-time ingestion.

**Per-lot forecasting** — Separate Random Forest models per lot prevent data leakage across locations with different capacity and usage patterns.

**Nonlinear surge pricing** — The occupancy² term models real-world surge behaviour where prices rise sharply only above ~80% load, not linearly from the start.

**Multithreaded simulation** — A daemon feeder thread appends rows to a watched CSV while Pathway processes them concurrently, replicating a Kafka-style streaming setup without additional infrastructure.

---

## 🔭 Future Improvements

- Replace CSV streaming with Kafka or Redpanda as the message broker
- Add geospatial competitor pricing (adjust price based on nearby lot rates)
- Deploy Pathway pipeline as a Docker service with a REST API for price queries
- Extend forecasting to multi-step horizon (predict next 3–6 intervals)
- Add Bokeh/Plotly interactive dashboard for live price monitoring

---

## 👩‍💻 Author

**Aaratrika**  
B.Tech — Software Engineering  
Portfolio projects targeting SDE & Data Analyst roles
