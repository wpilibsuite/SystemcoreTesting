# AdvantageScope 2027 Alpha

**Downloads for the 2027 alpha releases of AdvantageScope can be found [here](https://github.com/Mechanical-Advantage/AdvantageScope/releases) (find the first release with version v27.0.0-alpha-X).** The changelog below lists the available releases along with related compatibility information. See also the [known issues](#known-issues) section below.

### [v27.0.0-alpha-6](https://github.com/Mechanical-Advantage/AdvantageScope/releases/tag/v27.0.0-alpha-6)

- Updated Driver Station keys to new layout ([#550](https://github.com/Mechanical-Advantage/AdvantageScope/pull/550), @ThadHouse)
- Added preference for timestamp display mode (see the "Timestamps" documentation page)
- Updated WPILib NetworkTables layout for new table name ([allwpilib#9047](https://github.com/wpilibsuite/allwpilib/pull/9047))
- Added support for units of power
- Added support for ANSI escape sequences in console tab
- Improved rendering of large timestamp values in the line graph and timeline
- Updated boolean values in sidebar to differentiate without color
- Updated sidebar background when reduced transparency mode is active
- Disabled 3D field animations when reduced motion mode is active
- Added backwards compatibility for downloading logs from the roboRIO
- Added alpha/beta feedback button to AdvantageScope Lite
- Disabled alpha/beta expiration logic in AdvantageScope Lite for the FIRST Driver Station
- Changed default robot address to `robot.local`
- Added placeholder text to the robot log path preference
- Fixed a bug sometimes causing Driver Station console text to fail to load
- Fixed cancel button for unit conversion popup in AdvantageScope lite
- Fixed visual glitches with timeline mark opacity
- Fixed loading animation for AdvantageScope Lite
- Fixed a bug causing the robot mode to briefly render incorrectly when loading a log file
- Fixed parsing of `SwerveModuleVelocity` ([#553](https://github.com/Mechanical-Advantage/AdvantageScope/pull/553), @blaze-developer)
- Fixed support for mDNS robot addresses when downloading logs
- Fixed tabbing controls in the source list help window

### [v27.0.0-alpha-5](https://github.com/Mechanical-Advantage/AdvantageScope/releases/tag/v27.0.0-alpha-5)

- Added AdvantageScope Lite for the FIRST Driver Station with simplified features and a built-in layout ([#543](https://github.com/Mechanical-Advantage/AdvantageScope/pull/543))
- Added "Driver Station" live mode for connecting to the FIRST Driver Station, supported on both desktop and the DS distribution of AdvantageScope Lite
- Added support for FIRST Driver Station logging formats (including robot mode, joysticks, match info, and console data)
- Added support for 2027 robot mode formats (AdvantageKit, DataLog, and NetworkTables)
- Added support for 2027 joystick data, including touchpads and alternative button mappings ([#544](https://github.com/Mechanical-Advantage/AdvantageScope/pull/544))
- Added support for utility/test mode (with updated timeline colors)
- Added a "Show Robot Mode" option to the line graph with improved visuals
- Added line grid to 2D and 3D FTC fields ([#527](https://github.com/Mechanical-Advantage/AdvantageScope/pull/527), @blaze-developer)
- Added menu bar options for USB/Wi-Fi Systemcore addresses
- Applied all changes from [v26.0.0-beta-2](https://github.com/Mechanical-Advantage/AdvantageScope/releases/tag/v26.0.0-beta-2), [v26.0.0](https://github.com/Mechanical-Advantage/AdvantageScope/releases/tag/v26.0.0), [v26.0.1](https://github.com/Mechanical-Advantage/AdvantageScope/releases/tag/v26.0.1), and [v26.0.2](https://github.com/Mechanical-Advantage/AdvantageScope/releases/tag/v26.0.2)

<details>
<summary>Minor Changes</summary>

- Added help overlays for the line graph and timeline ([#545](https://github.com/Mechanical-Advantage/AdvantageScope/pull/545))
- Updated struct unit support for WPILib 2027 types
- Added support for renamed swerve velocity types (`SwerveModuleVelocity` and `ChassisVelocities`)
- Fixed a crash when filtering console data on a paused live source ([#519](https://github.com/Mechanical-Advantage/AdvantageScope/pull/519), @blaze-developer)
- Fixed an issue causing XR fields to render with incorrect offsets ([#530](https://github.com/Mechanical-Advantage/AdvantageScope/pull/530), @blaze-developer)
- Added support for non-bundled config JSON loading in AdvantageScope Lite ([#540](https://github.com/Mechanical-Advantage/AdvantageScope/pull/540), @ThadHouse)
- Improved the reliability of keyboard shortcuts in AdvantageScope Lite ([#529](https://github.com/Mechanical-Advantage/AdvantageScope/pull/529))
- Fixed AdvantageScope Lite server on Windows ([#500](https://github.com/Mechanical-Advantage/AdvantageScope/pull/500), @ThadHouse)
- Fixed state storage for AdvantageScope Lite to improve reliability
- Fixed inconsistent 404 message in AdvantageScope Lite server
- Added support for tab controls on all input fields
- Added tooltips for all input fields
- Switched all user-facing dates to ISO formats
- Added new energy unit ([#514](https://github.com/Mechanical-Advantage/AdvantageScope/pull/514), @harnwalN)
- Renamed "NetworkTables 4" to "NetworkTables"
- Renamed "NetworkTables 4 (Systemcore)" to "Systemcore Diagnostics" for clarity
- Adjusted some user-facing text to improve clarity
- Fixed Electron locale configuration ([#538](https://github.com/Mechanical-Advantage/AdvantageScope/pull/538))
- Fixed an issue causing menu bars to incorrectly appear in popup windows on Windows and Linux
- Updated sidebar style when running on macOS Golden Gate
- Added local network usage description for macOS
- Removed support for window style used on macOS Catalina and older
- Removed default NI Driver Station log folder path on Windows
- Removed experimental warning when switching to FTC fields

</details>

### [v27.0.0-alpha-4](https://github.com/Mechanical-Advantage/AdvantageScope/releases/tag/v27.0.0-alpha-4)

- Added support for uploading assets through AdvantageScope Lite (#411)
- Improve the reliability of keyboard shortcuts in AdvantageScope Lite (#390)

_The following changes are also available in [v26.0.0-beta-1](https://github.com/Mechanical-Advantage/AdvantageScope/releases/tag/v26.0.0-beta-1):_

- Added Road Runner log support (#373, @j5155)
- Added FTC Dashboard support (#380, @j5155)
- Added support for CSV files (#410, @amsam0)
- Added support for unit-aware graphing to the line graph tab
- Added support for AprilTags on FTC fields
- Added warning when switching to an FTC field for the first time
- Added menu options to quickly switch between live sources
- Added right-click menu in sidebar for copying field names to the clipboard (#398, @virtualkyle378)
- Improved organization of preferences window
- Added app icon for macOS Tahoe
- Added translucent sidebar effect on Windows 11
- Removed support for PathPlanner 2023 live source
- Added support for system installations of FFmpeg
- Fixed a bug causing incorrectly filtered values to be displayed in the line graph legend (#414)
- Fixed an issue causing some very large WPILOGs to fail to open (#406)

### [v27.0.0-alpha-3](https://github.com/Mechanical-Advantage/AdvantageScope/releases/tag/v27.0.0-alpha-3)

_The following changes are also available in [v26.0.0-beta-1](https://github.com/Mechanical-Advantage/AdvantageScope/releases/tag/v26.0.0-beta-1):_

- Fixed WPILOG parsing with official WPILib distributions of AdvantageScope
- Added support for separate 2D mechanisms on XZ and YZ planes
- Added warning on line graph when no enabled state found
- Fixed edge cases when resizing controls pane
- Fixed visual glitches when switching line graph styles

### [v27.0.0-alpha-2](https://github.com/Mechanical-Advantage/AdvantageScope/releases/tag/v27.0.0-alpha-2)

_The following changes are also available in [v26.0.0-beta-1](https://github.com/Mechanical-Advantage/AdvantageScope/releases/tag/v26.0.0-beta-1):_

- **Fixed rendering of WPILOGs with timestamp ranges starting after zero** (addresses the [known issue](#log-files-are-very-zoomed-out-by-default) below)
- **Fixed functionality of in-app menus on Windows and Linux**
- Added support for unit conversion with byte values
- Improved stability of YouTube and TBA video downloading
- Fixed visual artifacts on line graph tab with differentiated fields
- Fixed handling of `ELECTRON_RUN_AS_NODE` environment variable
- Fixed rendering of very large raw and array values

### [v27.0.0-alpha-1](https://github.com/Mechanical-Advantage/AdvantageScope/releases/tag/v27.0.0-alpha-1)

- Added support for AdvantageScope Lite ([docs](https://docs.advantagescope.org/more-features/advantagescope-lite))
- Added log downloading support for Systemcore using SFTP
- Added support for the system NT server on Systemcore, including DS and joystick data (select "NetworkTables 4 (Systemcore)")
  - We plan to allow simultaneous connections to the primary and system NT server, but this feature is not currently available.
- Added optional support for the unified 2027 coordinate system (disabled by default)
  - This new coordinate system uses a center-field origin facing away from the red alliance, and is consistent across FRC and FTC. The default coordinate system remains unchanged to ease the transition when porting existing FRC code, but this can be changed in the preferences menu.
- Removed shortcut for the USB roboRIO address
  - Adjust the "Robot Address" in the preferences menu when connecting to Systemcore via USB. A shortcut for the Systemcore USB address(es) may be added in a future release.
- Removed support for legacy numeric array format ([docs](https://docs.advantagescope.org/legacy-formats))

_The following changes are also available in [v26.0.0-beta-1](https://github.com/Mechanical-Advantage/AdvantageScope/releases/tag/v26.0.0-beta-1):_

- Added support for FTC fields and robots
- Improved handling of coordinate systems for FTC compatibility
- Improved color options for robot bumpers
- Added support for 2D mechanisms on the YZ axis of 3D robots
- Added support for 3D view antialiasing
- Renamed odometry tab to "2D Field" for consistency & accuracy
- Reorganized menu bar for consistency across platforms
- Removed "experimental" warning for AdvantageScope XR
- Removed support for Zebra MotionWorks (no longer in use at events)
- Restrict update checking to the current major version
- Improved the efficiency of line graph plotting
- Fixed handling of WPILOGs with timestamp ranges starting after zero
- Fixed edge cases with "Zoom to Enabled Range" function
- Fixed translucency of ghost objects on the 2D field
- Fixed a bug causing invalid 3D camera positions when reopening AdvantageScope
- Fixed the ordering of AdvantageKit logs with randomized names in the download window
- Fixed handling of default values and empty arrays in Protobuf messages

## Known Issues

### Robot mode and joystick data is not available in Systemcore diagnostics mode

Most users should use the "NetworkTables" or "NetworkTables (AdvantageKit)" live modes to connect directly to robot code, where these features are supported. This issue will be fixed in a future release.

### URCL is not supported

There is currently no 2027 alpha release for [URCL](https://docs.advantagescope.org/more-features/urcl). We will have more details to share about logging options in 2027 and beyond at a later date.

### ~~Joystick visualization with POVs is inaccurate~~

_Fixed by [v27.0.0-alpha-5](#v2700-alpha-5)_

~~Systemcore and the 2027 alpha of WPILib use a new format for joystick POV values that is not currently compatible with AdvantageScope, causing the joystick visualization to display inaccurate data. This issue will be fixed in a future release.~~

### ~~Keyboard shortcuts do not function consistently in AdvantageScope Lite~~

_Fixed by [v27.0.0-alpha-5](#v2700-alpha-5)_

~~The menu bar in AdvantageScope Lite displays keyboard shortcut information, but these shortcuts may function inconsistently depending on the OS and browser. This issue is being investigated and will be fixed in a future release (see https://github.com/Mechanical-Advantage/AdvantageScope/issues/390).~~

### ~~Log files are very zoomed out by default~~

_Fixed by [v27.0.0-alpha-2](#v2700-alpha-2)_

~~By default, log files from Systemcore currently use [epoch timestamps](https://en.wikipedia.org/wiki/Unix_time) that are not properly handled by AdvantageScope. This causes the timeline to be very zoomed out when opening log files, so significant zooming is required to access data. There are also various visual artifacts throughout the UI. This issue will be fixed in future releases of AdvantageScope and WPILib (see https://github.com/wpilibsuite/allwpilib/issues/8041).~~
