# YASS (Yet Another Software Suite)

Vendor libraries from the Yet Another Software Suite for 2027 WPILib and Systemcore.

- [YAMS (Yet Another Mechanism System)](#yams-yet-another-mechanism-system)

## YAMS (Yet Another Mechanism System)

Mechanism simulation and control framework: arms, elevators, pivots, flywheels, differential mechanisms, double jointed arms and swerve drives, with physics simulation and telemetry built in.

Website: https://yams.yassrobotics.com/

### Vendordep URL

YAMS ships one vendordep per command framework. Install the one that matches your robot code, not both.

Commands v2 (`org.wpilib.command2`, Java and C++):

```
https://yet-another-software-suite.github.io/YAMS/yams_commands2.json
```

Commands v3 (`org.wpilib.command3`, Java only):

```
https://yet-another-software-suite.github.io/YAMS/yams_commands3.json
```

v2026.10.03 is the first release built for WPILib v2027.0.0-alpha-7. Example robot projects for both frameworks are in [examples/commands2](https://github.com/Yet-Another-Software-Suite/YAMS/tree/master/examples/commands2) and [examples/commands3](https://github.com/Yet-Another-Software-Suite/YAMS/tree/master/examples/commands3).

### Migrating from 2026 YAMS

These are the highlights; see [MIGRATION.md](https://github.com/Yet-Another-Software-Suite/YAMS/blob/master/MIGRATION.md) for the full guide.

#### The Java API is split into packages

- **`yams.core`**: mechanism and motor controller state, physics simulation, and telemetry. It does not depend on any command framework and is not meant to be used directly by robot code.
- **`yams.commands2`**: the classes robot code should use with Commands v2. This is where the `Command` and `Trigger` factories live (`setAngle()`, `runTo()`, `near()`, `max()`, and so on).
- **`yams.commands3`**: the same layout for Commands v3. `SmartMotorControllerConfig` takes a `Mechanism` instead of a `Subsystem`, and the factories return Commands v3 `Command`s and `Trigger`s.

#### Most code only needs new imports

Method names, constructor argument order and config builder methods are unchanged.

```java
// Before
import yams.motorcontrollers.SmartMotorControllerConfig;
import yams.mechanisms.positional.Arm;

// After (Commands v2; use yams.commands3 for Commands v3)
import yams.commands2.config.SmartMotorControllerConfig;
import yams.commands2.mechanisms.Arm;
```

| Old package | New package for robot code |
| --- | --- |
| `yams.mechanisms.positional.Arm`, `Elevator`, `Pivot`, `DifferentialMechanism`, `DoubleJointedArm` | `yams.commands2.mechanisms.*` |
| `yams.mechanisms.velocity.FlyWheel` | `yams.commands2.mechanisms.FlyWheel` |
| `yams.mechanisms.swerve.SwerveDrive` | `yams.commands2.swerve.SwerveDrive` |
| `yams.motorcontrollers.SmartMotorControllerConfig` | `yams.commands2.config.SmartMotorControllerConfig` |
| `yams.mechanisms.config.SwerveDriveConfig` | `yams.commands2.config.SwerveDriveConfig` |
| `yams.mechanisms.config.ArmConfig`, `ElevatorConfig`, `PivotConfig`, `FlyWheelConfig`, etc. | `yams.core.mechanisms.config.*` |
| Motor controller wrappers (`SparkWrapper`, `TalonFXWrapper`, ...) | `yams.core.motorcontrollers.*` |
| `yams.exceptions`, `gearing`, `math`, `units`, `telemetry` | `yams.core.*` |

#### `isNear(...)` Trigger renamed to `near(...)`

This matches the other Trigger factories (`gte()`, `lte()`, `between()`, `max()`, `min()`). The boolean check `isNear(...)` keeps its name.

```java
// Before
arm.isNear(Degrees.of(80), Degrees.of(2)).onTrue(indexer.run());

// After
arm.near(Degrees.of(80), Degrees.of(2)).onTrue(indexer.run());
```

#### Idle mode renamed to zero power

`withIdleMode(...)`, `getIdleMode()` and `setIdleMode(...)` are now `withZeroPower(...)`, `getZeroPower()` and `setZeroPower(...)`. `MotorMode` also moved out of `SmartMotorControllerConfig` into `yams.core.motorcontrollers.enums.MotorMode`.

```java
// Before
import yams.motorcontrollers.SmartMotorControllerConfig.MotorMode;
config.withIdleMode(MotorMode.BRAKE);

// After
import yams.core.motorcontrollers.enums.MotorMode;
config.withZeroPower(MotorMode.BRAKE);
```

#### Live Tuning is opt-in

The "Live Tuning" command is registered automatically only when the config is bound to a Subsystem and telemetry verbosity is `HIGH`. Otherwise, call `setupLiveTuning()`:

```java
SmartMotorControllerConfig motorConfig = new SmartMotorControllerConfig(this)
    .withTelemetry("ArmMotor", TelemetryVerbosity.HIGH);
motorConfig.setupLiveTuning();
```
