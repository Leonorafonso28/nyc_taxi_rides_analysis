# NYC Taxi Rides — Geographic and Temporal Pattern Analysis with Apache Spark

Large-scale exploratory analysis of New York City taxi ride data using PySpark, investigating whether rides exhibit spatial clustering and temporal periodicity.

Developed as part of the MSc in Big Data Analysis and Engineering (NOVA University of Lisbon), using a 1% sample (~1M+ rides) of a real-world taxi dataset.

---

## Question

> *Do taxi rides exhibit geographic and/or temporal patterns?*

Short answer: yes, strongly — both spatially and temporally.

---

## Dataset

- ~1M ride records (1% sample of NYC taxi dataset)
- Features: pickup/dropoff datetime, GPS coordinates (latitude/longitude)
- Processed entirely in PySpark on a local Spark cluster

---

## Geographic Analysis

GPS coordinates were converted to a 300×300 grid using fixed bounds over the NYC area, enabling cell-level aggregation without coordinate noise.

**Key findings:**
- Pickup and dropoff hotspots are almost perfectly co-located, concentrated in a dense cluster around grid cell (161, 154) — consistent with Midtown Manhattan
- The top 5% of cells (95th percentile threshold) account for a disproportionate share of all ride activity
- The most frequent route connects cell (161, 154) → (161, 156), with ~1,600 occurrences in the sample — a short-distance intra-Manhattan movement

Visualisations: pickup heatmap, dropoff heatmap, origin–destination route heatmap (top 1,000 routes).

---

## Temporal Analysis

Temporal features extracted: hour of day, day of week, month, year.

**Key findings:**
- Peak demand: 18h–20h, with up to 108,900 pickups and 111,149 dropoffs at 19h
- Minimum demand: 5h, with ~17,500 pickups and ~16,700 dropoffs
- Moderate, relatively stable demand between 8h and 17h
- Pickup and dropoff temporal distributions are near-identical, suggesting rides are short and same-session

---

## Technical Highlights

- End-to-end PySpark pipeline: ingestion → cleaning → feature engineering → aggregation → visualisation
- Custom UDFs for GPS-to-grid conversion with bounds validation
- SparkSQL used alongside the DataFrame API for temporal aggregations
- `approxQuantile` for efficient percentile-based hotspot thresholding on distributed data
- Results collected to Pandas only at the final visualisation stage to minimise driver memory pressure

---

## Stack

Python · PySpark · SparkSQL · Pandas · Matplotlib · Seaborn · Java 17 (Spark runtime)