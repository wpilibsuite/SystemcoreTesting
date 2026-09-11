# REV Alpha Software for 2027

> [!NOTE]
> If you are testing the A301, see [A301.md](A301.md) for everything you need: firmware downloads, version compatibility, and getting started with the API.

## REVLib

REVLib should be available in the vendor dependencies of WPILib VS Code, but you can also use this JSON URL directly:

```txt
https://software-metadata.revrobotics.com/REVLib-2027.json
```

[Offline Install](https://github.com/REVrobotics/REV-Software-Binaries/releases/download/revlib-2027.0.0-alpha-7/REVLib-offline-v2027.0.0-alpha-7.zip)

Refer to [WPILib Docs](https://docs.wpilib.org/en/stable/docs/software/vscode-overview/3rd-party-libraries.html) about installing 3rd party libraries.

> [!NOTE]
> REVLib version requirements for A301 firmware are listed in [A301.md](A301.md).

<details>
<summary>Changelog</summary>

### REVLib v2027.0.0-alpha-7

- [REVLib] Updates to WPILib 2027 Alpha 7
- [REVLib] Device constructors now take the WPILib CANPort enum instead of a raw int for the CAN bus ID
- [REVLib] Renames getBusId() to getCanPort()
- [A301] Updates CAN specs and fixes inversion. Requires latest A301 firmware.
- [SPARK] Deprecates AbsoluteEncoderConfig.zeroCentered(). Use rangeOffset() instead.
- [SPARK] Removes hall sensor velocity averaging configurations in favor of new firmware filtering system
- [SPARK] Adjusts default encoder average depth to 8 and sample delta to 20
- [REVLib] Removes Conversion Factors in SPARKs and MAXSplineEncoder
- [SPARK] Removes ClosedLoopConfig.positionWrappingMinInput, ClosedLoopConfig.positionWrappingMaxInput, and ClosedLoopConfig.positionWrappingInputRange
- [REVLib] C++ - Replace usage of fmt library with std

### REVLib v2027.0.0-alpha-6

- [A301] Java: Fixes setCurrent()

### REVLib v2027.0.0-alpha-5

- [REVLib] Java: Fixes crash in getPeriodicStatus8()
- [A301] Adds IdleMode getter and setter
- [A301] Fixes inconsistencies when inverted
- [A301] Adds a busId-only constructor to C++
- [A301/SPARK] Adds ability to modify the absolute position range about zero -- (-1.0, 0] to [0, 1.0)

### REVLib v2027.0.0-alpha-4

- [REVLib] Removes deprecated functions
- [REVLib] Fixes links in documentation
- [REVLib] Fixes Sim classes to include CAN Bus ID in name
- [A301] - Adds new A301-specific CAN specifications
- [A301] - Removed isContinuous argument from setAbsolutePosition(); replaced with (En/Dis)ableAbsolutePositionContinuousInput()
- [A301] - Adds a setRelativePositionWithSpeed() and setAbsolutePositionWithSpeed() to run position closed loop control with a speed constraint
- [A301] - Updates A301(int busId) constructor to autodetect the device ID instead of requiring the device to be the factory default value of 3

### REVLib v2027.0.0-alpha-3

- [A301] Adds setRelativeEncoderPosition()
- [A301] Fixes setInverted() and getInverted()
- [ServoHub] Fixes internal crash when calling Status getters
- [A301] Renames getOutputCurrent() to getMotorCurrent()

### REVLib v2027.0.0-alpha-2

- [A301] Adds support for A301
- [REVLib] Java/C++: Creates a Signal wrapper type for signals, allowing a user to know if a value is outdated. This is a breaking change and will require user code to call `.get()` / `.get(default)` in Java or `Get()` in C++. User code can query `.isValid()` in Java or `IsValid()` in C++ to know whether the value is recent.

### REVLib v2027.0.0-alpha-1

- [REVLib] Adds support for Systemcore and the WPILib 2027 alpha
- [SPARK, Servo Hub] Adds `busId` parameter to class constructor to specify which CAN bus the device is on

</details>

## Hardware Client 2

[RHC2 for Desktop](https://rhc2.revrobotics.com/download/download.html)

[RHC2 IPK for Systemcore](https://rhc2.revrobotics.com/download/debian/rev-robotics-rev-hardware-client_1.4.2_arm64.ipk) - Install this by clicking "Add Package" on the home screen of Systemcore and selecting this file. It will take a minute or so to start up.

<details>
<summary>Changelog</summary>

## RHC2 1.4.2

- Adds download location prompt for download actions from Control Hub's Robot Controller Console
- Improves recovery of wireless Control Hub connections after a Wi-Fi drop

## RHC2 1.4.1

- Adds a prompt on startup when an update is available, with the option to update now, on exit, or ignore it
- Adds a prompt on startup showing the changelog after the app has been updated
- Adds visualizer for absolute encoder utility
- Add inversion configuration to A301
- Adds support for opening the update manager directly from a device's update tab to enable updating multiple devices of the same type
- Uses Control Hub's friendly name in device list
- Improves general experience with the FTC Log Viewer
- Updates the supported devices list on the About page to reflect all currently supported devices
- Fixes UI crash on Systemcore
- Fixes Robot Controller Console not working as expected for Control Hubs connected over USB
- Fixes Expansion Hub firmware version not showing up for Control Hub devices
- Fixes Control Hub menu labels being illegible in light mode
- Fixes false positive error toast when attempting to run an A301

## [1.4.0]

- Adds controller support for running motors
- Adds auto-fetching latest releases on startup
- Adds toast to show when an action failed due to being in read-only mode
- Adds setpoint presets to A301/SPARK run pages
- Adds FTC Log Viewer
- Fixes crash when adding closed loop control telemetry for a SPARK
- Fixes reset safe parameters showing the incorrect number of changes made
- Fixes run multiple checkbox not working with A301
- Fixes moving app to applications folder on macOS
- Fixes selecting an A301 opening a SPARK with the same CAN ID on the same bus

## RHC2 1.3.1

- Fixes crash on run tab with A301 on versions before prerelease 16

## RHC2 1.3.0

- Adds support for connecting to a remote RHC2 instance (like Systemcore) from a desktop client
- Adds support for installing/updating a Systemcore's RHC2 from the desktop client
- Adds ability for leader to release control and read only-clients to claim control
- Adds a prompt for Windows users to install DFU drivers if none are found
- Adds support for A301's absolute range offset parameter
- Adds tooltip and animation for identify button
- Improves robustness of leader/reader mechanism when clients disconnect
- Updates robot program override to only be controlled by the leader
- Fixes Systemcore serving a stale, cached UI after an update. You will need to empty cache and refresh the UI for this to take effect.
- Fixes frontend crash when downloads database file gets corrupted, particularly on Systemcore
- Fixes sorting of Motioncore buses in telemetry tab

## RHC2 1.2.1

- Adds support for new A301 CAN specification introduced in firmware version 27.0.0-prerelease.15
- Adds ability to specify a speed for position control in A301's run utility
- Adds a toggle on the Motioncore card to override the robot program to run motors without a driver station
- Adds gearbox and motor health status cards for A301
- Fixes devices not showing up on the Hardware page in Safari

## RHC2 1.2.0

- Adds support for REV FTC devices (Control Hub, Expansion Hub, Driver Hub, Driver Station Phone)
- Adds support for CAN Encoder Adapter
- Adds search bar for signals on Telemetry page
- Adds search bar for SPARK configuration tab
- Adds relative encoder utility to MAXSpline Encoder
- Fixes sidebar on telemetry expanding when a device has an alert in either run tab
- Fixes issue reporting
- Fixes issue where signals would freeze after closing a device
- Fixes issue with A301 CAN ID change not persisting
- Fixes issue where firmware versions sometimes do not populate in the update page

### Known Issues

- When connecting to a Control Hub via Wi-Fi, no other computer will be able to see the Control Hub until it is power cycled. The same laptop can continue to see and use the Control Hub even if it reconnects or RHC2 is reopened.

## RHC2 1.1.1

- Adds missing applied output signal for A301
- Adds indicator to navigation bar to show whether you are in read-only mode
- Improves startup time
- Improves CPU and memory utilization during runtime
- Improves leader/reader behavior
- Improves download page
- Improves update progress status display
- Fixes update progress status when updating multiple devices across separate buses

### RHC2 1.1.0

- Adds support for A301
- Adds support for Linux Arm64
- Adds support for multiple separate CAN buses
- Adds support for updating the bootloader of supported devices
- Adds support for devices connected via Motioncore when running on Systemcore
- Adds relative encoder utility for resetting relative position
- Improves SPARK setpoint handling to minimize interfering with a connected robot's setpoint commands
- Fixes issue where SPARKs with pre-2025 firmware are not detected on the CAN bus

</details>

## Firmware

### A301

A301 firmware downloads, update instructions, and the firmware changelog can be found in [A301.md](A301.md).

### Misc

Other than A301, there are currently no new device versions for 2027. Please use the latest 2026 releases of firmware for your devices.

<details>
<summary>Latest firmware versions</summary>

- SPARK Flex: `v26.1.6`
- SPARK MAX: `v26.1.5`
- Power Distribution Hub: `v26.1.3`
- Pneumatic Hub: `v26.1.3`
- Servo Hub: `v26.1.3`
- MAXSpline Encoder: `v26.1.3`

</details>
