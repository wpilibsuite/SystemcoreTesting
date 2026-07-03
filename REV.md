# REV Alpha Software for 2027

> [!NOTE]
> If you are testing the A301, see [A301.md](A301.md) for everything you need: firmware downloads, version compatibility, and getting started with the API.

## REVLib

REVLib should be available in the vendor dependencies of WPILib VS Code, but you can also use this JSON URL directly:

```txt
https://software-metadata.revrobotics.com/REVLib-2027.json
```

[Offline Install](https://github.com/REVrobotics/REV-Software-Binaries/releases/download/revlib-2027.0.0-alpha-4/REVLib-offline-v2027.0.0-alpha-4.zip)

Refer to [WPILib Docs](https://docs.wpilib.org/en/stable/docs/software/vscode-overview/3rd-party-libraries.html) about installing 3rd party libraries.

> [!NOTE]
> REVLib version requirements for A301 firmware are listed in [A301.md](A301.md).

<details>
<summary>Changelog</summary>

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

[RHC2 for Desktop](https://alpha.rhc2.revrobotics.com/download-site/download.html)

[RHC2 IPK for Systemcore](https://alpha.rhc2.revrobotics.com/download-site/debian/rev-robotics-rev-hardware-client-alpha_1.2.1_arm64.ipk) - Install this by clicking "Add Package" on the home screen of Systemcore and selecting this file. It will take a minute or so to start up.

<details>
<summary>Changelog</summary>

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
