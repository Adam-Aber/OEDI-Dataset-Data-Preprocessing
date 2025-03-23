# OEDI-Dataset-Data-Preprocessing


## 1. Chosen Dataset Description

In Phase 1, we evaluated three datasets: the Open Energy Data Initiative (OEDI) dataset, the Smart Home Energy Usage (SHEU) dataset, and the Smart Home with Weather Information (SHWI) dataset. The OEDI dataset was selected as it best aligns with the needs of our electricity theft detection project, particularly for binary classification. It contains **560,655 samples** across **13 columns** (12 features and 1 label). The theft label distinguishes between "Normal" consumption and six theft types ("Theft1" through "Theft6"), which can be simplified into a binary label (0 for "Normal," 1 for any theft). This suits our goal of detecting theft without specifying the type.

The SHEU and SHWI datasets lack theft-specific labels, making them less suitable despite their relevance to energy optimization studies. The OEDI dataset has a mild class imbalance: **331,824 "Normal" instances (59.2%)** and **228,831 theft instances (40.8%)**, which can be mitigated through oversampling or class weighting. This balance is better than typical real-world scenarios where theft is rarer. The "Class" attribute, with 16 client types (each with 35,040 samples), provides diverse consumption patterns, enhancing model applicability across user types—unlike SHEU (focused on smart homes) and SHWI (no client categorization).

The OEDI dataset includes **10 numerical features**, such as `Electricity:Facility [kW](Hourly)` and `Gas:Facility [kW](Hourly)`, offering a robust set of predictors for theft detection by analyzing electricity consumption behavior.

---

## 2. Feature Descriptions

Below are descriptions of key features in the OEDI dataset, including their distributions and relevance to theft detection:

### 2.1 Fans:Electricity [kW](Hourly)
- **Significance:** Low to moderate; can indicate theft (e.g., under-reporting in Theft Type 1), though influenced by seasonal variation.
- **Distribution:** Highly right-skewed, peaks at 0 kW (frequency ~400,000), drops to ~5,000 at 200 kW, with a tail to 800 kW. Suggests cooling is often unused, possibly due to climate.
- **Missing Values:** None.

![Fans:Electricity [kW](Hourly)](images/figure2.png)

### 2.2 Cooling:Electricity [kW](Hourly)
- **Significance:** Limited; less relevant unless manipulated significantly.
- **Distribution:** Right-skewed, peaks at 0 kW (frequency ~250,000), steep drop-off after 50 kW. Indicates minimal heating use, possibly due to gas reliance or weather.
- **Missing Values:** None.

![Cooling:Electricity [kW](Hourly)](images/figure3.png)

### 2.3 Heating:Electricity [kW](Hourly)
- **Significance:** Low; relevant only if electric heating is a major factor (unlikely in this dataset).
- **Distribution:** Very right-skewed, peaks at 0 kW (frequency ~250,000), small values beyond 50 kW.
- **Missing Values:** None.

![Heating:Electricity [kW](Hourly)](images/figure4.png)

### 2.4 InteriorLights:Electricity [kW](Hourly)
- **Significance:** High; constant consumer, manipulable (e.g., switched off in Theft Type 2).
- **Distribution:** Right-skewed, peaks at 0 kW (frequency ~25,000), drops to ~1,000 at 100 kW, sparse tail to 400 kW. Indicates frequent low/no usage with sporadic peaks.
- **Missing Values:** None.

![InteriorLights:Electricity [kW](Hourly)](images/figure5.png)

### 2.5 InteriorEquipment:Electricity [kW](Hourly)
- **Significance:** High; manipulable (e.g., under-reporting in Theft Type 1), key for theft detection.
- **Distribution:** Right-skewed, peaks at 0 kW (frequency ~200,000), drops to ~1,000 at 100 kW, sparse tail to 400 kW.
- **Missing Values:** None.

![InteriorEquipment:Electricity [kW](Hourly)](images/figure6.png)

### 2.6 Gas:Facility [kW](Hourly)
- **Significance:** Moderate; relevant if gas use is tampered with (e.g., reduced in Theft Type 1).
- **Distribution:** Right-skewed, peaks at 0 kW (frequency ~500,000), drops to ~10,000 at 1,000 kW, tail to 4,000 kW. Shows frequent no-gas intervals.
- **Missing Values:** None.

![Gas:Facility [kW](Hourly)](images/figure7.png)

### 2.7 Heating:Gas [kW](Hourly)
- **Significance:** Low; relevant only if gas heating is significant and controllable.
- **Distribution:** Right-skewed, peaks at 0 kW (frequency ~120,000), sharp decline beyond 1,000 kW. Mostly inactive.
- **Missing Values:** None.

![Heating:Gas [kW](Hourly)](images/figure8.png)

### 2.8 InteriorEquipment:Gas [kW](Hourly)
- **Significance:** Moderate; relevant if manipulated in theft.
- **Distribution:** Right-skewed, peaks at 0 kW (frequency ~300,000), drops to ~1,000 at 20 kW, tail to 80 kW. Sporadic usage.
- **Missing Values:** None.

![InteriorEquipment:Gas [kW](Hourly)](images/figure9.png)

### 2.9 Water Heater:WaterSystems:Gas [kW](Hourly)
- **Significance:** Moderate; relevant if manipulated.
- **Distribution:** Right-skewed, peaks at 0 kW (frequency ~500,000), drops to ~10,000 at 100 kW, tail to 800 kW. Frequent inactivity.
- **Missing Values:** None.

![Water Heater:WaterSystems:Gas [kW](Hourly)](images/figure10.png)

### 2.10 Class
- **Significance:** High; captures consumption variation across 16 client types (e.g., "FullServiceRestaurant," "LargeOffice"), each with 35,040 samples.
- **Distribution:** Balanced, no anomalies.
- **Missing Values:** None.

### 2.11 Theft
- **Significance:** Target variable; classifies patterns as "Normal" (331,824 samples, 59.2%) or one of six theft types (Theft1: 9.1%, Theft2: 4.1%, Theft3: 7.9%, Theft4: 7.4%, Theft5: 6.0%, Theft6: 6.3%).
- **Distribution:** Imbalanced (59.2% Normal), addressed via binary conversion.
- **Missing Values:** None.

---

## 3. Cleaning and Pre-processing

### 3.1 Log Scaling and Standardization
Due to the right-skewed nature of energy consumption data (e.g., `Electricity:Facility` max at 1,726.43 kW), we applied **log scaling** (`log(x+1)`) to handle extreme values and zeros, followed by **standardization** (mean = 0, std = 1). This approach:
- Reduces skewness better than min-max scaling.
- Highlights subtle theft patterns (e.g., 2 kW to 1 kW drops).
- Aligns features with models assuming normality (e.g., logistic regression).

Min-max scaling was avoided as it preserves skewness and compresses minor variations.

### 3.2 Normalization and Binary Conversion
After scaling, we normalized the dataset, retaining `Class` and `Theft` columns. The `Theft` column was converted to a binary label (`theft_binary`):
- 0 = "Normal" (331,824 samples, 59.2%).
- 1 = Any theft (228,831 samples, 40.8%).

This focuses on theft detection rather than type classification, aligning with practical goals.

### 3.3 Feature Selection
We used **Pearson correlation** between log-scaled features and `theft_binary` to select features, setting a threshold of **0.1**:
- **Excluded Features (< 0.1):**
  - `Electricity:Cooling [kW](Hourly)_log_scaled` (0.083642)
  - `Electricity:Heating [kW](Hourly)_log_scaled` (0.038806)
  - `Gas:Heating [kW](Hourly)_log_scaled` (0.084933)
- **Retained Features (> 0.1):**
  - `Electricity:Facility [kW](Hourly)_log_scaled` (0.308819)
  - `InteriorEquipment:Electricity [kW](Hourly)_log_scaled` (0.284878)
  - `InteriorLights:Electricity [kW](Hourly)_log_scaled` (0.250624)
  - `Fans:Electricity [kW](Hourly)_log_scaled` (0.173962)
  - `Gas:Facility [kW](Hourly)_log_scaled` (0.164465)
  - `Water Heater:WaterSystems:Gas [kW](Hourly)_log_scaled` (0.128854)
  - `InteriorEquipment:Gas [kW](Hourly)_log_scaled` (0.122722)

| Feature                                      | Correlation with Theft |
|----------------------------------------------|------------------------|
| Electricity:Facility [kW](Hourly)_log_scaled | 0.308819              |
| Fans:Electricity [kW](Hourly)_log_scaled     | 0.173962              |
| Cooling:Electricity [kW](Hourly)_log_scaled  | 0.083642              |
| Heating:Electricity [kW](Hourly)_log_scaled  | 0.038806              |
| InteriorLights:Electricity [kW](Hourly)_log_scaled | 0.250624        |
| InteriorEquipment:Electricity [kW](Hourly)_log_scaled | 0.284878     |
| Gas:Facility [kW](Hourly)_log_scaled         | 0.164465              |
| Heating:Gas [kW](Hourly)_log_scaled          | 0.084933              |
| InteriorEquipment:Gas [kW](Hourly)_log_scaled | 0.122722             |
| Water Heater:WaterSystems:Gas [kW](Hourly)_log_scaled | 0.128854     |

![Correlation Table](images/table1.png)

### 3.4 Categorical Data Preprocessing and Handling Missing Values
- **One-Hot Encoding:** Applied to the `Class` column (16 client types), expanding the dataset to 25 columns (16 binary columns, each with 35,040 samples).
- **Missing Values:** None present in the dataset.

---

## 4. Processed Feature Distributions

### 4.1 Electricity:Facility [kW](Hourly)_log_scaled
- **Distribution:** Sharp spike near 0 (frequency > 20,000), symmetric drops with peaks at 1 and 2. Multi-modal KDE plot.
- **Correlation:** 0.308819 (highest).

![Electricity:Facility [kW](Hourly)_log_scaled](images/figure11.png)

### 4.2 Fans:Electricity [kW](Hourly)_log_scaled
- **Distribution:** Steep peak at -0.5 (frequency ~12,000), gradual decline.
- **Correlation:** 0.173962.

![Fans:Electricity [kW](Hourly)_log_scaled](images/figure12.png)

### 4.3 InteriorLights:Electricity [kW](Hourly)_log_scaled
- **Distribution:** Peaks at 0 (frequency ~35,000), multi-modal.
- **Correlation:** 0.250624.

![InteriorLights:Electricity [kW](Hourly)_log_scaled](images/figure13.png)

### 4.4 InteriorEquipment:Electricity [kW](Hourly)_log_scaled
- **Distribution:** Spike near 0 (frequency ~30,000), broad distribution.
- **Correlation:** 0.284878 (second-highest).

![InteriorEquipment:Electricity [kW](Hourly)_log_scaled](images/figure14.png)

### 4.5 Gas:Facility [kW](Hourly)_log_scaled
- **Distribution:** Sharp spike at -0.5 (frequency ~100,000), gradual drop.
- **Correlation:** 0.164465.

![Gas:Facility [kW](Hourly)_log_scaled](images/figure15.png)

### 4.6 InteriorEquipment:Gas [kW](Hourly)_log_scaled
- **Distribution:** Peaks at 0 (frequency ~25,000).
- **Correlation:** 0.122722.

![InteriorEquipment:Gas [kW](Hourly)_log_scaled](images/figure16.png)

### 4.7 Water Heater:WaterSystems:Gas [kW](Hourly)_log_scaled
- **Distribution:** Peaks at -1 (frequency ~200,000), steep drop.
- **Correlation:** 0.128854.

![Water Heater:WaterSystems:Gas [kW](Hourly)_log_scaled](images/figure17.png)

---

## 5. Dataset Dumps

![Dataset Dump](images/figure18.png)  
![Dump of Numeric Features](images/figure19.png)
