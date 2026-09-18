# CTR-Electronics Phoenix 6

Please read through the requirements and basic example before utilizing the vendordep.

## Requirements

> [!IMPORTANT]
> During alpha, there are frequent breaking changes between releases. Please carefully review the [changelog](#download) between each release.

Due to the volatile nature of versions and breaking changes, the below list is provided to clarify compatible WPILib version, compatible Phoenix 6 API version and compatible firmware version.

### WPILib to Phoenix 6 API

- WPILib `2027_alpha7` compatible releases
  - Phoenix 6 `26.70.0-alpha-2`
- WPILib `2027_alpha5` compatible releases
  - Phoenix 6 `26.50.0-alpha-1`
- WPILib `2027_alpha1` and `2027_alpha2` compatible releases
  - Phoenix 6 `25.90.0-alpha-1`
  - Phoenix 6 `25.90.0-alpha-2`

### Phoenix 6 API to Firmware

- `26.70.0-alpha-2` compatible Phoenix 6 firmware
  - Firmware `26.70.X`
- `26.50.0-alpha-1` compatible Phoenix 6 firmware
  - Firmware `26.X` (any 2026 firmware release)
- `25.90.0-alpha-1` and `25.90.0-alpha-2` compatible Phoenix 6 firmware
  - Firmware `25.90.0.0`

## Python Usage

It is highly recommended to explicitly pin the Python dependency in your `project.toml`.

```toml
requires = [
    "phoenix6==26.70.0a2"
]
```

## Download

* Vendordep: Select from the vendor JSON repository in VS code

* canivore-usb Package:
  * WPILib `2027_alpha7`: [canivore-usb_1.18_systemcore_wpilib_alpha_7](https://ctre.download/files/systemcore/canivore-usb_1.18_systemcore_wpilib_alpha_7.ipk)
    * Starting in this release, only one IPK is necessary for the CANivore
  * WPILib `2027_alpha5`: [canivore-usb-kernel_1.18_aarch](https://ctre.download/files/systemcore/canivore-usb-kernel_1.18_aarch64.ipk) and [canivore-usb_1.16_aarch64](https://ctre.download/files/systemcore/canivore-usb_1.16_aarch64.ipk)
    * Install canivore-usb-kernel, then canivore-usb

## Basic Example

SystemCore CAN buses can be used by using the `CANBus(CANPort port)` constructor. An example is shown below.

```java
// Use the CANivore named "swag"
private final TalonFX m_motor = new TalonFX(0, new CANBus("swag"));

// Use the Systemcore S2 port
private final TalonFX m_motor = new TalonFX(0, new CANBus(CANPort.CAN_S2));
```

## Examples

The Phoenix6-Examples repo has branches for the following releases:

* `26.70.0-alpha-2`: [2027_alpha5](https://github.com/CrossTheRoadElec/Phoenix6-Examples/tree/2027_alpha7)
* `26.50.0-alpha-1`: [2027_alpha5](https://github.com/CrossTheRoadElec/Phoenix6-Examples/tree/2027_alpha5)

## Changelog

### 26.70.0-alpha-2

Changelog is available [here](https://api.ctr-electronics.com/changelog#20260918). Note that Tuner X has not yet been updated to support generating mechanisms with this release.

### 26.50.0-alpha-1

Changelog is available [here](https://api.ctr-electronics.com/changelog#20260612). Note that Tuner X has not yet been updated to support generating mechanisms with this release.

### 25.90.0-alpha-2

#### Changes in 25.90.0-alpha-2

- **BREAKING**: Removed the device overload that takes a string parameter. Construct a `CANBus` object instead. This should improve clarity and reduce confusion.

#### Fixes in 25.90.0-alpha-2

- Fixed Signal Logger auto-logging.
  - When auto logging is enabled, logging is started by any of the following (whichever occurs first):
    - The robot is enabled.
    - It has been at least 5 seconds since program startup (allowing for calls to `setPath`), and the Driver Station is connected to the robot.
  - After auto logging has started the log once, logging will not be automatically stopped or restarted by auto logging.

#### Known Issues in 25.90.0-alpha-2

Please see [known issues](https://api.ctr-electronics.com/changelog#known-issues-20260613).
<hr/>

### 25.90.0-alpha-1

- Devices no longer implement sendable as it has been removed from WPILib.

#### Known Issues in 25.90.0-alpha-1

- Phoenix 5 is unavailable.
- An offline installer is unavailable.
- Signal logger does not auto-start on FMS.
- Signal logger does not rename files to include the match name when connected to FMS.
- Tuner cannot deploy a temporary diagnostic server to the SystemCore. To use Phoenix Tuner X functionality, deploy a blank robot program with a Phoenix 6 device initialized. No other Tuner functionality is affected.
