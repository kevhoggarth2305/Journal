## Purpose

To investigate whether the environmental conditions recorded by the Weather Station exhibit a diurnal (daily) cycle and to identify typical hourly patterns in temperature, humidity, and atmospheric pressure.

## Hypothesis

Temperature will follow a clear daily cycle, with lower values during the early morning and higher values during the late afternoon or evening. Humidity is expected to vary inversely with temperature, while atmospheric pressure is expected to be influenced more by regional weather systems than by time of day.

## Equipment Used

- Raspberry Pi environmental monitoring station
- BME280 environmental sensor
- SQLite database (`readings` table)
- Python 3
- Pandas
- Matplotlib
- Custom analysis script: `diurnal_analysis.py`

## Setup

The Weather Station remained in continuous operation, recording measurements every 5 minutes. At the time of analysis, the database contained 1,404 environmental observations comprising:

- Temperature (°C)
- Relative Humidity (%)
- Atmospheric Pressure (hPa)
- Timestamp

Data were grouped by hour of day to calculate hourly means, standard deviations, and observation counts. Visualisations were generated to display average hourly behaviour and variability.

## Procedure

1. Retrieved all readings from the SQLite database.
2. Converted timestamps into hourly values (00:00–23:00).
3. Grouped readings by hour of day.
4. Calculated:
   - Mean temperature
   - Mean humidity
   - Mean pressure
   - Standard deviation for each variable
   - Observation count per hour
5. Identified:
   - Warmest and coolest hours
   - Highest and lowest humidity hours
   - Highest and lowest pressure hours
6. Generated graphical plots showing hourly averages with variability bands.
7. Saved results to:
   - `hourly_diurnal_summary.csv`
   - `diurnal_patterns.png`

## Observations

- The temperature curve displayed a smooth and consistent daily pattern.
- Temperatures gradually decreased overnight, reaching a minimum around dawn.
- Temperatures increased steadily through the day before plateauing during the evening.
- Humidity remained remarkably stable throughout the monitoring period.
- Humidity showed only a slight tendency to decrease during the warmest parts of the day.
- Atmospheric pressure displayed much larger variability than temperature or humidity.
- Pressure behaviour appeared to reflect ongoing regional weather changes rather than a repeatable daily cycle.
- Observation counts were generally consistent at approximately 60 measurements per hour, indicating a well-balanced dataset.

## Results

### Temperature

- Warmest Hour: **18:00**
- Average Temperature: **21.30°C**
- Coolest Hour: **06:00**
- Average Temperature: **18.69°C**
- Daily Temperature Range: **2.61°C**

### Relative Humidity

- Highest Hour: **09:00**
- Average Humidity: **53.05%**
- Lowest Hour: **12:00**
- Average Humidity: **51.24%**
- Daily Humidity Range: **1.81%**

### Atmospheric Pressure

- Highest Hour: **11:00**
- Average Pressure: **1029.47 hPa**
- Lowest Hour: **17:00**
- Average Pressure: **1024.78 hPa**
- Apparent Range: **4.69 hPa**

### Dataset

- Total observations analysed: **1,404**
- Sampling interval: **5 minutes**
- Approximately **4.9 days** of continuous monitoring

## Conclusions

The diurnal analysis successfully identified a clear daily temperature cycle within the monitored indoor environment. Temperatures were lowest around 06:00 and highest around 18:00, producing a modest daily range of 2.61°C.

Relative humidity exhibited only minor variation throughout the day, indicating a stable indoor moisture environment. Although small fluctuations were observed, humidity remained largely consistent and did not demonstrate a strong daily cycle.

Atmospheric pressure showed substantially greater variation than temperature or humidity. However, the large standard deviations and the ongoing regional high-pressure weather system suggest that the observed pressure changes were primarily driven by external meteorological conditions rather than a true diurnal pattern.

Overall, temperature was found to be the dominant daily environmental cycle within the monitored space, while humidity was stable and pressure was largely influenced by broader weather trends.

## Next Actions

1. Develop `daily_summary.py` to compare environmental conditions between individual days.
2. Quantify daily changes in atmospheric pressure and compare them with local weather forecasts.
3. Continue data collection to increase the length of the monitoring period.
4. Re-run diurnal analysis after several weeks of data have been accumulated to improve statistical confidence.
5. Investigate derived measurements such as:
   - Dew Point
   - Absolute Humidity
   - Temperature change rate
6. Explore whether outdoor weather conditions can be correlated with indoor environmental measurements.
7. Maintain the experiment log and record any significant heating or occupancy events that may influence future analyses.