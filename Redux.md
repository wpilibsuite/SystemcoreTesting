# ReduxLib

The current version of ReduxLib for SystemCore is **2027.0.0-alpha-7p1.**

It supports WPILib alpha version **2027.0.0-alpha-7+**.

## Major changes

* This vendordep now uses Alerts.
* **New in alpha-7p1:** The `.sensors` namespace has been removed; changing `com.reduxrobotics.sensors.canandmag.Canandmag` -> `com.reduxrobotics.canandmag.Canandmag` will fix your import errors.

This is otherwise _mostly_ similar to v2026.1.2, with some changes to use Alerts instead.
For more details, also see our [releases page](https://github.com/Redux-Robotics/canandrepo-public/releases)

## Install

### JSON

```txt
https://frcsdk.reduxrobotics.com/ReduxLib_2027.json
```

### Offline install zip

Extract this into the root of your `wpilib/2027_alpha7` directory.

This is typically in:

* `~/.local/share/wpilib/2027_alpha7` on Linux
* `~/.wpilib/2027_alpha7` on Mac
* `C:\Users\Public\wpilib\2027_alpha7` on Windows

[ReduxLib-offline-v2027.0.0-alpha-7p1.zip](https://frcsdk.reduxrobotics.com/offline/ReduxLib-offline-v2027.0.0-alpha-7p1.zip)

## CAN ID configuration

Just like v2026, the ReduxLib configurator will be available at `http://[insert SystemCore address here]:7244/`, 
e.g. [http://robot.local:7244](http://robot.local:7244).

This requires ReduxLib installed in a running (and not continuously crashing) robot project.

It should function similar to how it did in v2026. however **it currently does not automatically open existing CAN buses.**

## Usage

By default, if no bus is specified, devices will use bus 0 on the SystemCore or **`can_s0`**.

You can also pass in a `CANPort` for other on-board buses:

```java
// use SystemCore bus 4 instead
Canandgyro gyro = new Canandgyro(0, CANPort.CAN_S4);
```

Or, you can use a bus specifier string directly:

```java
// MotionCore port 2
Canandgyro gyro = new Canandgyro(0, "socketcan:can_d2");
```

## Known issues

### Can't open Configurator

Make sure your code isn't crashlooping first.

We're working on an IPK standalone build.

### Can't see buses in Configurator

You will need to declare a Redux device in your robot code running on the desired bus(es) to configure.
Having a valid ID doesn't matter; e.g. to configure Systemcore bus 1, one could add to their robot code:

```
// Whether or not you actually have a Canandgyro of id 42 on CAN_S1 doesn't matter. All that matters is the bus.
Canandgyro gyro = new Canandgyro(42, CANPort.CAN_S1);
```

The ability to manually open buses to configure them will be added in later updates, as well as a standalone IPK version of the configurator.

### Desktop sim crashes due to socketcan bus backends being unsupported on Windows/Mac

Will be fixed in a future release.
AdvantageKit users should probably be defining their simulation IO classes without specific hardware, however.

### Some internal alerts render weirdly because of `:` in the bus name

Will be fixed in a future release.

## Changelog

### ReduxLib v2027.0.0-alpha-7p1

### Fixed
* [Java] Fixed initialization order issues causing `NullPointerException`s on startup

#### Changed
* [Java/C++] Removed the intermediate `sensors` namespace

### ReduxLib v2027.0.0-alpha-7

#### Added

Windows arm64 desktop sim builds are now supported.

#### Changed

**You may need to instantiate a device class manually on a given bus to use it in Configurator.**

E.g. to set the CAN device ID of a Canandgyro on bus 3, you will likely have to run your robot program with:
```java
// I would just set the device ID to your intended value here; all that matters is that the CANPort is specified.
Canandgyro gyro = new Canandgyro(20, CANPort.CAN_S3);
```

Future releases will add the ability to manually open a bus.

* [Java/C++] The CAN bus ID now takes a `CANPort` instead of a raw integer index.
* [C++] Headers are now universally `.hpp`. ReduxFIFO headers remain `.h` as they expose a pure C ABI.
* [ReduxFIFO] `socketcan.fd` backend has been dropped in favor of dynamically figuring out if the bus is a CAN-FD bus.

#### Bug fixes

* [Configurator] Made the bus sorting more consistent
* [Java] Add `MessageBus.hashCode` to match `MessageBus.equals`
* [Java] Apply warning text to `CooldownWarning`
* [Java] Fix a bunch of incorrect doc comments all over the codebase
* [Java] Disabling presence alerts on a device via CanandEventLoop now also silences the Alert
* [Java/Canandcolor] Fix DigoutChain length 
* [Java/Canandcolor] Fix hsvHue calcuations
* [Java/Canandgyro] Fix setting the quaternion directly
* [Java] Fix `CanandcolorStatus.fromByteArray` via the power of delegation to known good impls
* [Java] Fix `CanandSettingsManager.getSettingSubset` timeout usage
* [Java] Fix `SettingResult.toByteArray` shifts
* [Java/CanandSettings] Reject NaN/infinite float inputs
* [Java/CanandSettings] Make boolean fetches return Optional.empty() instead of null
* [C++] Fix DigoutChain constructor
* [C++] Fix Canandgyro acceleration readings

#### ReduxLib v2027.0.0-alpha-6

- [General] Support WPILib v2027.0.0-alpha-6
- [General] Rebase everything on v2026.1.2

#### ReduxLib v2027.0.0-alpha-2

- [General] Add support for WPILib v2027.0.0-alpha-2
- [General] Add rewritten driver backend
- [Canandgyro] `setYaw` and `setPose` now retry up to 5 times by default.
