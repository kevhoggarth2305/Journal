# 2026-11-11 First ESP32 Bring-Up and Development Environment Setup

## Session Details

| Date | Start | End | Location | Project | Experiment(s) | Equipment |
| --- | --- | --- | --- | --- | --- | --- |
| 11/11/2026 | 19:00 | 21:00 | Home Office | ESP32 Development | Initial Board Bring-Up, PlatformIO Setup and First Firmware Upload | Raspberry Pi 5<br>ESP32-WROOM-32 Development Board<br>ESP32 Screw Terminal Breakout Board<br>VS Code<br>PlatformIO<br>USB Cable |

## Objective

Establish a complete ESP32 development environment on the Raspberry Pi, create a first PlatformIO project, compile and upload firmware, and gain an initial understanding of the Arduino execution model and ESP32 development workflow.

## Work Conducted

* Installed PlatformIO extension within VS Code.
* Investigated extended PlatformIO initialisation delays.
* Diagnosed extension activation issues related to project discovery and workspace structure.
* Confirmed PlatformIO Core installation within the Raspberry Pi user environment.
* Verified Python and PlatformIO dependencies.
* Created a dedicated ESP32 project structure:

```text
Projects/
└── ESP32/
    └── Blink/
```

* Generated a new PlatformIO ESP32 project using the Arduino framework.
* Examined the automatically generated project structure and configuration files.
* Reviewed the contents of `platformio.ini`.
* Created the first C++ source file (`main.cpp`).
* Developed a simple serial-output firmware application.
* Successfully compiled the firmware using the ESP32 toolchain.
* Connected the ESP32-WROOM-32 development board to the Raspberry Pi via USB.
* Identified the board serial interface as:

```text
/dev/ttyUSB0
```

* Attempted initial firmware uploads.
* Investigated upload failures associated with the ESP32 stub flasher.
* Reduced upload speed to improve communication reliability.
* Successfully uploaded firmware to the ESP32.
* Opened and configured the serial monitor.
* Observed continuous output from the running firmware.
* Performed a manual reset using the EN button.
* Observed the complete boot sequence and user-generated serial output.
* Discussed the relationship between:
  * ESP32 bootloader
  * Arduino framework
  * `setup()`
  * `loop()`
  * underlying C++ execution

## Observations

* Initial PlatformIO installation completed successfully despite appearing stalled.
* Opening VS Code directly from the project root resolved PlatformIO project discovery issues.
* The ESP32 board was detected immediately by Linux without requiring additional drivers.
* The ESP32 communicated successfully with the Raspberry Pi over USB.
* Upload failures were caused by communication timing rather than board or cable issues.
* Reducing upload speed to 115200 resolved flashing reliability problems.
* The firmware executed immediately after successful upload.
* Startup messages generated during `setup()` were missed initially because the serial monitor was opened after boot completed.
* Pressing the EN button allowed observation of the complete startup sequence.
* The Arduino framework provides the hidden `main()` function, simplifying embedded development.
* The distinction between one-time initialisation (`setup()`) and continuous execution (`loop()`) became much clearer through practical testing.

## Results

### Successful Firmware Build

Firmware compiled successfully using:

```text
PlatformIO
ESP32 Toolchain
Arduino Framework
```

### Successful Firmware Upload

After reducing upload speed:

```ini
upload_speed = 115200
monitor_speed = 115200
```

firmware uploaded successfully and executed correctly.

### Successful Serial Output

After board reset:

```text
ets Jul 29 2019 12:21:46

rst:0x1 (POWERON_RESET)
boot:0x13 (SPI_FAST_FLASH_BOOT)

...

Hello Kev!
ESP32-WROOM is alive!
Running...
Running...
```

This confirmed successful operation of:

* ESP32 bootloader
* Firmware upload process
* Serial communications
* User application code

## Issues Encountered

* PlatformIO extension appeared to remain indefinitely in an "Initialising PlatformIO Core" state.
* VS Code was unable to locate a valid PlatformIO project when opened from a higher-level directory.
* Initial firmware uploads failed with:

```text
Failed to start stub flasher
```

* Startup messages were not initially visible because the serial monitor connected after the board had already completed boot.

## Decisions Made

| Decision | Reason | Impact |
| --- | --- | --- |
| Develop ESP32 projects on Raspberry Pi | Integrates with existing Weather Station environment | Single development platform |
| Use PlatformIO rather than Arduino IDE | Better project structure and scalability | More professional development workflow |
| Create dedicated ESP32 project hierarchy | Supports future expansion and experimentation | Improved organisation |
| Fix upload speed at 115200 baud | Resolved upload reliability issues | Stable flashing workflow |
| Focus on serial communications before sensors | Simplifies troubleshooting and learning | Stronger foundational understanding |
| Explore execution model before hardware integration | Important for learning embedded development concepts | Better understanding of program flow |

## Equipment Register Update Required

### New Equipment

* ESP32-WROOM-32 Development Board
* ESP32 Screw Terminal Breakout Board

### Suggested Register Notes

Purpose:

```text
Microcontroller development, environmental monitoring,
wireless sensor platforms and ESP32 experimentation.
```

Status:

```text
Operational and tested.
```

Location:

```text
Science Lab
```

## Resources Referenced

* PlatformIO Documentation
* ESP32 PlatformIO Board Configuration
* Arduino Framework Documentation
* ESP32 Serial Monitor
* Linux USB Device Enumeration
* VS Code PlatformIO Extension

## Next Actions

* Create an ESP32 information project.
* Explore device metadata functions:
  * Chip model
  * CPU frequency
  * Flash size
  * SDK version
  * MAC address
* Experiment with timing functions such as `millis()`.
* Begin wiring and testing a BME280 sensor.
* Read and display environmental measurements via serial output.
* Investigate Wi-Fi connectivity.
* Begin planning an ESP32-based wireless weather sensor node.

## Reflections

This session marks a significant expansion of the wider Weather Station project into embedded systems development. While previous work has concentrated on Raspberry Pi data acquisition, SQLite databases and Python-based analytics, this was the first practical experience with a microcontroller platform.

The most valuable outcome was not simply getting firmware to run but understanding the overall execution model used by Arduino-based ESP32 development. Seeing the complete cycle from power-on, through bootloader execution, into framework initialisation and finally into user code provided a much clearer understanding of how microcontroller applications differ from traditional Python programs.

Troubleshooting the PlatformIO installation and firmware upload process also provided useful experience with the underlying development environment. While time-consuming, these issues helped develop familiarity with project structure, tooling and device communications.

From an engineering perspective, the session successfully demonstrated the complete firmware development lifecycle: write, compile, upload and execute. The appearance of the first custom serial messages from the board represented a satisfying confirmation that the entire toolchain was functioning correctly.

Most importantly, the ESP32 is now a usable platform rather than an unopened component. Future sessions can therefore focus on experimentation and sensor integration rather than software installation and configuration. With the development environment now operational, a logical next step is to begin connecting environmental sensors and exploring how wireless sensor nodes might eventually integrate with the wider Weather Station architecture.