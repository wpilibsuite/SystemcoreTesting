# YASS (Yet Another Software Suite)

Vendor libraries from the Yet Another Software Suite for 2027 WPILib and Systemcore.

The vendordeps and libraries are downloaded from `cdn.yassrobotics.com`, including when you build, so YAMS and YAGSL also work on school networks that block `github.io`. Vendordeps installed from the old `yet-another-software-suite.github.io` URLs keep working, but reinstalling from the URLs below switches them over.

- [YAMS (Yet Another Mechanism System)](#yams-yet-another-mechanism-system)
- [YAGSL (Yet Another Generic Swerve Library)](#yagsl-yet-another-generic-swerve-library)

## YAMS (Yet Another Mechanism System)

Mechanism simulation and control framework: arms, elevators, pivots, flywheels, differential mechanisms, double jointed arms and swerve drives, with physics simulation and telemetry built in.

Website: https://yams.yassrobotics.com/

### Vendordep URL

YAMS ships one vendordep per command framework. Install the one that matches your robot code, not both.

Commands v2 (`org.wpilib.command2`, Java and C++):

```
https://cdn.yassrobotics.com/yams_commands2.json
```

Commands v3 (`org.wpilib.command3`, Java only):

```
https://cdn.yassrobotics.com/yams_commands3.json
```

These always point at the latest release. The latest release for WPILib v2027.0.0-alpha-7 is [v2026.10.09](https://github.com/Yet-Another-Software-Suite/YAMS/releases/tag/v2026.10.09); to pin it, use [yams_commands2-2026.10.09.json](https://cdn.yassrobotics.com/yams_commands2-2026.10.09.json) or [yams_commands3-2026.10.09.json](https://cdn.yassrobotics.com/yams_commands3-2026.10.09.json).

Example robot projects for both frameworks are in [examples/commands2](https://github.com/Yet-Another-Software-Suite/YAMS/tree/master/examples/commands2) and [examples/commands3](https://github.com/Yet-Another-Software-Suite/YAMS/tree/master/examples/commands3).

### Thrifty Nova

YAMS v2026.10.09 supports The Thrifty Bot's Thrifty Nova motor controller through `NovaWrapper` (`yams.core.motorcontrollers.local.NovaWrapper` in Java, `yams::motorcontrollers::local::NovaWrapper` in C++), built on ThriftyLib 2027.0.0-alpha-5. To use Novas, also install the ThriftyLib vendordep:

```
https://software.thethriftybot.com/frcvendor/ThriftyLib-2027.json
```

```java
SmartMotorControllerConfig config = new SmartMotorControllerConfig(this)
    .withClosedLoopController(0.2, 0, 0)
    .withStatorCurrentLimit(Amps.of(40));
// A Nova driving a NEO on CAN bus 0, CAN ID 3
SmartMotorController motor = new NovaWrapper(new Nova(0, 3, MotorType.NEO), DCMotor.getNEO(1), config);
```

The closed loop controller runs on the Systemcore, so the Nova behaves the same in simulation and on the robot. For an external encoder, pass `withExternalEncoder(...)` an absolute (`FeedbackSensorType.ABS`) or quadrature (`FeedbackSensorType.QUAD`) encoder wired to the Nova's data port, or a Thrifty CAN Encoder (`CanEncoder`).

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

## YAGSL (Yet Another Generic Swerve Library)

Plug-and-play swerve drive library. YAGSL reads your swerve JSON configuration files and builds a YAMS `SwerveDrive` with the motors, encoders and gyro already configured.

Website: https://docs.yagsl.com/

### Configuration generator

Use **[config.yagsl.com](http://config.yagsl.com)** to generate your swerve JSON configuration files. Fill in your gyro, motors, absolute encoders, gear ratios and module locations, then download the ZIP and unzip it into `src/main/deploy` so the files end up in `src/main/deploy/swerve/base`. The site also has a **Quick Start guide** that walks you through setting up a new project with YAGSL.

The old configuration generator for 2026 YAGSL is still available at **[config.yagsl.com/old](http://config.yagsl.com/old/)**. It produces the 2026 JSON format, which 2027 YAGSL cannot read, so only use it for 2026 projects.

### Vendordep URL

Like YAMS, YAGSL ships one vendordep per command framework. Install the one that matches your robot code, not both. The 2026 `yagsl.json` vendordep is replaced by these two.

Commands v2 (`org.wpilib.command2`, Java only):

```
https://cdn.yassrobotics.com/yagsl_commands2.json
```

Commands v3 (`org.wpilib.command3`, Java only):

```
https://cdn.yassrobotics.com/yagsl_commands3.json
```

These always point at the latest release. The latest release for WPILib v2027.0.0-alpha-7 is [v2026.10.09](https://github.com/Yet-Another-Software-Suite/YAGSL/releases/tag/v2026.10.09); to pin it, use [yagsl_commands2-2026.10.09.json](https://cdn.yassrobotics.com/yagsl_commands2-2026.10.09.json) or [yagsl_commands3-2026.10.09.json](https://cdn.yassrobotics.com/yagsl_commands3-2026.10.09.json).

Each YAGSL vendordep only requires the matching YAMS vendordep (`yams_commands2.json` or `yams_commands3.json`). Every other vendor library is optional: install REVLib, Phoenix6, ReduxLib or ThriftyLib only if your configuration uses that vendor's devices. YAGSL loads a vendor's devices only when your JSON configuration asks for them. YAGSL v2026.10.09 uses YAMS v2026.10.09.

StudicaLib and AmLib devices are not supported yet in the 2027 alpha, since those vendors have not published 2027_alpha7 vendordeps. Example robot projects for both frameworks are in [examples/commands2](https://github.com/Yet-Another-Software-Suite/YAGSL/tree/main/examples/commands2) and [examples/commands3](https://github.com/Yet-Another-Software-Suite/YAGSL/tree/main/examples/commands3).

### Devices in the JSON configuration

Every device in the configuration (each module's `drive` and `angle` motors and `absoluteEncoder`, and the `gyro`) uses the same fields:

```json
{ "type": "nova_neo", "id": 3, "canbus": "1" }
```

- **`type`**: for motors, the motor controller then the motor, such as `talonfx_krakenx60`, `sparkmax_neo` or `nova_neo`. For absolute encoders and gyros, the device then how it is connected: `can`, `attached` (wired to the angle motor controller), `dio` or `analog` (a Systemcore SmartIO port), or `internal`. For example `cancoder_can`, `revthroughbore_attached`, `revthroughbore_dio` or `pigeon2_can`.
- **`id`**: the CAN ID of a CAN device.
- **`channel`**: the SmartIO port of a `dio` or `analog` encoder.
- **`canbus`**: the Systemcore CAN bus the device is on, the same way for every vendor:

| `canbus` | CAN bus |
| --- | --- |
| `""` (empty) | `can_s0` |
| `"1"`, `"2"`, `"3"`, `"4"` | `can_s1`, `can_s2`, `can_s3`, `can_s4` |

CTRE devices (`talonfx`, `talonfxs`, `cancoder`, `pigeon2`) can also be on a CANivore: set `canbus` to the CANivore's name. Every other vendor only takes a bus number, and [config.yagsl.com](http://config.yagsl.com) flags anything else.

CTRE devices with an empty `canbus` are on `can_s0` like every other vendor, even though Phoenix 6 on its own defaults to `can_s2`. If a configuration names a Systemcore CAN bus, such as `"can_s1"` or `"socketcan:can_s1"`, change it to the bus number, `"1"`.

**Thrifty Nova:** supported again through the YAMS `NovaWrapper`; install the [ThriftyLib vendordep](https://software.thethriftybot.com/frcvendor/ThriftyLib-2027.json) to use it. A Nova is `nova_` followed by its motor: `nova_neo`, `nova_neo2`, `nova_neo550`, `nova_vortex`, `nova_minion` or `nova_pulsar`. An absolute encoder attached to the Nova's data port (for example `revthroughbore_attached`) is used as the module's external feedback encoder, and attached analog encoders work too.

**Systemcore IMU:** the Systemcore's built in IMU can be the gyro, with the type `systemcore_internal`. It is part of WPILib, so it needs no vendordep and no `id` or `canbus`.

### Migrating from 2026 YAGSL

These notes compare against YAGSL 2026.4.1.

#### YAGSL is now built on YAMS

2026 YAGSL had its own `SwerveDrive`, `SwerveInputStream`, `SwerveController`, `SwerveMath`, `SwerveDriveTest` and telemetry classes. In 2027, YAGSL only parses your JSON configuration files and builds a YAMS `SwerveDrive`. Driving, odometry, input streams, controllers and telemetry all come from YAMS, so the old `swervelib.*` drive classes are gone.

The Java API is split into packages the same way YAMS is:

- **`swervelib.core`**: the JSON parser, JSON models and telemetry. It does not depend on any command framework.
- **`swervelib.commands2`**: the `SwerveParser` that robot code should use with Commands v2. It returns a `yams.commands2.swerve.SwerveDrive`.
- **`swervelib.commands3`**: the `SwerveParser` for Commands v3. It returns a `yams.commands3.swerve.SwerveDrive`.

#### New imports

```java
// Before (2026.4.1)
import swervelib.SwerveDrive;
import swervelib.SwerveInputStream;
import swervelib.parser.SwerveParser;
import swervelib.telemetry.SwerveDriveTelemetry;
import swervelib.telemetry.SwerveDriveTelemetry.TelemetryVerbosity;

// After (Commands v2; use swervelib.commands3 and yams.commands3 for Commands v3)
import swervelib.commands2.SwerveParser;
import yams.commands2.config.SwerveDriveConfig;
import yams.commands2.swerve.SwerveDrive;
import yams.commands2.swerve.SwerveInputStream;
import yams.core.telemetry.SwerveDriveTelemetryConfig;
import yams.core.telemetry.enums.TelemetryVerbosity;
```

| 2026.4.1 | 2027 |
| --- | --- |
| `swervelib.parser.SwerveParser` | `swervelib.commands2.SwerveParser` / `swervelib.commands3.SwerveParser` |
| `swervelib.SwerveDrive` | `yams.commands2.swerve.SwerveDrive` / `yams.commands3.swerve.SwerveDrive` |
| `swervelib.SwerveInputStream` | `yams.commands2.swerve.SwerveInputStream` / `yams.commands3.swerve.SwerveInputStream` |
| `swervelib.parser.SwerveDriveConfiguration`, `SwerveControllerConfiguration` | `yams.commands2.config.SwerveDriveConfig` / `yams.commands3.config.SwerveDriveConfig` |
| `swervelib.telemetry.SwerveDriveTelemetry.TelemetryVerbosity` | `yams.core.telemetry.enums.TelemetryVerbosity` |
| `swervelib.telemetry.SwerveDriveTelemetry` | `yams.core.telemetry.SwerveDriveTelemetryConfig`, passed to `SwerveDriveConfig.withTelemetry(...)` |
| `swervelib.SwerveModule` | `yams.core.mechanisms.swerve.SwerveModule` |
| `swervelib.parser.*` JSON models | `swervelib.core.parser.*` |

#### Creating the `SwerveDrive`

`SwerveParser` is no longer constructed per directory. Call the static `parse(...)` once, then build the drive from a YAMS `SwerveDriveConfig`. Settings that used to be constructor arguments or calls on the drive (starting pose, telemetry, heading controller) now go on the config.

```java
// Before (2026.4.1)
SwerveDriveTelemetry.verbosity = TelemetryVerbosity.HIGH;
swerveDrive = new SwerveParser(new File(Filesystem.getDeployDirectory(), "swerve"))
    .createSwerveDrive(Constants.MAX_SPEED, startingPose);

// After
var cfg = new SwerveDriveConfig()
    .withStartingPose(new Pose2d(3, 3, Rotation2d.ZERO))
    .withSubsystem(this)
    .withTranslationController(new PIDController(4, 0, 0))
    .withRotationController(new PIDController(3, 0, 0))
    .withTelemetry("swerve", new SwerveDriveTelemetryConfig(TelemetryVerbosity.HIGH));

SwerveParser.parse(new File(Filesystem.getDeployDirectory(), "swerve/base"));
SwerveDrive drive = SwerveParser.createSwerveDrive(cfg);
```

To also get the raw vendor devices (motor controllers, encoders, gyro), use `SwerveParser.createSwerveDriveDevices(cfg)`, which returns a `SwerveDriveDevices<SwerveDrive>` (`swervelib.core.parser.SwerveParser.SwerveDriveDevices`).

#### `SwerveInputStream` uses `with...` methods

The input stream now comes from YAMS and its options are all `with...` methods:

```java
// Before (2026.4.1)
SwerveInputStream stream = SwerveInputStream.of(drivebase.getSwerveDrive(), () -> -driverXbox.getLeftY(), () -> -driverXbox.getLeftX())
    .withControllerRotationAxis(driverXbox::getRightX)
    .deadband(0.05)
    .scaleTranslation(0.8)
    .allianceRelativeControl(true);
drivebase.setDefaultCommand(drivebase.driveFieldOriented(stream));

// After
SwerveInputStream stream = SwerveInputStream.of(drive, () -> -driverXbox.getLeftY(), () -> -driverXbox.getLeftX(), () -> -driverXbox.getRightX())
    .withDeadband(0.05)
    .withScaleTranslation(0.8)
    .withAllianceRelativeControl();
swerve.setDefaultCommand(swerve.run(() -> drive.setFieldRelativeChassisSpeeds(stream.get())));
```

`headingWhile(...)` is now `withHeadingControl(...)`, and `robotRelative(true)` is now `withRobotRelative()`.

#### Regenerate your JSON configuration

The JSON format changed (for example `imu` is now `gyro`, `encoder` is now `absoluteEncoder`, motor types name the motor like `talonfx_krakenx60`, and `controllerproperties.json` is no longer used). Rebuild your configuration with [config.yagsl.com](http://config.yagsl.com) rather than editing the 2026 files by hand. If you need to look at or edit your 2026 configuration, use the old generator at [config.yagsl.com/old](http://config.yagsl.com/old/).
