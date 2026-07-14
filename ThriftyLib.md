# 2027 ThriftyBot Beta Software


## ThriftyLib API (v2027.0.0)

### Installation
To use the 2027 Beta, use following URL:

**Vendor JSON URL:**
```
https://docs.home.thethriftybot.com/software/thriftylib/ThriftyLib-2027-2027.0.0-alpha-1.json
```

### Migration Guide (v2026 → v2027)

**NOTE**: The 2027 ThriftyLib API has been rewritten from the ground up. Our focus was on making the integration of new products as simple as possible for the end user. We recomend treating this as a new product and start out by reading our docs at our new docs site. API documentation can also be found here.

[ThriftyLib Docs Alpha](https://thrifty-docs.web.app/software/thriftylib/2027.0.1-Beta/overview)

#### 1. Nova object
The class name has been changed from `ThriftyNova` to `Nova`. Methods have been reorganized into categories found within the `Nova` object. These can be accessed from `.control()`, `configure()` and `status()`. 

Each category then contains methods that pertain to this subject. Think of this in the way of "What do I want to do with the Nova?"

"Set controls" -> `control()`
"Set configs' -> `configure()`
"Check the current ..." -> `status()`

For more information checkout the new documentation

#### 2. Package Reorganization
For all nova related functions you will need the following imports.

```java
import com.thrifty.nova.*;
import com.thrifty.core.Motor;
```

#### 3. New Configuration Pattern
Configurations have been reworked to combine how both single and multiple configurations are added. 

```java
import com.thrifty.nova.NovaConfig;
Nova motor = new Nova(0, 1); // Bus id = 0, CAN ID = 1

motor.configure(
    NovaConfig.inverted(true),
    NovaConfig.brakeMode(false)
);

```
The `configure()` method accepts an array of `NovaConfig`s. This array can be made directly inside of the configure method. 

Batch configs can also be created and applied to many objects through a `NovaConfigBatch`. 

```java
NovaConfigBatch drivetrainDefaults = new NovaConfigBatch(
    NovaConfig.brakeMode(true),
    NovaConfig.statorCurrent(40),
    NovaConfig.supplyCurrent(30),
    NovaConfig.rampForward(200),
    NovaConfig.rampReverse(200)
);

motor.configure(drivetrainDefaults);
```

## ThriftyConfig App (v2027.0.0-beta.1)
[Download for Windows](https://docs.home.thethriftybot.com/software/app/ThriftyConfig+Setup+2027.0.0-beta.1.exe)

**Key Features:**
- Swerve Project Generator
- Multi-controller Graphing and control  
- Nova Configuration over CAN


## Nova Firmware (v2027.0.0-beta.1)
[Download Firmware](https://docs.home.thethriftybot.com/pages/nova-firmware.html)
*Required for 2027 API and App features.*


## System Requirements

- Windows 10 or later (64-bit)
- macOS support is planned for a future release

## Additional Resources

- ThriftyConfig Documentation: [https://docs.home.thethriftybot.com/ThriftyConfig.html](https://docs.home.thethriftybot.com/ThriftyConfig.html)

- Nova Firmware Releases: [https://docs.home.thethriftybot.com/NovaFirmware.html](https://docs.home.thethriftybot.com/NovaFirmware.html)

- ThriftyConfig Releases: [https://docs.home.thethriftybot.com/ThriftyConfig.html](https://docs.home.thethriftybot.com/ThriftyConfig.html)
