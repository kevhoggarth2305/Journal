# 2026-10-27 Phase 1 Deployment

## Session Details

| Date | Start | End | Location | Project | Experiment(s) | Equipment |
| --- | --- | --- | --- | --- | --- | --- |
| 27/10/2026 | 16:00 | 17:00 | Home Office | Weather Station | Phase 1 Deployment - Autonomous Weather Logging Service | Raspberry Pi 5<br>BME280 Temperature/Humidity/Pressure Sensor<br>VS Code<br>Trilium Notes<br>SQLite Database<br>systemd Service Manager |

## Objective

Complete Phase 1 deployment of the Weather Science Project by converting the weather logger into a continuously operating service capable of autonomous operation, automatic startup and recovery following system reboot.

## Work Conducted

* Modified `weather_logger.py` to operate continuously using a looping data acquisition process.
* Added timed interval execution between sensor readings.
* Conducted repeated logging tests using a 30-second sampling interval.
* Verified multiple consecutive sensor reads and database inserts.
* Confirmed correct operation of SQLite database storage during continuous execution.
* Created `weatherstation.service` using systemd.
* Configured the service to use the dedicated `science-env` Python virtual environment.
* Enabled automatic service startup during Raspberry Pi boot.
* Started and validated service operation using `systemctl`.
* Investigated service logging using `journalctl`.
* Tested automatic service recovery following Raspberry Pi reboot.
* Verified continued data collection following system restart.
* Queried the SQLite database using both VS Code and the SQLite command line interface.
* Confirmed successful and ongoing weather data collection.

## Observations

* Continuous weather logging operated reliably during testing.
* Multiple sequential weather observations were successfully recorded without intervention.
* BME280 measurements remained stable and consistent throughout the session.
* Service status correctly reported:

  * Active: active (running)

* The weather logging service continued running independently of the terminal session.
* New observations continued to be added to the database while the service was running in the background.
* Following reboot, the service restarted automatically and resumed logging without any manual action.
* SQLite command-line operation required SQL statements to be terminated correctly using a semicolon.
* Database record count reached 18 observations by the conclusion of the session.
* Journal output appeared limited due to Python output buffering, however this did not affect data collection.

## Issues Encountered

* Initial uncertainty regarding operation of multiple instances of the logger while the service was running.
* Python application output was not immediately visible within the system journal.
* An incomplete SQL statement caused SQLite to enter multiline input mode until cancelled.

## Decisions Made

| Decision | Reason | Impact |
| --- | --- | --- |
| Deploy weather_logger.py as a systemd service | Allows autonomous operation without requiring a logged-in user | Enables long-term unattended data collection |
| Use a 5-minute collection interval for operational logging | Provides adequate weather sampling while reducing database growth | Suitable for multi-week deployments |
| Retain SQLite as the primary storage mechanism | Supports querying, analysis and concurrent access | Enables future reporting and dashboard development |
| Validate automatic recovery through reboot testing | Confirms operational resilience of the platform | Provides confidence for unattended operation |
| Defer dashboard and analysis development until data collection is established | Prioritises platform stability and data acquisition | Establishes a trusted baseline dataset |

## Resources Referenced

* systemd service management documentation
* SQLite command-line documentation
* journalctl logging tools
* Python virtual environment configuration
* Existing Weather Science Project codebase

## Next Actions

* Commit Phase 1 deployment milestone to Git.
* Allow continuous collection of environmental observations.
* Monitor record growth over the coming days.
* Develop database query utilities.
* Create graphical visualisation tools.
* Generate daily and weekly summary statistics.
* Investigate dashboard options for live monitoring.
* Prepare for future gravity experiments using environmental measurements as contextual data.

## Reflections

This session marked the successful deployment of the first autonomous version of the Weather Science Project. The system now operates as a persistent scientific instrument rather than a manually executed script. Weather observations are collected automatically, stored within SQLite and preserved across system restarts.

Successful validation of automatic startup and post-reboot recovery represents the completion of Phase 1. With data acquisition now operating independently, future development can focus on data analysis, visualisation, environmental trend identification and support for planned gravity-related experiments.

The Raspberry Pi weather station is now deployed and collecting scientific data continuously.