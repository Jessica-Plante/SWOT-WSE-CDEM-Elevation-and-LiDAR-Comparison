
# SWOT vs DEM Validation Analysis

This script performs a visual and statistical comparison between Water Surface Elevation (WSE) data obtained from the SWOT mission and elevation values from the Canadian Digital Elevation Model (CDEM). It also includes a comparison with LiDAR-derived elevations when available.

These comparisons support the analysis of river elevation changes in the context of a landslide-induced blockage between July and September 2024.

---

## Data Inputs

The script expects the following input files:

### 1. `DEM_WSE_validation.xlsx`

An Excel file containing SWOT WSE node elevations and corresponding CDEM values across several acquisition dates.

**Required columns:**

* `DEM_mean`: Mean elevation from the CDEM (in meters)
* `WSE_nodes`: Water Surface Elevation from SWOT (in meters)
* `Date`: Acquisition date (must be parseable by `pandas.to_datetime()`)

Rows with missing values in `DEM_mean`, `WSE_nodes`, or invalid `Date` entries are automatically dropped.

### 2. `stats_lidar_swot.xlsx`

An Excel file providing SWOT WSE node values and co-located LiDAR elevations for a single acquisition date (e.g., May 20–21, 2024).

**Required columns:**

* `WSE_SWOT`: SWOT Water Surface Elevation
* `z_LiDAR`: Elevation from airborne LiDAR data

---

## What the Script Does

### For each unique date in `DEM_WSE_validation.xlsx`, the script:

* Plots a **scatter plot** of `DEM_mean` (x-axis) vs. `WSE_nodes` (y-axis)
* Fits and overlays a **linear regression line**
* Calculates and displays the **Pearson correlation coefficient (r)**
* Draws a **1:1 reference line** in dashed gray
* Computes and displays a **95% confidence interval** around the regression line (if > 2 points)
* **Inverts the X-axis** to represent downstream river flow

> Note: Data from **August 2, 2024** is not plotted in a separate panel due to limited data. Instead, it is overlaid on the **August 13** plot.

### Additionally, it calculates summary statistics per date:

* **RMSE** (Root Mean Square Error)
* **MAE** (Mean Absolute Error)
* **Bias** (DEM – WSE or LiDAR – SWOT)
* **Pearson’s r**

### If `stats_lidar_swot.xlsx` is present:

* Similar statistics are calculated to compare **SWOT WSE vs. LiDAR elevations**.

---

## Dependencies

This script requires the following Python libraries:

```bash
pip install pandas numpy matplotlib scipy scikit-learn
```

### Key imports used:

```python
import pandas as pd
import numpy as np
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error, mean_absolute_error
from scipy.stats import t
import matplotlib.pyplot as plt
from matplotlib.ticker import AutoMinorLocator
```

---

## Example Output

For each acquisition date, printed statistics include:

```
Statistics for 2024-08-13:
  RMSE: 0.321
  MAE: 0.278
  Bias (DEM_mean - WSE_nodes): -0.065
  Pearson Correlation Coefficient (r): 0.89
--------------------
```

And, if LiDAR data is available:

```
Statistics for 2024-05-20/21 (SWOT vs LiDAR):
  RMSE: 0.244
  MAE: 0.205
  Bias (LiDAR - SWOT): -0.037
  Pearson Correlation Coefficient (r): 0.92
--------------------
```

---

## Notes

* Ensure your Excel files are correctly formatted and include all required columns.
* This script is intended for quick validation and visualization of elevation alignment between satellite (SWOT), topographic (CDEM), and airborne (LiDAR) datasets in riverine environments.
