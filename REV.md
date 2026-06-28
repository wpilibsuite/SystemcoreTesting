# REV Alpha Software for 2027

## REVLib

REVLib should be available in the vendor dependencies of WPILib VS Code, but you can also use this JSON URL directly:

```txt
https://software-metadata.revrobotics.com/REVLib-2027.json
```

[Offline Install](https://github.com/REVrobotics/REV-Software-Binaries/releases/download/revlib-2027.0.0-alpha-3/REVLib-offline-v2027.0.0-alpha-3.zip)

Refer to [WPILib Docs](https://docs.wpilib.org/en/stable/docs/software/vscode-overview/3rd-party-libraries.html) about installing 3rd party libraries.

<details>
<summary>Changelog</summary>

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

[RHC2 IPK for Systemcore](https://alpha.rhc2.revrobotics.com/download-site/debian/rev-robotics-rev-hardware-client-alpha_1.1.1_arm64.ipk) - Install this by clicking "Add Package" on the home screen of Systemcore and selecting this file. It will take a minute or so to start up.

<details>
<summary>Changelog</summary>

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

The recommend method to update the A301 firmware is through REV Hardware Client 2 running directly on Systemcore. Since Systemcore will likely not be connected to internet, you will need to upload the firmware file to RHC2 using the dialog:

![A301 update on systemcore](A301_Update.png)

Direct firmware downloads for updating via Systemcore:

- [A301 v2027.0.0-prerelease.14](https://github.com/REVrobotics/REV-Software-Binaries/releases/download/a301-27.0.0-prerelease.14/a301_27_0_0_prerelease_14.dfu)
- [A301 v2027.0.0-prerelease.12](https://github.com/REVrobotics/REV-Software-Binaries/releases/download/a301-27.0.0-prerelease.12/a301_27_0_0_prerelease_12.dfu)
- [A301 v2027.0.0-prerelease.11](https://github.com/REVrobotics/REV-Software-Binaries/releases/download/a301-27.0.0-prerelease.11/a301_27_0_0_prerelease_11.dfu)

> [!NOTE]
> The firmware can also be updated from RHC2 on desktop, but a bridging device (SPARK Flex, PDH, etc) is **required** for this method. To get the latest version of A301 firmware in RHC2, enter the following code in the "Downloads" tab of RHC2: `a301-alpha`

<details>
<summary>A301 Firmware Changelog</summary>

#### 2027.0.0-prerelease.14

- Fixes sensor faults not detected in voltage modes
- Fixes false failed gearbox detection

#### 2027.0.0-prerelease.12

- Fixes status periods
- Improves voltage control
- Replaces output current on status0 with improved average motor current
- Improves applied output reporting

#### 2027.0.0-prerelease.11

Initial release for A301

</details>

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
