# Systemcore FTC Alpha Instructions

## Introduction
Thank you for volunteering to help us alpha test the new Systemcore, Motioncore, A301, and 18V battery systems. These systems represent a significant change from the hardware and software FTC teams are used to, and as an alpha test, we expect there to be bumps along the road. Some elements of the system, particularly software and documentation, are either not yet finished or are unpolished. We encourage you to share feedback on what feels like it’s missing, or areas where you are stuck or struggling.
This document links heavily into existing alpha and WPILib documentation in order to avoid repeating material already created. In many cases this existing material is not yet assembled into a cohesive workflow, especially for FTC teams, so it is recommended to refer to linked documents only to complete the steps described and then return to this document rather than proceeding to the next steps in the external reference.

## Testing Logistics
The majority of testing communication moving forward will be handled via the GitHub project. This allows all students and mentors participating in testing to view any updates/communication directly rather than forcing a single contact into the position of forwarding e-mail communications. It also allows other members of the community not chosen to participate to follow along.

## Significant Updates
Any significant updates from FIRST will be posted in the Announcements section of the discussions. This will include things like major revisions of software being available, any additional testing tasks that may be added based on feedback, etc.

## Providing Feedback
Teams are encouraged to err on the side of oversharing. I would rather read something in a team's progress thread or in an issue and point to an already filed issue 10 times than have 10 teams all struggling with the same thing all assuming they are doing something wrong and not letting us know.

#### General Progress Updates
Each team should create a discussion thread in the FTC Progress Updates section to report progress as they work through testing. Teams can choose to use this thread as a “running diary” updated every meeting, or to share occasionally as significant chunks of work are completed. Either way teams should be sharing not just what issues they had, but successes as well. If you don’t have much feedback to share, these posts can be fairly brief but we would still like to keep tabs on what progress teams are making.

> Example post 1: We got through wiring and WPILib installation today, no major questions or feedback, the process seemed fairly straightforward.

> Example post 2: Over the last two weeks we completed all of Task 1. Parts X and Y were pretty easy, but we struggled a bit with part Z. We were able to figure it out but suggest including BLAH in the instructions to make things a bit more clear. We’re aiming to work on the next Task over the next month or so.

#### Specific issues
As an Alpha, we expect it’s very possible that there are missing/unclear instructions, or bugs in software or hardware that may impede your ability to accomplish testing. While we certainly don’t want to discourage folks debugging on your own, if you are thinking about whether you should ask about something, definitely ask! We’d rather answer more questions and get you moving again than have a bunch of teams wasting time struggling due to bugs or unclear instructions.
Specific issues with software, hardware or instructions can be posted in the [Issues section](https://github.com/wpilibsuite/SystemcoreTesting/issues).
More general questions or discussion (e.g. “I was wondering about…” or “how are people handling…”) can be posted in the most relevant [Discussion section](https://github.com/wpilibsuite/SystemcoreTesting/discussions).

## Community Communication
*FIRST* allows, and even encourages, you to share information you learn through this testing with other teams in your communities, both online and in person. We do ask that you consider that this is an Alpha, and we still have roughly a year to further develop the software and documentation prior to use in FTC 2027-28 season. Things can and will change, particularly around the A301 software implementation, prior to final release. Please make sure you are sharing any pain points with us first via the methods described above and then make sure you are providing proper context when sharing with the community.

## Task 1 – Hybrid Configuration Basic Testing
#### Wiring
The hybrid configuration allows teams to take advantage of the new technology in Systemcore while using familiar motors and batteries. This allows teams to spread out both the shock of learning a whole new system and investment in new components across multiple seasons. This configuration utilizes a current FTC battery connected to one or more Expansion Hubs in order to connect motors, servos and encoders. All other sensors connect to Systemcore which controls the Expansion Hubs over USB.
<img width="975" height="354" alt="System Diagram of hybrid configuration" src="https://github.com/user-attachments/assets/97420d13-6965-4037-9337-a04ce020e1eb" />
Figure 1 - Hybrid System Block Diagram. Created with Gemini

Power can be chained through additional Expansion Hubs if needed before ending with Systemcore, though it is recommended to start with a single Expansion Hub to keep things simple while working on initial bring-up of the system. Additional Expansion Hubs would each connect to a unique Systemcore USB port for data transfer.

#### WPILib Installation
The Systemcore control system will use WPILib as the base library infrastructure across both FTC and FRC. WPILib and its dedicated copy of the VSCode IDE can be installed on Windows, Mac or Linux. Download the latest alpha release using the link in the [Tooling section of the Readme on this repo](https://github.com/wpilibsuite/SystemcoreTesting#tooling) then intall using [these instructions](https://docs.wpilib.org/en/stable/docs/zero-to-robot/step-2/wpilib-setup.html).

> [!TIP]
> The year and version on the desktop icons and WPILib folder might not match the WPILib version from the installer. As of this writing, the latest WPILib is 2027.0.0-alpha-6, which installs to a 2027_alpha5 folder and shortcuts. The reason for this is complicated, but it is intended behavior. Alpha 6 will install straight over an Alpha 5 installation. In season, this will not be an issue, as the folder will just be the year by itself; this is purely an Alpha and Beta issue.

#### Systemcore Configuration
1. Download and install the Limelight Hardware Manager for your platform from the appropriate [link on the GitHub page](https://github.com/wpilibsuite/SystemcoreTesting/tree/main#tooling)
2. Using a USB-C cable plugged into Systemcore, flash the latest available OS update using the [Flashing instructions](https://github.com/wpilibsuite/SystemcoreTesting/tree/main#flash-mode). Your unit is a Beta unit so make sure to select the appropriate Beta OS.
3. Still using the USB connection, access the Systemcore via web browser, and [configure the team number and WiFi](https://github.com/wpilibsuite/SystemcoreTesting/tree/main#accessing-the-web-interface-and-setting-your-team-number).

You may also wish to verify that you can now connect to Systemcore wirelessly and access it via a web browser.

#### Driver Station installation
A solution for controlling Systemcore using Driver Hub is still in development. (Those interested in the details can see https://github.com/wpilibsuite/SystemcoreTesting/issues/308 for progress). In the meantime, you will need to use a PC application to control the robot. Download the release for your platform from the [Driver Station Releases page](https://github.com/wpilibsuite/FirstDriverStation-Public/releases/). For Windows, run the installer and follow the instructions. For other platforms, see the installation notes in the [Readme](https://github.com/wpilibsuite/FirstDriverStation-Public).

1. Make sure your Systemcore is connected to the PC via the Systemcore USB-C port
2. Launch the Driver Station application
3. You should see the Driver Station detect the Systemcore, turning the top indicator in the center cluster green and updating the battery voltage value and mini chart also in the center.

You may also wish to disconnect the USB and connect to the Systemcore Wifi to verify DS connection over WiFi before proceeding.

> [!IMPORTANT]
> Running the new Driver Station software on the REV Driver Hub is expected to be the only legal control method at events; the PC release is not expected to be competition-legal in FTC. Additionally, the new Driver Station will **not** run on Android, including the phones legal for use with the legacy system.

#### First example program
We’ll be working on resources for understanding WPILib and differences with the FTC SDK shortly, but first we want to deploy a simple sample program to verify that everything is configured and wired correctly.

1. Follow the directions [on this page](https://docs.wpilib.org/en/2027/docs/zero-to-robot/step-4/creating-test-drivetrain-program-cpp-java-python.html#deploying-the-project-to-a-robot) to create your sample program, with the exception of selecting Expansion Hub Sample instead of Getting Started and stopping once you have the project open. The rest of the page describes the Getting Started sample program and is not applicable to this test.
2. Once you’ve created and opened the project, use [these instructions](https://docs.wpilib.org/en/2027/docs/software/vscode-overview/deploying-robot-code.html#building-and-deploying-robot-code) to deploy the project.
3. The program is set up to map each joystick axis (i.e. left stick X/Y + right stick X/Y) to one of the 4 motors on the Expansion Hub to test basic functionality.
4. Open the Driver Station software, ensure your gamepad is detected by clicking the controller tab on the left side, set the mode to Teleop and click Enable.
5. Each of the 4 joystick axes should control one of the 4 motor outputs of the Expansion Hub.
6. If desired, you could modify this program to work with servos, additional Expansion Hubs and/or gamepads, or other buttons or axes on the gamepad to start exploring WPILib before moving on to more unique code in Task 2.

## Task 2 – Hybrid Configuration Additional Testing
The second task is to continue testing with the hybrid configuration using programs of your own. Using the classes in [org.wpilib.hardware.expansionhub](https://github.wpilib.org/allwpilib/docs/2027/java/org/wpilib/hardware/expansionhub/package-summary.html) to communicate with motors, encoders and servos connected to one or more Expansion Hubs, develop one or more programs to control a mechanism or robot.

Multiple Expansion Hubs can be connected to Systemcore, with each connected to it's own USB port. RS485 connection between Expansion Hubs is not supported.

> [!TIP]
> In the `org.wpilib.driverstation` package, you'll see gamepad classes prefixed `Ni...`. Do not use these classes, these are for the old FRC Driver Station. For the new 2027 Driver Station, use the `Gamepad` class, which has universal mappings for anything that shows up as a Gamepad in the Driver Station.

## More info on Blocks and A301 Testing coming soon!
