# 2026-10-26 Phase 1 Begins

## Session Details

| Date | Start | End | Location | Project | Experiment(s) | Equipment |
| --- | --- | --- | --- | --- | --- | --- |
| 26/10/2026 | 18:00 | 20:00 | Home Office | Weather Station | Phase 1 - SQLite Weather Logger | Raspberry Pi 5<br>BME280 Temperature/Humidity/Pressure Sensor<br>Windows Laptop (Remote Desktop)<br>VS Code<br>Trilium Notes<br>SQLite Database |

## Objective

Complete migration of project documentation to the Raspberry Pi and commence Phase 1 of the Weather Science Project by creating a database-backed weather logging system.

## Work Conducted

* Installed and updated Trilium Notes on the Raspberry Pi.
* Imported existing Trilium notebooks from the Ubuntu laptop export.
* Created the WeatherStation project folder structure.
* Initialised Git repository and established version control for the project.
* Configured VS Code to use the dedicated science-env Python virtual environment.
* Installed Adafruit BME280 and Blinka libraries within the virtual environment.
* Resolved missing platform dependencies reported by Blinka.
* Created and validated a SQLite database for weather observations.
* Designed and implemented the readings database table.
* Developed the first operational version of weather_logger.py.
* Integrated BME280 sensor readings with SQLite database storage.
* Successfully recorded the first automated weather observation.

## Observations

* Trilium migration was completed successfully and all existing project notes are now available on the Raspberry Pi.
* The science-env virtual environment was correctly configured and selected as the VS Code interpreter.
* Virtual environments do not inherit packages from the system Python installation and require project-specific dependencies to be installed.
* Blinka automatically detected and installed missing Raspberry Pi platform dependencies.
* An initial issue was identified where weather.db had been created as a plain text file rather than a SQLite database.
* Recreating the database correctly resolved all database access issues.
* First successful weather observation recorded:

  * Timestamp: 2026-10-26T19:50:29
  * Temperature: 23.7°C
  * Humidity: 44.3%
  * Pressure: 996.7 hPa

* This represents the first permanent weather measurement stored by the Weather Science Project.

## Issues Encountered

* Adafruit sensor libraries were not initially installed within the virtual environment.
* Blinka reported missing Raspberry Pi platform dependencies.
* weather.db was initially created as an ASCII text file rather than a valid SQLite database.
* Unsaved edits in VS Code initially resulted in execution of an older version of weather_logger.py.

## Decisions Made

| Decision | Reason | Impact |
| --- | --- | --- |
| Use SQLite instead of CSV files | Better support for long-term storage, querying and analysis | Provides a scalable and robust scientific data platform |
| Use science-env as the standard Python interpreter | Keeps project dependencies isolated from the operating system | Improves reproducibility and simplifies maintenance |
| Install and maintain Trilium directly on the Raspberry Pi | Keeps project documentation and software together on one platform | Simplifies project management |
| Validate manual data logging before implementing automation | Allows database and software design to be verified first | Reduces troubleshooting complexity |
| Use Git from project inception | Provides version control and recovery capability | Protects project development and documentation |

## Resources Referenced

* Adafruit BME280 documentation
* Adafruit Blinka documentation
* SQLite documentation
* VS Code Python interpreter configuration guidance
* Git version control documentation

## Next Actions

* Add error handling and resilience to weather_logger.py.
* Verify multiple consecutive database inserts.
* Create database query and inspection utilities.
* Automate weather data collection at regular intervals.
* Develop graphical visualisation of collected observations.
* Generate statistical summaries from stored weather data.
* Commit Phase 1 development milestone to Git.

## Reflections

This session marked the transition from infrastructure setup to operational scientific data collection. The Raspberry Pi platform is now fully configured, project documentation has been migrated successfully, and the first weather observation has been permanently recorded in a structured database. The project now possesses a stable technical foundation for automated weather logging, analysis and future forecasting experiments. The successful recording of the first observation represents the beginning of the data acquisition phase and the first operational milestone of the Weather Science Project.