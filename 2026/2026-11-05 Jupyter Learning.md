# Trillium Project Journal

**Date:** 05 November 2026

## Session Focus

With the environmental monitoring project operating reliably and continuing to collect data, the focus shifted towards learning computational science techniques using Python, Jupyter Notebooks and simulation.

A new project, **GravityLab**, was created to provide a sandbox for experimenting with scientific modelling without requiring additional hardware.

---

## Objectives

- Learn the fundamentals of Jupyter Notebooks.
- Learn Markdown and LaTeX equation formatting.
- Create a simple pendulum simulation.
- Explore the relationship between pendulum length and period.
- Understand the impact of gravity on pendulum motion.
- Introduce simulated measurement noise.
- Apply statistical techniques to recover physical values from noisy data.

---

## Work Completed

### Environment Setup

Created a new project structure:

    Projects/
    └── GravityLab/
        └── notebooks/

Configured Jupyter within the existing science virtual environment.

Investigated PDF export functionality and identified missing Pandoc dependency. Successfully worked around the issue by exporting to HTML and printing to PDF without making further system changes.

---

### Jupyter and Markdown

Learned the workflow of:

- Markdown cells
- Code cells
- Executing cells independently
- Combining documentation and executable code

Experimented with LaTeX equations and confirmed rendering of scientific notation.

Example:

$$
T = 2\pi\sqrt{\frac{L}{g}}
$$

Initial impression is extremely positive. The notebook format allows theory, code, results and observations to exist together in a single document.

---

### Pendulum Simulation

Implemented the standard pendulum equation:

$$
T = 2\pi\sqrt{\frac{L}{g}}
$$

Investigated the effect of varying pendulum length while keeping gravity constant.

Produced the first scientific plot showing:

- Increasing pendulum length increases period.
- The relationship is not linear.
- The curve gradually flattens due to the square root relationship.

This provided a practical demonstration that many real-world relationships are non-linear.

---

### Pendulums on Other Worlds

Compared pendulum behaviour under different gravitational fields:

- Moon
- Mars
- Earth
- Jupiter

Confirmed that lower gravity results in longer oscillation periods and demonstrated how simulation can be used to investigate phenomena that cannot easily be tested physically.

---

### Estimating Gravity

Rearranged the pendulum equation to estimate gravity from observed pendulum periods:

$$
g = \frac{4\pi^2L}{T^2}
$$

Successfully recovered values close to Earth's gravity from simulated observations.

This introduced the concept of solving the inverse problem:

- Instead of predicting measurements from known physics.
- Infer physical properties from measurements.

---

### Measurement Noise and Statistics

Introduced random measurement error into simulated observations.

Initially used:

    random.uniform()

This produced an unexpected distribution.

Investigation revealed that a uniform error model generates fundamentally different behaviour from naturally occurring measurement noise.

Updated the model to use:

    random.gauss()

to simulate normally distributed measurement error.

Generated histogram visualisations of the resulting gravity estimates.

Observed the emergence of a classic bell-shaped distribution centred around the expected gravitational value.

---

### Large Sample Simulation

Increased the number of simulated measurements significantly and observed:

- Improved stability of the estimate.
- Smoother distributions.
- Clear convergence towards the true value.

This reinforced concepts already encountered in the environmental monitoring project:

- Averages become more reliable with larger sample sizes.
- Statistical distributions emerge from noisy measurements.
- Individual observations can be inaccurate while aggregate results remain highly accurate.

---

## Key Learning Points

### Jupyter Notebooks

A notebook acts as a combination of:

- Laboratory notebook
- Technical report
- Python application

allowing questions, theory, calculations, graphs and observations to exist in a single executable document.

### LaTeX

Scientific notation is both practical and aesthetically pleasing within notebooks.

### Simulation

Before building hardware, it is possible to model expected behaviour and develop an understanding of the underlying physics.

### Statistics

Different sources of randomness produce different distributions.

Uniform noise:

    All values equally likely.

Gaussian noise:

    Small errors common.
    Large errors rare.

This has direct relevance to future sensor and scientific projects.

---

## Connections to the Climate Project

A significant observation during the session was the similarity between the gravity experiment and the environmental monitoring project.

Both systems follow the pattern:

    True Value
        ↓
    Measurement Noise
        ↓
    Observations
        ↓
    Statistical Analysis
        ↓
    Best Estimate

The same mathematical and statistical techniques apply regardless of whether the quantity being measured is:

- Temperature
- Humidity
- Pressure
- Pendulum Period
- Gravitational Acceleration

This reinforces the value of learning general analytical techniques rather than focusing solely on individual projects.

---

## Reflections

This session provided a highly productive introduction to computational physics and scientific notebooks.

The ability to modify assumptions, rerun calculations and immediately update graphs creates a powerful exploratory workflow which feels significantly different from traditional Python scripts.

Jupyter Notebooks appear likely to become a valuable tool not only for future physics experiments but also for analysis of climate monitoring data.

The most important lesson of the evening was not the pendulum itself, but the discovery of a new way of working: combining theory, code, results and documentation into a single reproducible scientific workflow.

---

## Next Steps

### GravityLab Notebook 02

Investigate:

> How many measurements are required to obtain a reliable estimate of gravity?

Potential topics:

- Sample size
- Confidence intervals
- Averaging multiple observations
- Measurement uncertainty

### Future Hardware Experiment

Design a real pendulum apparatus using:

- ESP32 or Raspberry Pi
- IR breakbeam sensor
- MQTT
- SQLite

with the long-term goal of measuring local gravitational acceleration experimentally.

### Environmental Monitoring

Allow the climate monitoring project to continue collecting data uninterrupted in order to increase baseline accuracy and prepare for future anomaly analysis and seasonal modelling.