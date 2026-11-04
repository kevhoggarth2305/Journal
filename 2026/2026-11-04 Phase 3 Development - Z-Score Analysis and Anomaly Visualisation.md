# 2026-11-04 Phase 3 Development - Z-Score Analysis and Anomaly Visualisation

## Session Details

| Date | Start | End | Location | Project | Experiment(s) | Equipment |
| --- | --- | --- | --- | --- | --- | --- |
| 04/11/2026 | 18:00 | 20:00 | Home Office | Weather Station | Statistical Anomaly Detection and Event Visualisation | Raspberry Pi 5<br>BME280 Temperature/Humidity/Pressure Sensor<br>SQLite Database<br>VS Code<br>Python Analysis Scripts<br>Pandas<br>Matplotlib<br>Git<br>Events Log (CSV) |

## Objective

Extend the statistical baseline model by implementing z-score analysis and develop a visualisation capable of identifying and explaining anomalous environmental behaviour through comparison with logged environmental events.

## Work Conducted

* Resumed development from the previously implemented baseline temperature model.
* Extended the `reading_analysis` view to include z-score calculations.
* Validated the calculation of z-scores against expected temperature deviations.
* Performed anomaly detection using a threshold of `|z_score| > 2`.
* Queried all statistically significant deviations from expected environmental conditions.
* Reviewed anomaly results against the environmental events log.
* Confirmed strong correlation between elevated z-scores and logged heater activity.
* Investigated unexpected negative anomalies for potential future analysis.
* Developed a new anomaly visualisation script in Python.
* Loaded analysis data directly from the `reading_analysis` view.
* Calculated upper and lower confidence bands using baseline standard deviation.
* Plotted:
  * Actual temperature
  * Expected temperature
  * Expected ±1 standard deviation confidence interval
  * Detected anomalies
* Loaded event data from `events.csv`.
* Parsed event start and end timestamps.
* Filtered environmental events to heater activity.
* Implemented shaded heater activity periods on the visualisation using `axvspan()`.
* Improved overall chart presentation with:
  * Larger figure dimensions
  * Title
  * Axis labels
  * Legend
  * Date formatting
  * Grid lines
* Generated the first integrated anomaly detection visualisation.

## Observations

* The anomaly detection process successfully identified periods of sustained temperature elevation.
* The largest positive anomalies corresponded directly with previously logged heater activity.
* Most detected anomalies occurred during a single heater event on 28 October.
* Additional elevated z-scores were observed during recent occupied evening periods.
* One notable negative anomaly was identified on 31 October and remains unexplained.
* Not every heater activation generated an anomaly.
* Some heater events produced temperatures that remained within the statistically expected range.
* The visualisation clearly demonstrates the relationship between environmental events and statistical anomalies.
* Confidence bands provide a useful visual indicator of expected environmental variation.

## Results

### Z-Score Threshold Analysis

Detection threshold:

```text
|z_score| > 2
```

Detected anomalies included:

* Extended heater-associated warming event on 28 October.
* Elevated temperatures during occupied evening periods on 03 November.
* A single statistically significant negative anomaly on 31 October.

### Event Validation

The strongest anomaly cluster aligned with the recorded heater event:

```text
2026-10-28 10:30 -> 13:30
HEATER
Portable heater
```

Temperatures during this period exceeded expected values by approximately 2-3°C and generated the highest z-scores within the current dataset.

### Visualisation Outcomes

The completed chart successfully displays:

* Actual temperature measurements.
* Baseline expected temperature.
* One standard deviation confidence intervals.
* Detected anomalies.
* Heater operating periods.

The visualisation provides immediate visual confirmation of several relationships that previously required manual SQL investigation.

## Issues Encountered

* Initial attempts to visualise heater activity used `fill_between()`, which is designed for y-axis ranges rather than time periods.
* Heater event visualisation therefore failed conceptually despite syntactically correct code.
* Resolved by replacing `fill_between()` with `axvspan()` to create vertical shaded event regions.
* Several plotting improvements were required to improve readability once multiple analytical layers were added.

## Decisions Made

| Decision | Reason | Impact |
| --- | --- | --- |
| Adopt z-score anomaly detection | Normalises anomaly detection for differing natural variability throughout the day | More robust than fixed temperature thresholds |
| Use \|z\| > 2 as initial anomaly threshold | Produces useful anomaly detection without excessive noise | Practical starting point for future refinement |
| Visualise heater activity on charts | Provides contextual explanation of anomalies | Improves interpretation of results |
| Retain all readings within baseline calculations | Maximises available data during early project stages | Produces representative "actual office behaviour" model |
| Continue using 15-minute baseline periods | Provides sufficient sample counts with current dataset size | Stable statistical foundation |
| Maintain event logging discipline | Enables validation of future anomaly detection results | Supports scientific interpretation |

## Resources Referenced

* SQLite views
* Existing Weather Station database
* Environmental event log
* Pandas
* Matplotlib
* Standard deviation and z-score calculations
* Previous baseline modelling outputs

## Next Actions

* Investigate the unexplained negative anomaly recorded on 31 October.
* Experiment with alternative anomaly thresholds.
* Add anomaly counts to automated reports.
* Create a dedicated z-score visualisation.
* Evaluate anomaly frequency by day and time period.
* Compare anomaly occurrence against all logged event categories.
* Investigate exclusion of heater data from future experimental baselines.
* Develop a "clean" baseline representing unmodified environmental conditions.
* Begin planning simple predictive temperature forecasting.

## Reflections

This session represents a significant milestone in the evolution of the Weather Station project. The baseline model developed during previous work has now progressed into a functioning anomaly detection system capable of identifying statistically unusual environmental conditions and relating them to known real-world events.

The introduction of z-scores proved particularly valuable because it allows environmental deviations to be interpreted within the context of natural variability. A one-degree temperature difference can be entirely routine at one time of day while being highly unusual at another. Normalising observations using standard deviation provides a far more meaningful assessment of environmental behaviour than simple fixed thresholds.

Perhaps the most satisfying outcome was the successful validation of the anomaly model against independently recorded heater events. The strongest anomalies identified by the analysis corresponded directly with documented periods of heater activity. Importantly, not every heater event generated an anomaly. This suggests that the model is not merely responding to event labels but is evaluating the magnitude of the resulting environmental impact.

An additional insight emerged from recognising that the current baseline represents typical office behaviour rather than idealised environmental behaviour. Because heater events form part of the historical dataset, their influence has already been incorporated into the expected temperatures and associated variance. This explains why some heater periods remain within the model's normal operating range. Such observations provide useful direction for future development of experimental "clean" baselines that exclude known environmental interventions.

The newly developed visualisation provides a particularly effective summary of system behaviour. Previously, correlations between events and anomalies required SQL queries and manual interpretation. The integrated chart now presents expected behaviour, actual observations, confidence intervals, anomalies and heater events simultaneously, allowing relationships to be understood almost immediately.

Overall, this session marks the transition from statistical modelling to statistical interpretation. The project is no longer simply collecting environmental data; it is beginning to explain why certain environmental conditions occur and identify which observations deserve further investigation. This represents another substantial step towards a genuinely scientific environmental monitoring platform.