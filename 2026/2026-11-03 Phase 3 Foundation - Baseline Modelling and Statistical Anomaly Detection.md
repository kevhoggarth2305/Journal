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
* Event Type: HEATER
* Description: Portable heater
* Start Time: 2026-10-28 10:30
* End Time: 2026-10-28 13:30

## Next Actions

* Complete implementation of z-score calculation within the analysis view.
* Query and visualise readings with extreme z-scores.
* Overlay expected and actual temperature on existing plots.
* Add deviation and z-score visualisations.
* Investigate correlation between anomaly scores and logged events.
* Automate overnight baseline regeneration.
* Expand dataset beyond two weeks to improve baseline accuracy.
* Explore day-of-week baselines.
* Investigate humidity and pressure baseline models.

## Reflections

This session marked an important transition in the Weather Station project from simple environmental monitoring towards predictive environmental modelling. Previous work focused primarily on collecting, storing and visualising environmental observations. Today's activities shifted attention towards defining what constitutes normal behaviour within the monitored environment.

The development of a baseline temperature model represents a significant step forward. Rather than simply reporting the current temperature, the system can now determine whether a measurement is typical for a given time of day. This transforms the weather station from a passive data logger into a platform capable of supporting anomaly detection and future forecasting.

The introduction of standard deviation calculations proved particularly valuable. Analysis showed that environmental variability changes considerably throughout the day. Overnight conditions remain highly predictable with relatively small variation, while daytime conditions exhibit much larger fluctuations driven by occupancy, heating and natural environmental influences. This observation reinforces the value of using statistical measures such as z-scores rather than simple fixed thresholds when identifying anomalies.

The most rewarding outcome of the session was the successful validation of the baseline model. The largest temperature deviations identified by the analysis corresponded directly to a documented heater event recorded in the environmental events log. The model highlighted the period as unusual without any prior knowledge of the event itself, demonstrating that it is detecting genuine environmental changes rather than random measurement noise.

Several useful technical lessons were also learned. Investigation of timestamp drift revealed how small execution-time overheads accumulate over long periods when using fixed sleep intervals. While the drift is not operationally significant for this project, understanding its effects improved confidence in the interpretation of the data and influenced the decision to aggregate readings into fifteen-minute baseline buckets.

In addition, the growing complexity of database objects highlighted the advantages of maintaining SQL definitions within dedicated `.sql` files rather than entering long statements interactively. This approach improves repeatability, supports version control and aligns more closely with professional development practices.

Overall, the session represents one of the most significant milestones in the project so far. The weather station now possesses a statistically derived understanding of typical environmental behaviour, providing the foundation required for anomaly detection, event impact analysis, confidence intervals and future predictive modelling. The project continues its evolution from environmental monitoring towards genuine environmental data science.