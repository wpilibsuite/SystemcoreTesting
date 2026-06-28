# Systemcore, Motioncore & A301 testing

This repository is for Alpha and Beta testing of the Systemcore, Motioncore, and A301.

Note that all updated documentation for 2027 WPILib changes, new features, etc. can be found on the '2027' version of the WPILib Docs site: https://docs.wpilib.org/en/2027/

> [!IMPORTANT]
> Software posted here is Alpha software, and is not compatible with the Control Hub or roboRIO.

[Systemcore Specification PDF](https://downloads.limelightvision.io/documents/systemcore_specifications_june15_2025_alpha.pdf)

[Systemcore Cables PDF](https://downloads.limelightvision.io/documents/systemcore_motioncore_cable_specifications.pdf)

[A301 Details](A301.md)

## Alpha vs Beta Systemcore units

Systemcore units originally shipped to teams during the initial FRC Alpha test are Alpha Systemcore units.  Units shipped to FTC Alpha testing teams are Beta Systemcore units.

| Alpha Systemcore | Beta Systemcore |
|---|---|
| ![SystemcoreTopHousing](https://ik.imagekit.io/llimi/controlsystem/tophousingcrop) | <img width="800" alt="image" src="https://github.com/user-attachments/assets/82149596-f0b2-470d-be5e-3c5dd2cd9cc0" /> |

> [!NOTE]
> Alpha units do not have port labels:
> ![SystemcoreUnboxing](https://ik.imagekit.io/llimi/controlsystem/scunboxing.png)

## Alpha/Beta Software Directory

### Tooling

- [2027 Alpha 6 WPILib Installer](https://github.com/wpilibsuite/allwpilib/releases/tag/v2027.0.0-alpha-6)
- [2027 Driver Station](https://github.com/wpilibsuite/FirstDriverStation-Public)
- [OS Releases, Toolchain, Package Examples, Cross-Compilation Examples](https://github.com/LimelightVision/systemcore-os-public)
- Limelight Hardware Manager 2.0.7 - [Windows](https://downloads.limelightvision.io/software/LimelightHardwareManagerSetup2_0_7.exe) | [macOS (Apple Silicon)](https://downloads.limelightvision.io/software/LimelightHardwareManager-macOS-AppleSilicon2_0_7.dmg) | [macOS (Intel)](https://downloads.limelightvision.io/software/LimelightHardwareManager-macOS-Intel2_0_7.dmg)

> [!TIP]
> The year and version on the desktop icons and WPILib folder might not match the WPILib version from the installer. As of this writing, the latest WPILib is 2027.0.0-alpha-6, which installs to a 2027_alpha5 folder and shortcuts. The reason for this is complicated, but it is intended behavior. Alpha 6 will install straight over an Alpha 5 installation. In season, this will not be an issue, as the folder will just be the year by itself, this is purely an Alpha and Beta issue.

### Vendor Libraries

* [CTR Electronics Phoenix 6](CTR-Phoenix.md)
* [REV Robotics](REV.md)
* [AdvantageKit](AdvantageKit.md)
* [ChoreoLib](ChoreoLib.md)
* [PathPlannerLib](PathPlannerLib.md)

### Other

* [AdvantageScope 2027 Alpha](AdvantageScope.md)
* [Elastic 2027 Alpha](Elastic.md)

### Software Compatibility

| Systemcore Alpha/Beta Image Version | WPILib | NI Driver Station | 2027 Driver Station |
|---:|---|---|---|
| <= 9 | v2027.0.0-alpha-1 or v2027.0.0-alpha-2 | Compatible | Not compatible |
| >= 10 | >= v2027.0.0-alpha-5 | Compatible\* | Compatible |

\* The NI Driver Station will continue to function with all currently released Systemcore images. Some newer Driver Station features (for example, OpMode selection and Alerts) are only available in the 2027 Driver Station and require Systemcore images >= 10 and WPILib >= v2027.0.0-alpha-5.

#### Third Party Library Compatibility by WPILib Version

| Library | WPILib v2027.0.0-alpha-2 | WPILib v2027.0.0-alpha-5/6 |
|---|---|---|
| CTRE Phoenix 6 | 25.90.0-alpha-1 or 25.90.0-alpha-2 | v26.50.0-alpha-1 |
| REVLib | v2027.0.0-alpha-1 | v2027.0.0-alpha-2 |
| ReduxLib | 2027.0.0-alpha-2 | :x: |
| PathPlannerLib | 2027.0.0-alpha-2 | :x: |
| ChoreoLib | 2027.0.0-alpha-1 | :x: |
| AdvantageKit | v27.0.0-alpha-3 | v27.0.0-alpha-4 |

- :x: — No compatible release currently available for this WPILib version.

## Systemcore Quick Start

|  |  |
|------------------------------------------|-----------------|
| Built-in Wi-Fi Access Point SSID         | `SYSTEMCORE`    |
| Built-in Wi-Fi Access Point Password     | `PASSWORD`      |
| Systemcore Wi-Fi Access Point IP         | `172.30.0.1`    |
| Systemcore USB IP (Windows)              | `172.26.0.1` (`172.28.0.1` for images 9 and before) |
| Systemcore USB IP (Linux, Mac)           | `172.27.0.1` (`172.29.0.1` for images 9 and before) |
| Systemcore Ethernet IP                   | Check display   |
| Default User                             | `systemcore`    |
| Default Password                         | `systemcore`    |
| Default Mount Locations for USB Storage  | `/U`, `/V`, etc |
| CAN Bus Interface Names                  | `can_s0` through `can_s4` for Systemcore, `can_d0` through `can_d19` for Motioncore |

### Powering On

For Alpha units, connect your Systemcore to your robot's power distribution board. We recommend the use of 18AWG wire with white Weidmuller ferrules.

> [!CAUTION]
> Do not use both power inputs on Alpha units (Bridge + Weidmuller) simultaneously.

For Beta units, the only way to power is through the MicroFit Pwr/Bridge port. If using Motioncore, connect the Pwr/Bridge port on Systemcore to the Bridge port on Motioncore using one of the provided Microfit cables. If using Expansion Hubs or raw power, use the MicroFit to XT30 cable included in your kit. (If you need raw wires, either cut off the XT30 or buy an XT30 extension cable off of Amazon and cut the wires off of that.)

### Flash Mode

For Alpha units, if the USB-C port is attached before power is applied, Systemcore will boot into flash mode.

For Beta units, hold down the Config button while powering the unit with USB connected to boot Systemcore into flash mode.

### Flashing OS Updates
Update to the latest stable release to get started.

[Systemcore Flash Tutorial Video (60s)](https://player.vimeo.com/video/1095423117)
<details>
<summary>Flashing OS Updates (Windows, macOS)</summary>

1. Download the latest release from the [systemcore-os-public repository](https://github.com/LimelightVision/systemcore-os-public). Make sure to grab the correct one for your hardware revision (alpha or beta)
2. Make sure the latest Limelight Hardware Manager is installed (see [Tooling](#tooling) above)
3. Open Limelight Hardware Manager
3. Navigate to the Flash OS Tab
4. Boot Systemcore into Flash Mode (see [Flash Mode](#flash-mode) section above). You should see activity in the log window. If you don't see anything, click the 'reinstall drivers' button at .
5. Select an OS .zip or .img to flash, and wait for extraction to complete
6. Refresh drives and select the one marked as Limelight/Systemcore
7. Click the “Flash” Button after it starts flashing. Full system images will take several minutes to flash.
8. Once complete, remove USB and power from Systemcore

</details>

<details>
<summary>Flashing OS Updates (Ubuntu/Debian)</summary>

1. Download the latest release from the [systemcore-os-public repository](https://github.com/LimelightVision/systemcore-os-public). Make sure to grab the correct one for your hardware revision (alpha or beta)
2. Download [Balena Etcher](https://etcher.balena.io/)
3. Spin-up RPIBoot:
    ```bash
    apt update
    apt install libusb-1.0-0-dev pkg-config build-essential
    git clone --recurse-submodules --shallow-submodules --depth=1 https://github.com/raspberrypi/usbboot
    cd usbboot
    make
    cd mass-storage-gadget64
    sudo ../rpiboot -d .
    ```
4. Boot Systemcore into Flash Mode (see [Flash Mode](#flash-mode) section above)
5. Flash with Etcher. Full system images will take several minutes to flash.
6. Once complete, remove USB and power from Systemcore

</details>

> [!NOTE]
> The new Hardware Manager will soon be fully cross-platform. Also, a browser-based OS update procedure is in development.

### Accessing the Web Interface and Setting Your Team Number

1. Boot Systemcore normally
2. Connect via USB-C, Ethernet, or Wi-Fi
3. Navigate to http://robot.local in a web browser
4. Configure your team number in the configuration tab, and click the red "Change Team Number" Button:
   ![](https://ik.imagekit.io/llimi/controlsystem/teamnumber.png)
5. Open the Driver Station software and configure it with a matching team number
6. The Driver Station should establish communications with Systemcore at this point:
   ![](https://ik.imagekit.io/llimi/controlsystem/dsconnectivity.png)

### Deploying Your First Robot Program to Systemcore (C++/Java)

1. Boot and establish connectivity with your Systemcore
2. Ensure WPILib 2027 has been installed
3. Open '2027_alpha5 WPILib VS Code'
4. Make a new WPILib project as you normally would, and make sure to set the correct team number during the project creation step
5. Deploy the project as you normally would
6. The Driver Station should reflect the presence of robot code:
   ![](https://ik.imagekit.io/llimi/controlsystem/dscode.png)
7. You're now ready to enable.
8. After initial testing with this new, simple, project, you can return to VS Code and open existing robot projects, which should trigger the Importer to create new 2027 projects based on those existing projects.

### Deploying Your First Robot Program to Systemcore (Python)

See [RobotPy specific notes](robotpy.md)

### Exploring On-Robot Telemetry with Elastic and AdvantageScope

[Package Installation Tutorial Video (30s)](https://player.vimeo.com/video/1095497571)

1. Download the Elastic and AdvantageScope IPK packages.
2. Navigate to the web interface.
3. Click the "Add Package" card.
4. Install one package at a time.
5. Click the new "Elastic" or "AdvantageScope Lite" launch cards.
6. Use Elastic and AdvantageScope as you normally would. A quick way to add live telemetry to your robot is the use of the SmartDashboard API (subject to change soon) ```SmartDashboard.putNumber("key",value);```.

> [!NOTE]
> Elastic and AdvantageScope Lite packages will soon be pre-baked into the OS.

### Enabling CANivore Support

[Package Installation Tutorial Video (30s)](https://player.vimeo.com/video/1095497571)

1. [Download the CANivore IPK packages](https://github.com/wpilibsuite/SystemCoreTesting/blob/main/CTR-Phoenix.md#download)
2. Navigate to the web interface
3. Click the "Add Package" card
4. Install the `usb-kernel` package, and then install the `usb` package
5. Powercycle your Systemcore.

### Making Cables

Check the "Mating Connector" Section of the [Systemcore Specification PDF](https://downloads.limelightvision.io/documents/systemcore_specifications_june15_2025_alpha.pdf), and [this GitHub discussion](https://github.com/wpilibsuite/SystemCoreTesting/discussions/11).

### Transitioning from roboRIO
#### I2C

The I2C pinout does not match that of the roboRIO. Cables such as https://www.revrobotics.com/rev-11-1729/?searchid=4554778&search_query=4+wire+i2c will need to be modified - SCL and SDA will need to be swapped. The Systemcore pinout matches the Qwiic / ControlHub pinout.

#### IO

The IO signal pins have a 4.7K pulldown resistor. Limit switches will need to short the SIGNAL pin to the +3.3V pin on Systemcore.
