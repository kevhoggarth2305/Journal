# 2026-11-15 ESP32 Sensor Node Build and Multi-Sensor Database Integration

## Session Details

| Date | Start | End | Location | Project | Experiment(s) | Equipment |
| --- | --- | --- | --- | --- | --- | --- |
| 15/11/2026 | 12:00 | 17:00 | Home Office | Weather Station | ESP32 Sensor Node Construction, BME280 Integration, Multi-Sensor SQLite Architecture | Raspberry Pi 5<br>ESP32 Dev Module<br>BME280 Sensor Module<br>Prototype Board<br>Terminal Blocks<br>Dupont Leads<br>SQLite Database<br>VS Code<br>PlatformIO<br>Python<br>Systemd Services<br>Copilot |

---

## Objective

Construct and validate a dedicated ESP32-based environmental sensor node, establish communication with a BME280 sensor, integrate the new sensor into the existing Weather Station architecture, and extend the database design to support multiple independent sensor locations.

---

## Work Conducted

* Designed the first ESP32 sensor node hardware layout.
* Evaluated mounting options for the BME280 and cable termination hardware.
* Constructed a prototype sensor board using prototyping PCB.
* Installed terminal block and BME280 header connections.
* Verified board continuity using a multimeter.
* Diagnosed a faulty terminal block after continuity testing and mechanical inspection.
* Successfully desoldered and removed the defective component.
* Installed a replacement terminal block.
* Re-tested all signal paths.
* Confirmed correct continuity between ESP32 pins and sensor pins.
* Confirmed the absence of shorts between adjacent signals.
* Created a new PlatformIO project named `WeatherSensor`.
* Configured PlatformIO for ESP32 development using the Arduino framework.
* Re-established successful compilation and flashing workflow.
* Investigated ESP32 upload behaviour and confirmed the manual BOOT-button upload sequence.
* Verified serial communication through the PlatformIO monitor.
* Developed and executed an I²C scanner.
* Successfully detected the BME280 sensor at address `0x76`.
* Installed the Adafruit BME280 library via PlatformIO.
* Implemented initial sensor-reading firmware.
* Successfully obtained temperature, humidity and pressure measurements from the ESP32.
* Created a consolidated VS Code workspace containing:
  * WeatherStation
  * ESP32
  * Journal
* Redesigned database architecture to support multiple sensor locations.
* Created a new `locations` table.
* Added `location_id` to the existing `readings` table.
* Migrated all historical readings to the new schema.
* Created a location-aware database view.
* Modified the original Raspberry Pi sensor logger to record `location_id = 1`.
* Created a Python serial logger for the ESP32 sensor.
* Successfully inserted ESP32 measurements into SQLite using `location_id = 2`.
* Created and enabled `esp32logger.service`.
* Confirmed automatic startup and database logging via systemd.

---

## Observations

* Continuity testing identified a mechanically defective terminal block before power was applied.
* Desoldering and component replacement proved substantially more difficult than initial assembly.
* The ESP32 upload process on this particular board requires manual intervention using the BOOT button.
* Pressing the EN button reliably triggers firmware restart and serial output.
* The BME280 sensor was immediately detected once I²C communication was established.
* The ESP32 and Raspberry Pi sensors reported similar but not identical environmental values.
* Initial temperature differences approached approximately 1.5°C.
* Later measurements showed significantly closer agreement.
* Pressure measurements between sensors differed by only a few tenths of a hPa.
* Humidity measurements differed by less than 2%.
* The ESP32 sensor appears to be stabilising as operating conditions become more consistent.

---

## Results

### ESP32 Sensor Detection

```text
Scanning...
Found device at 0x76
```

Successfully confirmed:

* Sensor power.
* I²C communication.
* Correct SDA/SCL wiring.
* Functional BME280 hardware.

### Initial Sensor Measurements

```text
Temperature: 20.75 °C
Humidity: 48.59 %
Pressure: 995.64 hPa
```

### Database Schema Extension

New table:

```sql
locations
```

Initial records:

| ID | Name |
| --- | --- |
| 1 | Office |
| 2 | ESP32_Test |

New column added:

```sql
readings.location_id
```

### Multi-Sensor Logging Verification

Representative entries:

```text
location_id = 1  -> Original Office Sensor
location_id = 2  -> ESP32_Test Sensor
```

Example output:

```text
5471 | 2026-11-15T16:12:18 | location_id=1
5470 | 2026-11-15T16:10:16 | location_id=2
5469 | 2026-11-15T16:10:06 | location_id=2
```

This confirms successful simultaneous logging from two independent sensor systems into a shared readings table.

---

## Technical Lessons Learned

* Mechanical testing of components before soldering is just as important as electrical testing.
* Continuity testing can prevent lengthy fault-finding later in the build process.
* Systematic diagnosis is more effective than making assumptions about faults.
* The ESP32 development workflow is now understood:
  * Build
  * Upload
  * Hold BOOT during connection
  * Use EN to restart if necessary
* PlatformIO monitor baud rate must match firmware serial configuration.
* SQLite schema evolution can be performed safely using incremental migration steps and validation queries.
* Using location identifiers provides a scalable architecture for future sensor expansion.

---

## Next Actions

* Leave both sensors running overnight to accumulate comparison data.
* Quantify average offsets between Office and ESP32_Test sensors.
* Investigate whether observed temperature differences are stable over time.
* Develop comparison notebooks in Jupyter.
* Visualise temperature, humidity and pressure differences between sensor locations.
* Assess long-term sensor agreement before outdoor deployment.
* Replace USB communication with WiFi-based transmission.
* Design permanent enclosure and power solution for the ESP32 sensor node.
* Prepare the first outdoor deployment location.

---

## Reflections

This session represents one of the largest single advances in the Weather Station project so far. The project began the day with a single Raspberry Pi based sensor and finished with a fully operational multi-sensor architecture capable of supporting independent environmental monitoring locations.

The hardware work provided valuable practical experience. The discovery of a defective terminal block highlighted the importance of validating components before installation. Although desoldering and replacement proved frustrating, the process reinforced the value of careful diagnostic testing. The fault was identified entirely through continuity measurements and logical investigation before any power was applied, preventing significantly more complicated troubleshooting later in the build.

The ESP32 development environment also moved from unfamiliar territory to a repeatable and understood workflow. Firmware creation, compilation, upload, serial monitoring and I²C device discovery were all successfully demonstrated. Detection of the BME280 at address 0x76 represented the first confirmation that the custom sensor board was functioning correctly. Obtaining live environmental readings from hardware assembled, soldered and configured during the same session was particularly satisfying.

The database redesign may ultimately prove as significant as the hardware work. By introducing a dedicated locations table and linking all readings through a location identifier, the architecture now scales naturally to future expansion. Additional indoor, outdoor, loft or garden sensors can be added without restructuring the database or analysis workflows. The decision to normalise the schema before introducing new sensors prevented technical debt and established a much cleaner foundation for future development.

Perhaps the most scientifically interesting outcome is that two sensors are now observing the same environment simultaneously. The initial temperature differences observed between the Office sensor and the ESP32_Test sensor provide an immediate opportunity for calibration and comparative analysis. Rather than viewing these differences as errors, they should be considered useful information about sensor behaviour, measurement uncertainty and environmental influences. Overnight logging will provide the first meaningful dataset for quantifying these relationships.

Overall, the project has evolved from a single-sensor environmental logger into a genuine multi-sensor environmental monitoring platform. The combination of custom hardware, embedded software, database engineering and environmental science makes this one of the most rewarding and significant project milestones achieved to date.