# 2026-11-25 Phase 5 Development - MQTT Migration and Wireless Sensor Deployment

## Session Details

| Date | Start | End | Location | Project | Experiment(s) | Equipment |
| --- | --- | --- | --- | --- | --- | --- |
| 25/11/2026 | Evening | Evening | Home Office | Weather Station (Trillium) | MQTT Migration, Wireless Communications and Service Deployment | Raspberry Pi 5<br>ESP32 Development Board<br>BME280 Temperature/Humidity/Pressure Sensor<br>SQLite Database<br>Mosquitto MQTT Broker<br>VS Code<br>PlatformIO<br>Python<br>systemd |

## Objective

Remove the USB serial dependency between the ESP32 and Raspberry Pi by migrating communications to MQTT over Wi-Fi while maintaining automatic insertion of weather data into the SQLite database and preserving compatibility with existing Jupyter analysis notebooks.

## Work Conducted

* Reviewed the existing architecture and confirmed the ESP32 was transmitting data via USB serial rather than HTTP.
* Installed and configured Mosquitto MQTT Broker on the Raspberry Pi.
* Successfully tested MQTT using:
  * `mosquitto_pub`
  * `mosquitto_sub`
* Added Wi-Fi support to the ESP32 firmware.
* Added MQTT support using the PlatformIO PubSubClient library.
* Configured the ESP32 to connect to the home wireless network.
* Verified successful network connection and DHCP address assignment.
* Confirmed MQTT publishing from the ESP32 to:
  
  ```text
  weather/ESP32_Test
  ```

* Diagnosed MQTT connection failures.
* Determined that Mosquitto was initially listening only on:

  ```text
  127.0.0.1
  ```

* Reconfigured Mosquitto to accept network connections from external devices.
* Confirmed successful MQTT communication between the ESP32 and Raspberry Pi.
* Verified reception of sensor data using MQTT subscriptions.
* Developed a replacement Python logger using:
  * `paho-mqtt`
  * SQLite
* Replaced serial data acquisition with MQTT message processing.
* Successfully inserted MQTT-based readings into the `readings` table.
* Created a new:

  ```text
  mqttlogger.service
  ```

  systemd service.

* Configured:
  * automatic startup
  * automatic restart
  * virtual-environment execution
  * real-time console logging
* Diagnosed service startup failures caused by an incorrect virtual environment path.
* Corrected the service configuration and verified successful execution.
* Added unbuffered Python execution to improve systemd logging visibility.
* Disabled the legacy serial logger service.
* Enabled automatic startup of the MQTT logger service during system boot.
* Disconnected the ESP32 from the Raspberry Pi.
* Powered the ESP32 independently using a Raspberry Pi 3 power supply.
* Relocated the ESP32 from the calibration position to a temporary dining table location.
* Verified continued MQTT connectivity and database updates following relocation.

## Observations

* MQTT integration proved simpler than anticipated once broker configuration issues were resolved.
* The primary challenge was Mosquitto listener configuration rather than ESP32 development.
* PlatformIO and MQTT debugging provided clear diagnostics throughout the migration.
* Wi-Fi connectivity proved stable immediately after deployment.
* The migration removed the requirement for a permanent USB connection between sensor and logger.
* Running the logger under systemd restores the autonomous behaviour previously provided by the serial logger architecture.
* Real-time service monitoring through `journalctl` provides significantly better operational visibility than the previous serial design.

## Results

### Wireless Sensor Architecture

The following architecture is now operational:

```text
ESP32
   ↓ Wi-Fi
MQTT Broker
   ↓
mqttlogger.service
   ↓
SQLite Database
   ↓
Jupyter Notebooks
```

### Autonomous Operation

The system now automatically:

* Connects to Wi-Fi.
* Connects to MQTT.
* Publishes sensor readings.
* Receives MQTT messages.
* Inserts readings into SQLite.
* Restarts logging services following reboot.

No manual intervention is required following power restoration.

### Sensor Calibration Verification

Prior to relocation the sensors were operated side-by-side for calibration assessment.

Results:

```text
Temperature Difference : 0.03°C
Humidity Difference    : 1.28 %
Pressure Difference    : 0.72 hPa
```

These results indicate excellent agreement between the two sensors and provide confidence that future environmental differences will represent genuine location variation rather than sensor error.

### Independent Power Operation

The ESP32 successfully continued operation when disconnected from the Raspberry Pi and powered using an independent 5V supply.

The sensor:

* Reconnected to Wi-Fi automatically.
* Reconnected to MQTT automatically.
* Continued inserting records into SQLite.
* Required no manual intervention after power-up.

## Issues Encountered

* Firmware uploads initially failed because the serial logger service retained ownership of `/dev/ttyUSB0`.
* MQTT connections initially failed due to Mosquitto listening only on localhost.
* The MQTT logger service referenced an incorrect virtual environment path.
* Python output buffering prevented log messages appearing immediately within systemd journals.
* VS Code was briefly opened using an outdated workspace, causing confusion regarding Jupyter kernel selection.

## Decisions Made

| Decision | Reason | Impact |
| --- | --- | --- |
| Adopt MQTT as the primary communications mechanism | Removes USB dependency and improves scalability | Enables wireless deployment |
| Retain the existing SQLite architecture | Existing data model is functioning reliably | Minimises migration risk |
| Deploy logging as a systemd service | Supports autonomous operation | Better reliability |
| Disable legacy serial logger | Prevents service conflicts | Simplified architecture |
| Retain location ID 2 temporarily | Current deployment is a short-term wireless stability test | Avoids unnecessary database changes |
| Delay MQTT topic-based location mapping until permanent sensor deployment | Current testing does not require it | Reduced complexity |

## Resources Referenced

* Mosquitto MQTT Broker
* PubSubClient
* paho-mqtt
* SQLite
* systemd
* PlatformIO
* VS Code
* Jupyter Notebook
* Raspberry Pi 5
* ESP32
* BME280 Environmental Sensor

## Next Actions

* Monitor Wi-Fi and MQTT stability over several days.
* Complete testing from the dining table location.
* Relocate the sensor to the bedroom for medium-term environmental monitoring.
* Remove hard-coded location identifiers from the MQTT logger.
* Implement topic-based sensor identification.
* Support multiple MQTT-connected sensors through a single logging service.
* Investigate battery-powered operation using USB power banks.
* Evaluate outdoor housing and weatherproofing options.
* Research radiation shielding suitable for outdoor temperature monitoring.
* Prepare for a permanent outdoor sensor deployment.

## Reflections

This session represents one of the most significant infrastructure upgrades completed within the Trillium project.

The weather station has progressed from a development-oriented USB-connected sensor into a genuinely distributed monitoring platform. The successful migration to MQTT over Wi-Fi removes a major physical limitation and allows sensors to be placed according to environmental requirements rather than cable length.

Particularly encouraging was the calibration exercise completed prior to relocation. The measured agreement between sensors was considerably better than expected and confirms that future differences observed between locations are likely to represent genuine environmental variation rather than instrumentation error.

The migration also demonstrated the value of incremental engineering. Wi-Fi connectivity, MQTT communications, broker configuration, database integration and service deployment were all validated independently before being combined into the final architecture. This reduced troubleshooting complexity and allowed faults to be isolated efficiently.

The most significant milestone was the successful insertion of the first wireless MQTT reading into the production database. Once the ESP32 continued logging after being disconnected from the Raspberry Pi and powered independently, the project crossed an important threshold. The weather station is no longer a development prototype connected by USB; it is now a networked environmental monitoring platform capable of supporting future multi-room and outdoor deployments.

Future development will focus on permanent sensor locations, dynamic MQTT topic handling, and an eventual outdoor installation. The successful completion of this migration provides the technical foundation for all future phases of the project.