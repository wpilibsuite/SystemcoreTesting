Vision support is under active development. There may be bugs or unfinished features.

# LimelightLib 2 Vendordep

| Systemcore OS | WPILib | Vendordep URL |
|---|---|---|
| Alpha 14 / Beta 14 and newer | 2027 alpha 7 and newer | https://limelightvision.github.io/limelightlib-public/LimelightLib-alpha7.json |
| Alpha 13 / Beta 13 and older | 2027 alpha 5 and alpha 6 | https://limelightvision.github.io/limelightlib-public/LimelightLib-alpha5-6.json |

### API reference

https://limelightvision.github.io/limelightlib-public/docs/

# Limelight 2027.0 Beta Smart Camera Images

https://drive.google.com/drive/folders/1NwZB1yKtuCkDpp5WSTrInyImtRlClsnv?usp=sharing

On Systemcore, vision is built-in. Plug in a USB camera, navigate to the cameras tab, and you'll see a live stream tile. Click the tile to go to the Pipeline editor for that vision instance.

# LimelightLib 2 Concepts

### Cameras and vision instances.
A `Limelight` object talks to one camera / vision instance by its NetworkTables name. Limelight Smart Cameras default to `limelight`. Systemcore runs up to four USB vision instances, `limelightsc0` through `limelightsc3`, one per USB port, exposed as `Limelight.SYSTEMCORE_USB0` through `SYSTEMCORE_USB3`. Set the camera's position in robot space in the constructor.

```java
Limelight camera = new Limelight();                                // Limelight hardware, "limelight"
//Limelight camera = new Limelight("limelight");
//Limelight camera = new Limelight(Limelight.SYSTEMCORE_USB0);     // Systemcore USB, "limelightsc0"
//Limelight camera = new Limelight(Limelight.SYSTEMCORE_USB1);
//Limelight camera = new Limelight("limelight", new Pose3d(0.30, 0.0, 0.20, new Rotation3d())); // with camera pose in robot space
```

### Results envelope and camera health.
Every frame arrives as one MessagePack envelope, so every value from `getLatestResults()` comes from the same frame. `getStatus()` reports `OK`, `NO_DATA`, `STALE` (no frame within 0.25 s, adjustable with `withStaleFrameThreshold`), or `DECODE_ERROR`. `hasTarget()` is false when the camera is stale, so a disconnected camera never drives the robot with old data.

```java
Limelight camera = new Limelight()

// Each loop
if (camera.hasTarget()) {                   
    var results = camera.getLatestResults();
}
```

### Visual servoing.
Aim with `getTXDegrees()`, `getTYDegrees()`, and `getTargetAreaPercent()` (area metrics are now always 0-100 (percentage of image area) and never 0-1). The pipeline picks the primary target based on sorting parameters configured in the UI. `setPriorityTagIDOverride(id)` makes one AprilTag primary regardless of size or position, and `clearPriorityTagIDOverride()` hands selection back to the pipeline. Using getters like getTXDegrees() will internally call getLatestResults() and parse new msgpack envelopes only when there is new data.

```java
double turnKp = -0.02;
Limelight camera = new Limelight();

// Each loop
double forward = joystick.getLeftY();
double turn = joystick.getRightX();
bool aimingEnabled = joystick.getRightShoulderButton();//pseudocode
if(aimingEnabled && camera.hasTarget()){
     turn = turnKp * camera.getTXDegrees();
}
arcadeDrive(forward, turn);
```

### Coordinate systems.
All 3D data is right-handed NWU: X forward, Y left, Z up, in field, robot, camera, and target space (the target's own frame, X out of the tag face). Pose arrays are `[x, y, z, roll, pitch, yaw]` in meters and degrees. Accessors like `getCameraPose_TargetSpace()` return `Pose3d`. 2D targeting (tx, ty, txnc, tync) use the classic Limelight optical convention (cam looks down -Z, +x to the right of the image, +y to the top of the image).

```java
// NWU, right-handed: X forward, Y left, Z up. Meters and degrees.
Pose3d cameraPoseTargetSpace = camera.getCameraPose_TargetSpace(); // X out of the tag face
double distanceToTagMeters = cameraPoseTargetSpace.getTranslation().getNorm();
```

### Localization with MegaTag1 and MegaTag2.
MT1 and MT2 produce a robot pose in field space from visible AprilTags. MT1 solves position and heading from tag geometry alone, so it needs no robot input but is sensitive to single-tag ambiguity and distance. MT2 takes your robot heading and solves only for position, to produce ambiguity-free estimates even with one tag, but it requires you to publish robot yaw (in field space) every loop. Estimate types name the algorithm and origin: `MT1_WPIBLUE`, `MT1_WPIRED`, `MT2_WPIBLUE`, `MT2_WPIRED`.

```java
Pose3d cameraPoseRobotSpace = new Pose3d(0.30, 0.0, 0.20, new Rotation3d());
Limelight camera = new Limelight("limelight", cameraPoseRobotSpace);
// ^ Limelight instances now have default filtering and trust scaling configurations that serve as reasonable starting points.

// Each robot loop, drain the entire PoseEstimate Queue. See rejected poses in AdvantageScope/Elastic with automatic telemetry
for (var estimate : camera.readAcceptedPoseEstimates(Limelight.PoseEstimateType.MT1_WPIBLUE)) {
    poseEstimator.addVisionMeasurement(estimate.pose, estimate.timestampSeconds, estimate.stdDevs);
}
```

### Robot orientation.
`Limelight.setSharedRobotOrientation(yawDegrees)` publishes your pose estimator's heading (not the raw gyro) to the `limelightshared` table, and every camera reads it for MT2 estimates. A turret camera can use its own value: `setRobotOrientation(yaw, flush)` opts it out of the shared orientation, `setUseSharedOrientation(true)` opts it back in.

Publish robot yaw before reading the queue each robot loop.

```java
Pose3d cameraPoseRobotSpace = new Pose3d(0.30, 0.0, 0.20, new Rotation3d());
Limelight camera = new Limelight("limelight", cameraPoseRobotSpace);

// Each robot loop, drain the entire PoseEstimate Queue. See rejected poses in AdvantageScope/Elastic with automatic telemetry
Limelight.setSharedRobotOrientation(robotYawDegrees);
for (var estimate : camera.readAcceptedPoseEstimates(Limelight.PoseEstimateType.MT2_WPIBLUE)) {
    poseEstimator.addVisionMeasurement(estimate.pose, estimate.timestampSeconds, estimate.stdDevs);
}
```

### Camera pose in robot space
Comes from the web UI or from code. The `Pose3d` constructor or `setCameraPose_RobotSpaceOverride(pose, flush)` overrides the UI and can update every loop for a camera on an elevator or turret. `clearCameraPose_RobotSpaceOverride()` returns to the UI value. An all-zero pose is the clear value. The UI shows when code controls the pose.

```java
Pose3d cameraPoseRobotSpace = new Pose3d(0.30, 0.0, 0.20, new Rotation3d()); // 0.30 m forward, 0.20 m up, level
Limelight camera = new Limelight("limelight", cameraPoseRobotSpace);

// Camera on an elevator: update every loop
camera.setCameraPose_RobotSpaceOverride(new Pose3d(0.30, 0.0, 0.20 + elevatorHeightMeters, new Rotation3d()), false);

// Back to the web UI value
camera.clearCameraPose_RobotSpaceOverride();
```

### Reading pose estimates.

The camera queues up to 20 frames between loops. Drain the queue every loop. `readAcceptedPoseEstimates(type)` returns only estimates that passed every check. `readPoseEstimateQueue(type)` returns everything with rejection reasons. `readResultsQueue()` returns raw envelopes. Use one queue method per camera per frame. Each latency-compensated `PoseEstimate` carries `pose`, a `timestampSeconds`, fusion-ready `stdDevs` (see `PoseEstimateConfig`), tag count, distance, area, and `rejectionFlags`. `PoseEstimateConfig.describeRejection(flags)` prints reasons like `TAG_COUNT|AMBIGUITY`.

```java
// Each robot loop. Use one queue method per camera per frame.
// readAcceptedPoseEstimates (see Localization) is the fusion path. This one is for diagnostics.
for (var estimate : camera.readPoseEstimateQueue(Limelight.PoseEstimateType.MT2_WPIBLUE)) {
    System.out.println(estimate.pose + " " + Limelight.PoseEstimateConfig.describeRejection(estimate.rejectionFlags)); // e.g. TAG_COUNT|AMBIGUITY
}
```

### Pose Estimate Filtering and trust scaling
A `PoseEstimateConfig` decides what is accepted and how much it is trusted. One per algorithm (MegaTag1 vs MegaTag2), via `withPoseEstimateConfig_MT1` and `_MT2`. Every camera starts with `defaultMT1()` and `defaultMT2()`, reasonable starting points rather than tuned values. Gates: min tag count, max single-tag ambiguity, max single-tag distance, max average distance, min average area, field bounds with margin (0 disables a gate). Trust: XY standard deviation starts at a base, scales with distance to an exponent (1 linear, 0.5 square root, 0 off), divides by tag count to an exponent, then clamps. Heading defaults to `UNTRUSTED` so vision heading is ignored. Configs are copied on attach. The Configured example below sets every term.

This example configures every filter and standard-deviation scaling term.
MT1 scales XY uncertainty linearly with distance and clamps it to 0.05-2.0
meters. MT2 scales it by the square root of distance and clamps it to
0.0001-2.0 meters. Both divide by the square root of the fielded tag count.
Vision heading is not fused. The field bounds are for the 2026 welded field.
Tune the other thresholds on your robot.

```java
double untrusted = Limelight.PoseEstimateConfig.UNTRUSTED;
Limelight.PoseEstimateConfig mt1Config = Limelight.PoseEstimateConfig.defaultMT1() // starting with the safe default means you are not required to set every single configuration value
        .withMinTagCount(1)
        .withMaxSingleTagAmbiguity(0.7) // MT1 needs low-ambiguity perspectives
        .withMaxSingleTagDistance(4.0) // If we only see one tag, don't trust it unless we are at most 4m away from it
        .withMaxAvgTagDistance(6.0) // If we see multiple tags, max avg distance must be less than 6 meters.
        .withMinAvgTagArea(0.02) // 0-100, percentage of image area. This is .02%, not 2%.
        .withFieldBounds(16.541, 8.069) // Reject pose estimates that are out of bounds.
        .withFieldBoundsMargin(0.5) // Add .5m margin around field.
        .withStdDevXY(0.5, 0.05, 2.0) // .5 base, absolute min .05, absolute max 2.0. With scaling enabled, .5 at 1m distance
        .withStdDevTheta(untrusted, untrusted, untrusted) // never incorporate pose estimate rotation. You may want to incorporate rotation by setting these to other values
        .withStdDevDistanceScaling(1.0, 0.0, 6.0) // linear scaling, only scale between 0 and 6m.
        .withStdDevTagCountDivision(0.5); // Enhance trust by square root of number of contributing tags

Limelight.PoseEstimateConfig mt2Config = Limelight.PoseEstimateConfig.defaultMT2()
        .withMinTagCount(1)
        .withMaxSingleTagAmbiguity(1.0) // MT2 can handle maximally ambiguous perspectives. Accept tags regardless of ambiguity value.
        .withMaxSingleTagDistance(0.0) // 0 disables this check
        .withMaxAvgTagDistance(8.0)
        .withMinAvgTagArea(0.02) //0-100, percentage of image area
        .withFieldBounds(16.541, 8.069)
        .withFieldBoundsMargin(0.5)
        .withStdDevXY(0.3, 0.0001, 2.0)
        .withStdDevTheta(untrusted, untrusted, untrusted)
        .withStdDevDistanceScaling(0.5, 0.0, 8.0) // Less aggressive STDDev scaling for MT2. Scale by sqrt(distance) rather than distance^1.
        .withStdDevTagCountDivision(0.5); // Enhance trust by a factor equal to the square root of number of contributing tags

Pose3d cameraPoseRobotSpace = new Pose3d(0.30, 0.0, 0.20, new Rotation3d());
Limelight camera = new Limelight("limelight", cameraPoseRobotSpace)
        .withPoseEstimateConfig_MT1(mt1Config)
        .withPoseEstimateConfig_MT2(mt2Config)
        .withTelemetry(true); // Keep automatic telemetry enabled. All accepted and rejected poses will remain easy to visualize in popular dashboards
boolean useMegaTag2 = true;

// Each robot loop
Limelight.PoseEstimateType type = useMegaTag2 ? Limelight.PoseEstimateType.MT2_WPIBLUE : Limelight.PoseEstimateType.MT1_WPIBLUE;
if (useMegaTag2) {
    Limelight.setSharedRobotOrientation(robotYawDegrees); // Use your robot pose yaw here, not your raw IMU reading.
}
for (var estimate : camera.readAcceptedPoseEstimates(type)) {
    poseEstimator.addVisionMeasurement(estimate.pose, estimate.timestampSeconds, estimate.stdDevs);
}
```

### Telemetry
On by default and found at `limelight_telemetry/<camera>/<type>/` with pose estimate rejection reasons and counters. Every camera also contributes to the shared `limelight_telemetry/Field` Field2d. `withTelemetry(false)` turns it off.

```java
// Nothing to write. Watch limelight_telemetry/Field in AdvantageScope, Elastic, or Glass.
Limelight camera = new Limelight("limelight", cameraPoseRobotSpace).withTelemetry(false); // turn it off
```

### Pipeline configuration overrides.

Download a pipeline as a `.vpr` file, drop it in the deploy folder, and load it with `Limelight.PipelineConfiguration.fromDeployFolder("aiming")`. `setPipelineConfigurationOverride(config)` sends it to the camera, `setUsePipelineConfigurationOverride(true)` runs it instead of the selected on-camera pipeline. The ten pipelines are untouched, so you can switch freely. The UI shows a banner while the override runs. In the UI, you can make edits but these edits will not be saved. You will need to download the pipelien and overwrite the file in the deploy folder for any changes to persist.

Use this if you have multiple cameras. A good example of this is the 2026 world championship in which robots have four cameras, and need to work on three different fields (wooden practice field, real practice field, real field).

Before, teams had to manage several pipelines per camera. Now, you can configure a single pipeline per environemnt and make one code change to configure all cameras on your robot. The new Limelight camera constructors handle robot-space positioning, so you can easily share pipelines across cameras.

```java
// All file IO happens once.
Limelight.PipelineConfiguration practiceFieldDim = Limelight.PipelineConfiguration.fromDeployFolder("practice"); // src/main/deploy/practice.vpr
Limelight.PipelineConfiguration realFieldBright = Limelight.PipelineConfiguration.fromDeployFolder("real"); // src/main/deploy/real.vpr


bool onPracticeField = false;
if(onPracticeField)
{
    camera.setPipelineConfigurationOverride(practiceFieldDim);
}
else
{
    camera.setPipelineConfigurationOverride(realFieldBright);
}

camera.setUsePipelineConfigurationOverride(true);

```

### Field map overrides

Put the `.fmap` in the deploy folder and publish it once for every camera: `Limelight.setSharedMap(Limelight.FieldMap.fromDeployFolder("field"))`. All cameras localize against it instead of their own uploaded map. `Limelight.clearSharedMap()` returns them to their own maps. `isSharedMapActive()` reports per camera. Cap 256 KB.

```java
// Robot constructor, once for every camera
Limelight.setSharedMap(Limelight.FieldMap.fromDeployFolder("field")); // src/main/deploy/field.fmap

// Back to each camera's own uploaded map
Limelight.clearSharedMap();
```

### All Overrides (Methods in robot code that override UI configurations)

There is a single chain of authority. UI Pipeline settings are at the bottom, Pipeline Configuration Overrides (.vpr in deploy folder) override UI Pipeline Settings, and Individual Settings Overrides (EG setFiducialDownscalingOverride) override all.

Every override has a matching clear that hands control back to the pipeline or web UI.

| Name | Override | Clear |
|---|---|---|
| Priority tag ID | `setPriorityTagIDOverride(id)` | `clearPriorityTagIDOverride()` |
| Camera pose in robot space | `setCameraPose_RobotSpaceOverride(pose, flush)` | `clearCameraPose_RobotSpaceOverride()` |
| Pipeline configuration (`.vpr`) | `setPipelineConfigurationOverride(config)` then `setUsePipelineConfigurationOverride(true)` | `setUsePipelineConfigurationOverride(false)` |
| Shared field map (`.fmap`, all cameras) | `Limelight.setSharedMap(map)` | `Limelight.clearSharedMap()` |
| Fiducial ID filter | `setFiducialIDFiltersOverride(int[])` | `clearFiducialIDFiltersOverride()` |
| Fiducial 3D offset (target space) | `setFiducial3DOffsetOverride(Translation3d)` | `clearFiducial3DOffsetOverride()` |
| Crop window | `setCropWindowOverride(...)` | `clearCropWindowOverride()` |
| Keystone | `setKeystoneOverride(...)` | `clearKeystoneOverride()` |
| Fiducial downscaling | `setFiducialDownscalingOverride(DownscaleOverride.X2)` | `clearFiducialDownscalingOverride()` |

### Throttle
`setThrottle(n)` skips frames to cut heat while disabled.

```java
camera.setThrottle(100); // disabledInit: skip frames to cut heat
```

### Pipeline index
`setPipelineIndex(n)` selects one of the ten pipelines.

```java
camera.setPipelineIndex(2); // 0 through 9
```

### IMU modes
Limelight 4 has an IMU that can be used to improve MT2. `setIMUMode`: `EXTERNAL`, `EXTERNAL_SEED_INTERNAL`, `INTERNAL`, `INTERNAL_MT1_ASSIST`, `INTERNAL_EXTERNAL_ASSIST`. `setIMUAssistAlpha` sets convergence speed. Systemcore USB instances always use an "external" heading, even if using the Systemcore IMU. For Limelight 4, the recommended approach is to use Seed_Internal continuously, followed by Internal_External_Assist while localizing or aiming. Internal_External_Assist uses the built-in IMU for the best possible response times while always converging on the external reference such as the Systemcore IMU or Pigeon2 IMU. This means you can use the built-in IMU for fast responsse, and your main robot imu for your true absolute reference.

```java
camera.setIMUMode(Limelight.IMUMode.INTERNAL_MT1_ASSIST);
camera.setIMUAssistAlpha(0.001);
```

# Limelight 2027.0 Beta Migration Guide

Breaking changes TLDR:

1. All results are sent via a single MessagePack topic in NetworkTables. The original NT API can be enabled per-pipeline in the pipeline output tab.

2. A single robot-orientation update (posted to a new `limelightshared` NT table) can feed every camera using MegaTag2.

3. LimelightLib 2 introduces an object-oriented API with built-in pose filtering and trust scaling. Telemetry for accepted and rejected pose estimates is enabled by default, allowing those estimates and related statistics to be visualized in AdvantageScope, Elastic, and Glass. LimelightHelpers is replaced by the LimelightLib 2 vendordep.

4. Maps (`.fmap`) and pipeline configurations (`.vpr`) can be placed in the deploy folder of your robot project and used as overrides. This is the recommended way to run several cameras on one robot with configurations in source control.

5. 2027.0 unifies every 3D coordinate system into the **NWU, right-handed** (X forward, Y left, Z up) standard.

**3D Robot Space**: X forward, Y left, Z up.
**3D Camera Space**: X forward (boresight), Y left, Z up.
**3D Tag / target**: X out of the tag face, Y tag-left, Z up.

2D targeting features still use the Limelight optical space, in which the camera looks down -Z. This matches other 3D graphics conventions and the standard 2D Cartesian coordinate system (center image pixel is (0,0), +Y up, +X right).

This release still has the same three field localization systems: native centered, wpiblue, and wpired. Future releases will adapt to a new unified centered coordinate system. botpose, botpose_wpiblue, and botpose_wpired work as they did in 2026 releases.

6. Limelight 3G, 3A, and 4 now have OTA update capabilities. You can test OTA by first using the newest hardware manager to update to 2027.0. You can then OTA update (still to 2027.0) using the `.llupdate` files in the folder.

## Checklist

1. Update camera firmware to 2027.0 (Limelight hardware) or Systemcore OS (USB cameras), and replace LimelightHelpers with LimelightLib2
2. Re-enter camera mount side and pitch on every pipeline (flip both signs), or publish the camera pose from code with the `Pose3d` constructor.
3. Flip the third component of every POI (pipelines and old `.fmap` files).
4. Update any code using targetspace or cameraspace transforms to work with the new coordinate system.
5. If you want to use the classic NetworkTables keys, re-enable them in the Output tab (per pipeline).
6. Verify visually: the web UI's 3D views render exactly what NetworkTables publishes, so if the robot, camera, and tags look right in the visualizers, you're good to go.
7. Open AdvantageScope or Elastic and watch `limelight_telemetry/Field`. Accepted and rejected estimates from every camera appear there with no extra code.

# Systemcore USB Cameras

Systemcore supports up to four USB vision instances. Curated cameras get hardcoded workarounds for USB firmware issues, one or two video modes selected for FIRST robots, and a built-in default calibration. Non-curated cameras still work, auto-populate their controls, expose one automatically selected video mode, and show a red calibration banner until you upload a calibration. USB cameras default to auto exposure and auto white balance.

Cameras with default calibrations as of Systemcore OS Alpha 14 / Beta 14:

Global-shutter vision cameras
* Arducam OV9281 USB
* Arducam OV9782 / goBILDA Global Shutter
* Arducam OV2311 USB
* Innomaker OV9281 USB
* Waveshare OV9281 USB (not recommended due to poor optics, warning displayed in UI)
* ThriftyBot ThriftiestCam (no auto exposure for now)

Rolling-shutter vision cameras
* UC60 / goBILDA Rolling Shutter

Logitech webcams
* Logitech C270 (960p, Model A)
* Logitech C270 (720p, Model B) (no manual exposure, not recommended, warning displayed in UI)
* Logi C270 HD (720p, Model C)
* Logitech C310
* Logitech Brio 101
* Logitech Brio 100
* Logitech C930
* Logitech C920 Family
* Logitech C922 Pro Stream
* Logitech 1080P Pro Stream

Other
* NexiGo N60 FHD
* Microsoft LifeCam HD-3000
* Sony PS3 Eye (blue-dot lens)


Calibration notes
* "Default Calibration" means the built-in calibration for a curated camera is in use. Good to start with. Calibrate your own unit for the best 3D accuracy.
* A red banner means a generic calibration is in use, for example an unsupported USB camera. Calibrate before trusting any 3D result.
* Your uploaded calibration always wins over the default, and `isUsingCustomCalibration()` reports it in robot code.
* Calibration stays on the camera. It is not part of the code-driven overrides.

# 2027.0 Changelog

## Limelight OS 2027.0 (IN PROGRESS CHANGE LOG)

### Significantly Improved Update UX

1. Limelight OS can now be updated over the air (OTA) through the web UI with `.llupdate` files (LL3A, LL3G, LL4 only). No hardware manager, no USB, no flash mode. Power on, drop the update into the web UI, and you're done. You will need to use the very latest version of the hardware manager one time to get your camera on 2027.0 or later.
Complete pipelines, including python scripts, neural networks, and
apriltag maps, can now be downloaded/uploaded as `.llpipeline` files. All 10 pipelines can be downloaded/uploaded as a single `.llpipelinepack` file.
2. Recovery image files and the new `.llupdate` OTA update packages have been optimized for fast downloads and minimal disk utilization.
3. The filesystem now expands on first boot to provide around 4 additional gigabytes of storage for rewind recordings.
4. Every downloaded file now includes the camera name and hardware type name. This makes rewind and pipeline organization easier than before.
5. The hardware manager, while no longer necessary for most teams after 2027.0, is now cross-platform.

### LimelightLib 2

LimelightLib 2 introduces an object-oriented API with built-in pose filtering and trust scaling. Telemetry for accepted and rejected pose estimates is enabled by default, allowing those estimates and related statistics to be visualized in AdvantageScope, Elastic, and Glass. One library is published for two WPILib trains (alpha 5/6 and alpha 7). Pipeline configurations and field maps can be published from the robot project's deploy folder.

### IMU Reliability

Known IMU fault conditions have been eliminated.

### Unified 3D Coordinate Systems (BREAKING)

* Limelight now uses the NWU right-hand convention everywhere. It follows the right-hand
rule, with X forward, Y left, and Z up.
* Robotspace and Points of Interest now follow NWU as well, so pipelines and code
 need updated cameraPoseInRobotSpace side, pitch, and Point of Interest values.
* See the 2027.0 Migration Guide above.

### Atomic MessagePack Results (BREAKING)

* All results are published to NT as a single MessagePack topic. This guarantees atomicity for all results data from a camera.
* Results now include camera intrinsics and a `customcal` flag indicating whether
the camera is using a user calibration, and report whether a code-driven pipeline override or shared field map is active.
* The classic NetworkTables results API is disabled by default. It can be
enabled again per pipeline in the Output tab.

### Shared Robot Orientation

* All Limelights now read `robot_orientation_set` from the new
`limelightshared` table by default. A single robot orientation update can feed
every MegaTag2 camera.
* Robot code can opt individual Limelights out of the shared orientation and
publish directly to those cameras instead.

### Pipelien Configuration and Field Map Overrides

* Robot code can publish a pipeline configuration (`.vpr`) that overrides the selected pipeline, and a shared field map (`.fmap`) that every camera uses.
* The web UI shows a banner while an override is active. Edits stay live for tuning but are not saved. Download the configuration to keep them.

### Full Resolution Capture Endpoint

The new `/capture` HTTP endpoint returns the latest uncompressed frame as a PNG
at the camera's full resolution.

### MAVLink Output

Limelight vision instances can now publish MAVLink for various drone platforms.

### WebRTC Updates

* WebRTC retransmission capabilities are now disabled.
* Focus pipelines now use an H.264 profile that prioritizes higher sharpness and clarity while HQ
Mode is enabled. The stream runs at native resolution at approximately 5 FPS
with a 2.5 Mbps cap.

### AprilTag and Pipeline Updates

* AprilTag pipelines now default to 3D mode.
* Target area is consistently reported on a 0 to 100 scale.
* The UI indicates when Camera Pose in Robot Space or fiducial downscaling is
  controlled by robot code. Those values are shown directly and the
  corresponding controls are locked.

### Calibration and Distortion Diagnostics

Improved distortion visual in the web UI. Default calibrations for curated USB cameras, with a clear indication of whether a default, custom, or generic calibration is in use.

### Live Diagnostics Search

The web UI now includes a search bar within the live JSON results view.

### Systemcore Support

2027.0 runs on Systemcore and supports up to four USB vision instances. Twenty cameras are curated with default calibrations and hardcoded workarounds for USB firmware issues. See the Systemcore USB Cameras section above for the list and calibration notes.
