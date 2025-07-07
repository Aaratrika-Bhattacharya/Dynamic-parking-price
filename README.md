# Dynamic-parking-price
It is designed to dynamically calculate parking prices based on real-time occupancy and capacity data for multiple parking spaces.


## Overview

Model 1 is designed to dynamically calculate parking prices based on real-time occupancy and capacity data for multiple parking spaces. The workflow leverages Python (Pandas) for data processing and Pathway for scalable, schema-driven, and streaming data operations.

## Stepwise Process

### 1. Data Ingestion

- **Input:** A CSV file (`dataset.csv`) containing parking data with columns such as `ID`, `SystemCodeNumber`, `Capacity`, `Occupancy`, `Timestamp`, and other metadata.
- **Tools:** Pandas, Pathway.

### 2. Preprocessing and Sorting

- **Sort** the DataFrame by `ID` and `Timestamp` to ensure chronological order for each parking space.
- **Group** the data by `ID` so that each parking space's time series is processed independently.

### 3. Price Calculation (Pandas)

- **Initialize Parameters:**  
  - `alpha`: Sensitivity coefficient for demand (e.g., 1.0).
  - `base_price`: Minimum starting price (e.g., 10.0).
- **Iterate** over each group (parking space):
  - For the first timestamp, set price = `base_price`.
  - For subsequent rows:
    - If `Capacity` is zero, carry forward the previous price.
    - Else, calculate:
      ```
      price = previous_price + alpha * (Occupancy / Capacity)
      ```
  - **Append** the calculated price to a new `Price` column.

### 4. Output Intermediate Results

- **Save** the updated DataFrame (with the new `Price` column) as `model1_with_price.csv`.

### 5. Load Data into Pathway

- **Define a Schema** in Pathway to match the CSV structure, including the new `Price` column.
- **Read** the CSV into a Pathway table using the defined schema.

### 6. Further Processing or Export (Pathway)

- **Optionally** perform additional streaming, filtering, or aggregation in Pathway.
- **Write** the final processed table to `model1_pathway_final.csv`.

## Technology Architecture

| Layer/Step            | Technology         | Purpose                                          |
|-----------------------|--------------------|--------------------------------------------------|
| Data Ingestion        | Pandas             | CSV reading, initial DataFrame creation          |
| Preprocessing         | Pandas             | Sorting, grouping, row-wise price computation    |
| Intermediate Storage  | CSV                | Exchange format between Pandas and Pathway       |
| Schema Enforcement    | Pathway            | Strong typing, validation, and further processing|
| Stream Processing     | Pathway            | Scalable, declarative data pipeline              |
| Output                | CSV                | Final results for downstream use                 |

## Key Points

- **Vectorization:** The price calculation is row-wise and depends on previous values; thus, it is implemented as a loop within each group.
- **Separation of Concerns:** Pandas is used for batch calculations, while Pathway handles schema validation and scalable processing.
- **Modularity:** Each step can be adapted or extended (e.g., to add new features or integrate with real-time data sources).

## How to Run

1. Place your input data in `dataset.csv`.
2. Run the Python script to compute prices and generate `model1_with_price.csv`.
3. Use Pathway to read the CSV, apply schema, and output the final results as `model1_pathway_final.csv`.

## Extensibility

- The model can be adapted for real-time streaming data by integrating Pathway’s connectors.
- The price formula can be extended to include additional factors (e.g., queue length, special days).

