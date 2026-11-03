# 2026-11-03 Phase 3 Foundation - Baseline Modelling and Statistical Anomaly Detection

## Session Details

| Date | Start | End | Location | Project | Experiment(s) | Equipment |
| --- | --- | --- | --- | --- | --- | --- |
| 03/11/2026 | 18:00 | 19:45 | Home Office | Weather Station | Baseline Temperature Model and Anomaly Detection Framework | Raspberry Pi 5<br>BME280 Temperature/Humidity/Pressure Sensor<br>SQLite Database<br>VS Code<br>Python Analysis Scripts<br>Git<br>Events Log (CSV)<br>Copilot |

## Objective

Develop the first statistical baseline model for the environmental monitoring system, enabling future comparison of live observations against expected environmental conditions for a given time of day.

## Work Conducted

* Reviewed the accumulating environmental dataset.
* Confirmed dataset growth to approximately 2,042 observations.
* Investigated observation timestamps and sampling intervals.
* Verified the sensor collection process continues to operate at approximately five-minute intervals.
* Identified gradual timestamp drift caused by execution time accumulating between readings.
* Determined that drift had spread observations across all minute values over time.
* Explored approaches for constructing an expected temperature baseline.
* Evaluated one-minute, five-minute and fifteen-minute aggregation strategies.
* Determined that fifteen-minute buckets provided the best balance between resolution and statistical reliability given the current dataset size.
* Created a new `baseline_temperature` table within SQLite.
* Calculated expected temperatures for each fifteen-minute period across the day.
* Added observation counts to validate sample sizes.
* Extended the baseline model to calculate temperature standard deviation for each time slot.
* Investigated overnight, daytime and evening behaviour through baseline statistics.
* Confirmed the presence of a predictable daily thermal cycle.
* Created a SQL-based analysis view architecture.
* Developed the initial `reading_analysis` view design linking raw readings to baseline statistics.
* Investigated largest deviations from expected temperature.
* Queried readings with the greatest positive departures from baseline.
* Compared identified anomalies against logged environmental events.
* Validated the model against historical heater usage records.

## Observations

* Temperature follows a repeatable daily pattern.
* Overnight temperatures decline gradually and consistently.
* Typical overnight variability was approximately ±0.5°C.
* Daytime variability increased significantly compared to overnight conditions.
* Standard deviation around midday exceeded 1°C.
* Temperature variability appears strongly influenced by occupancy, heating and environmental conditions.
* The fifteen-minute baseline structure produced statistically meaningful sample counts.
* Most baseline periods contained approximately twenty-one observations.
* The baseline model successfully highlighted periods of unusual temperature behaviour.
* The most significant temperature deviations were concentrated within a single continuous period on 28 October.

## Results

### Baseline Characteristics

| Time | Expected Temperature | Standard Deviation |
| --- | --- | --- |
| 06:00 | 18.52°C | 0.586°C |
| 09:00 | 19.53°C | 1.111°C |
| 12:00 | 20.80°C | 1.221°C |
| 15:00 | 21.18°C | 0.902°C |
| 18:00 | 21.47°C | 0.867°C |
| 21:00 | 20.81°C | 0.465°C |

### Largest Detected Anomaly

* Timestamp: 2026-10-28 12:24
* Actual Temperature: 23.9°C
* Expected Temperature: 20.8°C
* Deviation: +3.1°C

### Event Correlation

Comparison against the environmental event log identified:

```text
2026-10-28 10:30 -> 13:30
HEATER
Portable heater