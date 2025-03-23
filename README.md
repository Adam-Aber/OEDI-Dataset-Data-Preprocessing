# OEDI-Dataset-Data-Preprocessing


## 1. Chosen Dataset Description

For the task of detecting electricity theft in the smart grid, I evaluated three datasets: the Open Energy Data Initiative (OEDI) dataset, the Smart Home Energy Usage (SHEU) dataset, and the Smart Home with Weather Information (SHWI) dataset. The OEDI dataset was selected as it best aligns with the needs of our electricity theft detection project, particularly for binary classification. It contains **560,655 samples** across **13 columns** (12 features and 1 label). The theft label distinguishes between "Normal" consumption and six theft types ("Theft1" through "Theft6"), which can be simplified into a binary label (0 for "Normal," 1 for any theft). This suits our goal of detecting theft without specifying the type.

The SHEU and SHWI datasets lack theft-specific labels, making them less suitable despite their relevance to energy optimization studies. The OEDI dataset has a mild class imbalance: **331,824 "Normal" instances (59.2%)** and **228,831 theft instances (40.8%)**, which can be mitigated through oversampling or class weighting. This balance is better than typical real-world scenarios where theft is rarer. The "Class" attribute, with 16 client types (each with 35,040 samples), provides diverse consumption patterns, enhancing model applicability across user types—unlike SHEU (focused on smart homes) and SHWI (no client categorization).

The OEDI dataset includes **10 numerical features**, such as `Electricity:Facility [kW](Hourly)` and `Gas:Facility [kW](Hourly)`, offering a robust set of predictors for theft detection by analyzing electricity consumption behavior.

---

## 2. Feature Descriptions

Below are descriptions of key features in the OEDI dataset, including their raw distributions and relevance to theft detection:

### 2.1 Electricity:Facility [kW](Hourly)
This attribute represents total electricity consumption over the whole facility and is an important marker for detecting electricity theft because it catches in one sweep usage patterns that are easy to manipulate, for example, large decreases or zeroing out as in Theft Types 1 and 2. Its raw statistical form, as presented in the histogram, is extremely right-skewed with a sharp peak at 0 kW with a frequency of approximately 200,000, a sharp decrease to approximately 10,000 at 250 kW, and an extended tail to approximately 1,750 kW with very low frequencies, suggesting large number of zero or low usage periods punctuated by isolated high consumption events. There are no missing values in this column.

![Electricity:Facility [kW](Hourly)](distributions/figure1.png)

### 2.2 Fans:Electricity [kW](Hourly)
- **Significance:** Low to moderate; can indicate theft (e.g., under-reporting in Theft Type 1), though influenced by seasonal variation.
- **Distribution:** Highly right-skewed, peaks at 0 kW (frequency ~400,000), drops to ~5,000 at 200 kW, with a tail to 800 kW. Suggests cooling is often unused, possibly due to climate.
- **Missing Values:** None.

![Fans:Electricity [kW](Hourly)](distributions/figure2.png)

### 2.2 Cooling:Electricity [kW](Hourly)

This feature tracks the electricity usage of cooling devices, such as air conditioning,and can have low to moderate significance for theft detection if manipulated, for example, under-reporting in Theft Type 1, though its value could be moderated by seasonal variation. Its raw histogram is highly right-skewed with a high peak at 0 kW reaching a frequency of around 400,000, declining sharply to about 5,000 at 200 kW, and having an extremely low tail until 800 kW, indicating that cooling is usually not in use, perhaps due to climatic factors. There are no missing values in this column.

- **Significance:** Limited; less relevant unless manipulated significantly.
- **Distribution:** Right-skewed, peaks at 0 kW (frequency ~250,000), steep drop-off after 50 kW. Indicates minimal heating use, possibly due to gas reliance or weather.
- **Missing Values:** None.

![Cooling:Electricity [kW](Hourly)](distributions/figure3.png)

### 2.3 Heating:Electricity [kW](Hourly)

The "Heating:Electricity [kW](Hourly)" characteristic records the use of electricity by heating systems, with little applicability to theft detection unless electric heating is a significant and control lable factor, which seems less likely given the nature of the dataset. Its raw histogram is very right-skewed with a maximum at 0 kW of frequency 250,000 with a steep drop-off with very small values after 50 kW, suggesting that heating is not typically in operation, possibly due to reliance on gas or weather conditions. There are no missing values in this column.

- **Significance:** Low; relevant only if electric heating is a major factor (unlikely in this dataset).
- **Distribution:** Very right-skewed, peaks at 0 kW (frequency ~250,000), small values beyond 50 kW.
- **Missing Values:** None.

![Heating:Electricity [kW](Hourly)](distributions/figure4.png)

### 2.4 InteriorLights:Electricity [kW](Hourly)

"InteriorLights:Electricity [kW](Hourly)" is the consumption of electricity for interior light, a constant consumer with high salience for theft detection since it can be tampered with, for instance, being  switched off in Theft Type 2. Its raw distribution is right-skewed, peaking at 0 kW with a frequency of approximately 25,000, to approximately 1,000 at 100 kW, and having a sparse tail up to 400 kW, meaning frequent no or low lighting with sporadic greater usage. There are no missing values in this column.

- **Significance:** High; constant consumer, manipulable (e.g., switched off in Theft Type 2).
- **Distribution:** Right-skewed, peaks at 0 kW (frequency ~25,000), drops to ~1,000 at 100 kW, sparse tail to 400 kW. Indicates frequent low/no usage with sporadic peaks.
- **Missing Values:** None.

![InteriorLights:Electricity [kW](Hourly)](distributions/figure5.png)

### 2.5 InteriorEquipment:Electricity [kW](Hourly)

The "InteriorEquipment:Electricity[kW](Hourly)" attribute records electricity usage by interior equipment, i.e., appliances or machinery, and is highly relevant to theft detection due to its significance as a manipulable consumption type, e.g., under-reporting in Theft Type 1. Its raw distribution is right-skewed with a peak at 0 kW at approximately 200,000 frequency, decreasing to approximately 1,000 at 100 kW, and with a sparse tail to 400 kW, indicative of frequent zero or low equipment usage with occasional peaks. There are no missing values in this column.

- **Significance:** High; manipulable (e.g., under-reporting in Theft Type 1), key for theft detection.
- **Distribution:** Right-skewed, peaks at 0 kW (frequency ~200,000), drops to ~1,000 at 100 kW, sparse tail to 400 kW.
- **Missing Values:** None.

![InteriorEquipment:Electricity [kW](Hourly)](distributions/figure6.png)

### 2.6 Gas:Facility [kW](Hourly)

The "Gas:Facility [kW](Hourly)" feature is total gas usage across the facility, of moderate significance in identifying theft if gas use is tampered with, for instance, reduced use in Theft Type 1. Its raw distribution is right-skewed, peaking at 0 kW with a frequency of some 500,000, dropping to about 10,000 at 1,000 kW, and with a minor tail to 4,000 kW, showing plenty of no-gas intervals and occasional heavy use. There are no missing values in this column.

- **Significance:** Moderate; relevant if gas use is tampered with (e.g., reduced in Theft Type 1).
- **Distribution:** Right-skewed, peaks at 0 kW (frequency ~500,000), drops to ~10,000 at 1,000 kW, tail to 4,000 kW. Shows frequent no-gas intervals.
- **Missing Values:** None.

![Gas:Facility [kW](Hourly)](distributions/figure7.png)

### 2.7 Heating:Gas [kW](Hourly)

The "Heating:Gas [kW](Hourly)" feature measures consumption of heating equipment by gas, and low relevance to crime except in case gas heating accounts for a substantial and controllable portion. Its untrimmed histogram is right-skewed, largest at 0 kW with a frequency (120,000) then precipitous decline with extremely minuscule values beyond 1,000 kW, which is indicative of most heating being quiescent nearly all the time. There are no missing values in this column.

- **Significance:** Low; relevant only if gas heating is significant and controllable.
- **Distribution:** Right-skewed, peaks at 0 kW (frequency ~120,000), sharp decline beyond 1,000 kW. Mostly inactive.
- **Missing Values:** None.

![Heating:Gas [kW](Hourly)](distributions/figure8.png)

### 2.8 InteriorEquipment:Gas [kW](Hourly)

The "InteriorEquipment:Gas [kW](Hourly)" feature derives gas usage by interior equipment and is of moderate relevance if manipulated in theft operations. Its raw distribution is right-skewed, peaking at 0 kW at approximately 300,000, falling to approximately 1,000 at 20 kW, and with a thin tail up to 80 kW, and this suggests sporadic inactivity with irregular usage. There are no missing values in this column.

- **Significance:** Moderate; relevant if manipulated in theft.
- **Distribution:** Right-skewed, peaks at 0 kW (frequency ~300,000), drops to ~1,000 at 20 kW, tail to 80 kW. Sporadic usage.
- **Missing Values:** None.

![InteriorEquipment:Gas [kW](Hourly)](distributions/figure9.png)

### 2.9 Water Heater:WaterSystems:Gas [kW](Hourly)

The "Water Heater:WaterSystems:Gas [kW](Hourly)" attribute is a gauge of gas usage for water heating systems, being moderately important if manipulated in theft. Its raw distribution is right-skewed, with a peak at 0 kW and a frequency of approximately 500,000, declining to approximately 10,000 at 100 kW, and with a sparse tail to 800 kW, indicating frequent inactivity with occasional use. There are no missing values in this column.

- **Significance:** Moderate; relevant if manipulated.
- **Distribution:** Right-skewed, peaks at 0 kW (frequency ~500,000), drops to ~10,000 at 100 kW, tail to 800 kW. Frequent inactivity.
- **Missing Values:** None.

![Water Heater:WaterSystems:Gas [kW](Hourly)](distributions/figure10.png)

### 2.10 Class

The "Class" attribute specifies the class of client (like ’FullServiceRestaurant’), of high significance for capturing consumption variation affecting theft behavior among different types of users, such as warehouses versus hospitals. It has 17 types—16 types of clients (LargeOffice, 35,040 samples per type, to mention one), attribute demonstrating that there is well-balanced data with an infinitesimal anomaly. There are no missing values in this column.

- **Significance:** High; captures consumption variation across 16 client types (e.g., "FullServiceRestaurant," "LargeOffice"), each with 35,040 samples.
- **Distribution:** Balanced, no anomalies.
- **Missing Values:** None.

### 2.11 Theft

The "theft" feature is the original feature, classifying patterns as either "Normal" or one of six theft types, and is the target of th binary classification problem of detecting electricity theft. It has 7 unique values: Normal (331,824 samples, 59.2%), Theft1 (51,083, 9.1%), Theft3 (44,349, 7.9%), Theft4 (41,460, 7.4%), Theft6 (35,413, 6.3%), Theft5 (33,553, 6.0%), and Theft2 (22,973, 4.1%), with an imbalance of 59.2% "Normal" samples. There are no missing values, and since it is the target variable, it is uncorrelated with itself, but its synthetically generated theft patterns increase detection study, so it is quite relevant even though imbalance has to be dealt with

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

Below are the distributions of the features after log-scaling and standardization. These reflect the normalized data used for modeling.

### 4.1 Electricity:Facility [kW](Hourly)_log_scaled
- **Distribution:** Sharp spike near 0 (frequency > 20,000), symmetric drops with peaks at 1 and 2. Multi-modal KDE plot.
- **Correlation:** 0.308819 (highest).

![Electricity:Facility [kW](Hourly)_log_scaled](images/normalized_dist_Electricity_Facility_[kW](Hourly).png)

### 4.2 Fans:Electricity [kW](Hourly)_log_scaled
- **Distribution:** Steep peak at -0.5 (frequency ~12,000), gradual decline.
- **Correlation:** 0.173962.

![Fans:Electricity [kW](Hourly)_log_scaled](images/normalized_dist_Fans_Electricity_[kW](Hourly).png)

### 4.3 InteriorLights:Electricity [kW](Hourly)_log_scaled
- **Distribution:** Peaks at 0 (frequency ~35,000), multi-modal.
- **Correlation:** 0.250624.

![InteriorLights:Electricity [kW](Hourly)_log_scaled](images/normalized_dist_InteriorLights_Electricity_[kW](Hourly).png)

### 4.4 InteriorEquipment:Electricity [kW](Hourly)_log_scaled
- **Distribution:** Spike near 0 (frequency ~30,000), broad distribution.
- **Correlation:** 0.284878 (second-highest).

![InteriorEquipment:Electricity [kW](Hourly)_log_scaled](images/normalized_dist_InteriorEquipment_Electricity_[kW](Hourly).png)

### 4.5 Gas:Facility [kW](Hourly)_log_scaled
- **Distribution:** Sharp spike at -0.5 (frequency ~100,000), gradual drop.
- **Correlation:** 0.164465.

![Gas:Facility [kW](Hourly)_log_scaled](images/normalized_dist_Gas_Facility_[kW](Hourly).png)

### 4.6 InteriorEquipment:Gas [kW](Hourly)_log_scaled
- **Distribution:** Peaks at 0 (frequency ~25,000).
- **Correlation:** 0.122722.

![InteriorEquipment:Gas [kW](Hourly)_log_scaled](images/normalized_dist_InteriorEquipment_Gas_[kW](Hourly).png)

### 4.7 Water Heater:WaterSystems:Gas [kW](Hourly)_log_scaled
- **Distribution:** Peaks at -1 (frequency ~200,000), steep drop.
- **Correlation:** 0.128854.

![Water Heater:WaterSystems:Gas [kW](Hourly)_log_scaled](images/normalized_dist_InteriorLights_Electricity_[kW](Hourly).png)

### 4.8 Cooling:Electricity [kW](Hourly)_log_scaled (Removed)
- **Distribution:** Peaks near 0 (frequency ~250,000), with a long tail.
- **Correlation:** 0.083642 (below threshold, removed).

![Cooling:Electricity [kW](Hourly)_log_scaled](images/normalized_dist_Cooling_Electricity_[kW](Hourly).png)

### 4.9 Heating:Electricity [kW](Hourly)_log_scaled (Removed)
- **Distribution:** Peaks near 0 (frequency ~500,000), with a very sparse tail.
- **Correlation:** 0.038806 (below threshold, removed).

![Heating:Electricity [kW](Hourly)_log_scaled](images/normalized_dist_Heating_Electricity_[kW](Hourly).png)

### 4.10 Heating:Gas [kW](Hourly)_log_scaled (Removed)
- **Distribution:** Peaks near 0 (frequency ~250,000), with a long tail.
- **Correlation:** 0.084933 (below threshold, removed).

![Heating:Gas [kW](Hourly)_log_scaled](images/normalized_dist_Heating_Gas_[kW](Hourly).png)

---

## 5. Dataset

![Cleaned Dataset:](compressed_data.csv.gz)  
