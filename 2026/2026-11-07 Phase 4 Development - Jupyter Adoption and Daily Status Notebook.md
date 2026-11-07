# 2026-11-07 Phase 4 Development - Jupyter Adoption and Daily Status Notebook

## Session Details

| Date | Start | End | Location | Project | Experiment(s) | Equipment |
| --- | --- | --- | --- | --- | --- | --- |
| 07/11/2026 | 02:15 | 04:00 | Home Office | Weather Station (Trillium) | Jupyter Notebook Development and Operational Monitoring | Raspberry Pi 5<br>BME280 Temperature/Humidity/Pressure Sensor<br>SQLite Database<br>VS Code<br>Jupyter Notebook<br>Python<br>Pandas<br>Matplotlib |

## Objective

Explore Jupyter Notebooks as a scientific and operational analysis environment while developing the first version of a reusable Trillium Daily Status notebook capable of reviewing system health, environmental conditions, deviations and anomalies.

## Work Conducted

* Continued Jupyter learning following the previous GravityLab sessions.
* Discussed the differing roles of:
  * Trillium Journal entries.
  * Jupyter analysis notebooks.
  * Future dashboard development.
* Established a project structure for notebook-based analysis.
* Created a new `notebooks` folder within the Weather Station project.
* Created the notebook:

  ```text
  Daily_Status.ipynb
  ```

* Implemented notebook sections for:
  * System Health
  * Database Statistics
  * Latest Readings
  * Current Conditions vs Expected
  * Temperature Visualisation
  * Baseline Comparison
  * Deviation Analysis
  * Z-Score Analysis
  * Daily Notes

* Connected the notebook directly to the SQLite database.
* Queried current system status directly from the `readings` table.
* Queried statistical information from the `reading_analysis` view.
* Generated tabular displays using Pandas DataFrames within notebook cells.
* Created a temperature chart covering the last 24 hours.
* Added baseline temperatures to the visualisation.
* Produced a comparison graph showing:
  * Actual temperature.
  * Expected baseline temperature.
* Implemented a deviation chart showing:

  ```text
  deviation = actual - expected
  ```

* Added a dedicated z-score chart.
* Added ±2σ and ±3σ threshold indicators.
* Added graph legends and improved axis formatting.
* Modified date formatting to display readable hourly labels.
* Introduced automatic formatting using:
  * `matplotlib.dates`
  * `DateFormatter`
  * `tight_layout()`

## Observations

* Jupyter proved significantly more powerful than initially expected.
* Combining Markdown, SQL, Python, visualisations and commentary within a single document creates a modern equivalent of a scientific laboratory notebook.
* The workflow feels substantially different from script-based development.
* The notebook format encourages investigation and exploration rather than simply executing code.
* Visualising deviations from expected behaviour immediately highlighted patterns that would have been difficult to identify from raw temperature data alone.
* A significant negative deviation was identified during lunchtime on 06 November.
* The event was clearly visible on the deviation chart despite not yet being investigated.
* The discovery illustrates the value of visual exploratory analysis.

## Results

### Daily Status Notebook

The initial notebook successfully provides:

* Database connectivity verification.
* Latest reading verification.
* Record count monitoring.
* Current environmental conditions.
* Expected versus actual temperature comparison.
* Temperature deviation visualisation.
* Z-score monitoring.
* Largest anomaly identification.
* Operational review capability.

### Reading Count Investigation

Initial daily reading count:

```text
323 readings
```

appeared inconsistent with the expected:

```text
288 readings
```

for a five-minute sampling interval.

Investigation revealed that the SQL query:

```sql
WHERE timestamp >= datetime('now', '-1 day')
```

was not behaving as expected with the stored timestamp format.

Further analysis showed:

```text
First reading: 2026-11-06 00:00:27
Last reading : 2026-11-07 03:01:14
```

indicating a substantially larger time window than intended.

The notebook was refactored to use Pandas datetime filtering:

```python
latest_time = df["timestamp"].max()

last_24h = df[
    df["timestamp"] >= latest_time - pd.Timedelta(hours=24)
]
```

Result:

```text
288 readings
```

This confirmed:

* Data capture is operating correctly.
* Five-minute scheduling remains accurate.
* No missing readings were identified.
* No duplicate readings were detected.

### Operational Insight

The notebook directly identified an unexpected discrepancy in reporting logic and provided a framework to diagnose and resolve the issue.

## Issues Encountered

* Intermittent code rendering corruption appeared within the Copilot chat interface.
* SQL keywords and Python identifiers occasionally appeared with missing or substituted characters.
* Several notebook cells required manual verification before execution.
* Initial confusion occurred regarding notebook execution order due to a code cell being edited but not run.
* VS Code notebook export functionality was found to differ from traditional Jupyter Lab reporting options.
* Hiding code inputs for report-style output requires further investigation.

## Decisions Made

| Decision | Reason | Impact |
| --- | --- | --- |
| Adopt Jupyter Notebooks for exploratory analysis | Combines code, results and interpretation in one place | Improved scientific workflow |
| Use notebooks for operational review and experimentation | Better suited to investigation than standalone scripts | Encourages analysis-driven development |
| Retain Trillium Journal for project history | Journal and notebooks serve different purposes | Clear separation of responsibilities |
| Move 24-hour calculations into Pandas | Avoids ambiguity in SQL datetime handling | More reliable reporting |
| Build all daily statistics from `last_24h` datasets | Ensures consistency across notebook outputs | Reduces future errors |
| Continue postponing dashboard development | Notebook development is revealing requirements naturally | Better-informed future design |

## Resources Referenced

* SQLite database
* Existing `reading_analysis` view
* Jupyter Notebook
* Pandas DataFrames
* Matplotlib
* DateFormatter
* Z-score analysis
* Baseline temperature model
* Previous Trillium analytical work

## Next Actions

* Investigate the lunchtime negative deviation observed on 06 November.
* Replace remaining SQL-based 24-hour filtering with Pandas equivalents.
* Complete anomaly summary sections within the Daily Status notebook.
* Add confidence interval visualisations to the notebook.
* Explore report-style notebook outputs suitable for operational reviews.
* Investigate code-hidden exports using Jupyter Lab.
* Create additional notebooks for:
  * Baseline Analysis
  * Anomaly Investigation
  * Forecasting Experiments
  * Heater Impact Analysis
* Continue evaluating whether notebook-based reporting can defer the need for a dedicated dashboard.

## Reflections

This session unexpectedly became one of the most significant development sessions completed so far within the Trillium project. Although the original intention was simply to explore the Jupyter user interface, the exercise quickly demonstrated that notebooks occupy a fundamentally different role from both conventional Python scripts and project journals.

The most valuable insight was recognising the distinction between operational history and scientific investigation. The Trillium Journal records decisions, progress and lessons learned, whereas notebooks capture questions, methods, analysis, evidence and conclusions. Together they provide a complete record of both project development and scientific reasoning.

The Daily Status notebook proved immediately useful. Rather than simply displaying information, it actively encouraged investigation. A suspicious reading count led to the discovery of incorrect date filtering behaviour, while visualisation of deviations revealed environmental events that may otherwise have gone unnoticed. The notebook therefore acted not merely as a reporting tool but as an analytical instrument in its own right.

Particularly encouraging was the transition from manually executing SQL queries towards creating reusable operational workflows. The notebook can now be executed to provide a complete environmental review within minutes, while remaining fully documented and reproducible.

The session also reinforced the value of visualisation. The overlay of measured and expected temperatures, followed by dedicated deviation and z-score graphs, transformed the project from a simple environmental logger into a platform capable of highlighting meaningful behavioural patterns. The unexpected lunchtime cooling event serves as an example of the insights now becoming visible.

Overall, this work represents another important evolution in Trillium's development. The project is progressing beyond data collection and anomaly detection into a structured analytical workflow capable of supporting routine operations, scientific investigation and future forecasting efforts. The Daily Status notebook has already demonstrated practical value and is likely to become a central component of future project activities.