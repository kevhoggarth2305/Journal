# 2026-11-14 Indoor Environmental Analysis - Temperature Trends, Heatmaps and Baseline Evaluation

## Session Details

| Date | Start | End | Location | Project | Experiment(s) | Equipment |
| --- | --- | --- | --- | --- | --- | --- |
| 14/11/2026 | 17:00 | Ongoing | Home Office | Weather Station | Indoor Temperature Trend Analysis and Baseline Evaluation | Raspberry Pi 5<br><br>BME280 Temperature/Humidity/Pressure Sensor<br>SQLite Database<br>Jupyter Notebook<br>Python Analysis Scripts<br>VS Code<br>Git<br>Copilot |

## Objective

Conduct the first in-depth exploratory analysis of the indoor environmental dataset to answer the original temperature-related research questions established for the Weather Station project.

The analysis aimed to:

1. Determine how daily minimum, mean and maximum temperatures evolve over time.
2. Identify evidence of colder periods potentially associated with frost events or colder external weather.
3. Investigate repeating daily temperature patterns.
4. Evaluate whether the anomaly detection baseline remains representative as environmental conditions change.

## Work Conducted

* Loaded and explored the accumulated indoor environmental dataset.
* Generated daily minimum, mean and maximum temperature statistics.
* Produced temperature trend visualisations.
* Added a linear trend line to daily mean temperatures.
* Calculated long-term rate of temperature change.
* Identified a cooling trend of approximately -0.091°C per day.
* Investigated the significance of colder overnight and morning temperatures.
* Generated average temperature profiles by hour of day.
* Identified clear occupancy-related and heating-related temperature patterns.
* Produced a temperature heatmap showing hourly temperatures across all recorded dates.
* Examined temperature variation between home-working days and office-working days.
* Observed distinct thermal signatures associated with occupancy behaviour.
* Reviewed anomaly detection outputs using z-score analysis.
* Visualised temperature z-scores over time.
* Calculated daily average z-scores.
* Investigated average z-score behaviour by hour of day.
* Evaluated potential baseline drift within the anomaly detection model.
* Developed hypotheses regarding seasonal cooling versus short-term cold weather effects.

## Observations

* Daily mean temperature exhibits a gradual downward trend over the observation period.
* Daily minimum and maximum temperatures generally follow the same decline.
* The office environment demonstrates a highly repeatable daily thermal cycle.
* Indoor temperatures are strongly influenced by occupancy and heating behaviour.
* Work-from-home days can be clearly distinguished in the temperature heatmap.
* Days spent in the Manchester office exhibit a noticeably different thermal signature.
* Evening temperature increases correspond closely with occupancy and portable heater usage.
* Overnight temperatures display the greatest sensitivity to external weather conditions.
* Morning temperatures increasingly deviate below the established baseline.
* Afternoon temperatures remain comparatively close to expected baseline values.
* The strongest negative z-scores occur between approximately 02:00 and 06:00.
* The anomaly system appears most sensitive during periods when the office is least influenced by occupancy and heating.

## Results

### Long-Term Temperature Trend

| Metric | Value |
| --- | --- |
| Mean Temperature Trend | -0.091°C/day |
| Approximate Weekly Change | -0.64°C/week |

### Daily Thermal Behaviour

Analysis of hourly averages revealed:

* Lowest temperatures occur during the early morning period.
* Temperatures recover throughout the morning as the house heating system influences the office environment.
* Afternoon temperatures remain comparatively stable.
* Evening temperatures are elevated by occupancy, computing equipment and occasional portable heater use.
* The office exhibits clear evidence of thermal inertia, with indoor conditions responding gradually to external influences.

### Heatmap Analysis

The temperature heatmap proved particularly informative.

Key observations include:

* Clear differentiation between home-working and office-working days.
* Persistent evening warming caused by occupancy and heating.
* Evidence of a gradual cooling trend across the observation period.
* Repeatable daily patterns despite changing absolute temperatures.
* Distinct thermal signatures associated with work routine and occupancy behaviour.

### Baseline Evaluation

Analysis of z-score behaviour identified:

* A gradual reduction in daily mean z-score over time.
* Increasingly negative overnight and morning z-scores.
* Afternoon z-scores remaining close to expected values.
* Stronger negative departures occurring during the period when the office is most exposed to external environmental conditions.

## Evaluation

The original hypothesis proposed that increasing anomaly detections might be caused by baseline drift within the anomaly detection model.

Analysis shows that temperatures are becoming progressively colder relative to the statistical baseline, particularly during overnight and early morning periods. However, the evidence does not yet conclusively demonstrate that the baseline model itself is becoming invalid.

The hourly z-score analysis suggests a more nuanced explanation.

During overnight hours the office is minimally influenced by occupancy, portable heating and equipment. Consequently, indoor temperatures are likely to be most strongly coupled to external weather conditions during this period. The increasing negative z-scores therefore correspond to the period where seasonal cooling or short-term cold weather would be expected to have the greatest influence.

Conversely, afternoon temperatures remain close to baseline expectations, suggesting that the model continues to perform well when the office environment is regulated by the home's central heating system.

The current data therefore supports two competing explanations:

1. The onset of baseline drift caused by seasonal cooling.
2. A temporary colder-than-average weather period that will resolve naturally.

At present, the dataset is insufficient to distinguish between these possibilities with confidence.

## Conclusions

### Question 1

**How do the daily minimum, mean and maximum temperatures change over time?**

Daily minimum, mean and maximum temperatures all demonstrate a gradual downward trend. Mean temperature declined at approximately -0.091°C per day across the observation period.

### Question 2

**Are there identifiable colder periods that may correspond to frost events or colder external weather conditions?**

Yes. Distinct colder periods are visible within the data, particularly overnight and during early morning hours. Direct confirmation of frost influence will require future external temperature measurements.

### Question 3

**Are there repeating temperature patterns throughout each day?**

Yes. Strong and highly repeatable daily temperature cycles are present. Occupancy, central heating and portable heater usage all contribute recognisable thermal signatures that can be identified within both hourly averages and heatmap visualisations.

### Question 4

**Does the anomaly detection model accurately reflect unusual conditions, or is the baseline beginning to drift as seasonal conditions change?**

The available evidence suggests that colder overnight temperatures are increasingly being identified as unusual relative to the established baseline. However, it remains unclear whether this reflects genuine baseline drift or a short-term period of colder weather. Additional observations will be required to distinguish between seasonal change and temporary meteorological variation.

## Reflections

This session marked the first transition from environmental monitoring into genuine environmental interpretation.

Previous work focused primarily on collecting, validating and storing environmental observations. Today's work demonstrated how accumulated data can be used to investigate physical processes occurring within the home environment.

The temperature heatmap proved particularly insightful. Rather than simply revealing environmental conditions, it exposed behavioural patterns associated with occupancy, work location and heating usage. The ability to distinguish home-working days from days spent in the Manchester office illustrates how environmental data can indirectly capture human activity and building behaviour.

The evaluation of anomaly detection produced an important scientific lesson. Initial assumptions suggested that increasing anomaly counts implied baseline drift. However, further analysis indicated that the situation is more complex. The observed deviations may reflect genuine seasonal cooling or a short-term cold spell rather than a failure of the baseline itself.

This highlights the importance of avoiding premature conclusions and allowing hypotheses to remain open until additional evidence becomes available.

The session also reinforced the value of long-term data collection. Many of the questions being explored cannot be fully answered after only a few weeks of monitoring. Determining seasonal averages, identifying cold spells and assessing baseline stability all require observation over longer timescales.

Overall, the session successfully answered the original temperature analysis questions while raising new and more interesting questions regarding weather, climate, anomaly detection and the influence of external environmental conditions on indoor spaces.

## Next Actions

* Complete the transition to atmospheric pressure analysis.
* Investigate long-term pressure trends.
* Examine pressure variation associated with changing weather systems.
* Explore relationships between pressure and temperature behaviour.
* Continue collecting indoor environmental observations.
* Complete construction of the ESP32-based outdoor monitoring node.
* Begin collection of external temperature data for indoor/outdoor comparison.
* Investigate thermal lag between external and internal environmental conditions.
* Revisit anomaly detection performance after several additional weeks of observations.
* Assess whether negative morning z-scores recover towards baseline levels as weather conditions change.