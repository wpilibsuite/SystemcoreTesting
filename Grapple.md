# 2027 Grapple FRC Software

Grapple's software for 2027 has been overhauled to suit the SystemCore. Here's the major changes:
- **Torq** is the replacement for Grapple's device configuration software GrappleHook. It now runs directly on the SystemCore as an IPK package, with Desktop distributions availabhle for USB devices like the MitoCANdria.
- The vendor-dep is mostly API-compatible with 2026 but with a modernised backend that is now unified between Torq and our WPILib-compatible vendordep.

## Installation
**VendorDep** installation uses the new URL https://storage.googleapis.com/grapple-frc-maven/grapple-vendordep-2027.json 

**Torq** can be installed here: https://github.com/GrappleRobotics/Binaries/releases/tag/frc-tools-27.0.0

Use the `torq-systemcore-*.ipk` package for installation on the SystemCore via the SystemCore's WebUI. Desktop packages are included for most platforms for USB-based configuration.

## Migration Guide
Device APIs in the new `grapple-vendordep` remain compatible with 2026 `libgrapplefrc`. There is an additional constructor pattern you may use to select alternate CAN busses (default if not specified is `CAN_S0`).

```java
// Java
var lasercan_s0 = new LaserCan(1);  // CAN_S0
var lasercan_s1 = new LaserCan(CANPort.CAN_S1, 1);  // Other busses
```

```cpp
// C++
grpl::LaserCan lasercan_s0(1); // CAN_S0
grpl::LaserCan lasercan_s1(wpi::CANPort::CAN_S0, 1); // Other busses
```

Python support is coming soon.
