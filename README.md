# Marlin 3D Printer Firmware

![GitHub](https://img.shields.io/github/license/marlinfirmware/marlin.svg)
![GitHub contributors](https://img.shields.io/github/contributors/marlinfirmware/marlin.svg)
![GitHub Release Date](https://img.shields.io/github/release-date/marlinfirmware/marlin.svg)
[![Build Status](https://github.com/MarlinFirmware/Marlin/workflows/CI/badge.svg?branch=bugfix-2.0.x)](https://github.com/MarlinFirmware/Marlin/actions)

<img align="right" width=175 src="buildroot/share/pixmaps/logo/marlin-250.png" />

Additional documentation can be found at the [Marlin Home Page](https://marlinfw.org/).
Please test this firmware and let us know if it misbehaves in any way. Volunteers are standing by!

## CUSTOMIZED VERSION FOR CREALITY ENDER 3 PRO WITH SKR 1.4 Turbo

This version of the Marlin 2.0 firmware has been customized with the goal of getting a "standard" setup working for an Ender 3 (Pro). The base configurations from [Marlin - Configurations](https://github.com/MarlinFirmware/Configurations), specifically the Creality Ender 3 example, were used to modify this branch with the closes possible values so that upon building and installing the firmware the printer is close to being ready for printing.

_Note, while the turbo version of the SKR 1.4 is selected in this branch simple changes in `platformio.ini` and `Configuration.h` can change the board type to the standard SKR 1.4 with no other needed modifications._

**PLEASE READ THROUGH THE CHANGES BELOW. I CANNOT BE RESPONSIBLE FOR DAMAGE YOU MAY DO TO YOUR PRINTER BY NOT UNDERSTANDING WHAT HAS BEEN CHANGED IN THE FIRMWARE. IF YOU DON'T CHANGE BLTOUCH SETTINGS FOR 5V MODE OR NOZZLE OFFSETS THEN YOU CAN PHYSICALLY DAMAGE YOUR SETUP IF YOUR PARAMETERS FALL OUTSIDE OF WHAT I HAVE CHANGED. THESE ARE EASY CHANGES DON'T BE FOOLED. THE HARD WORK HAS BEEN DONE FOR YOU!**

### Notable Changes

This branch comes from the 2.0.5.3 release/tag. Use the compare function to compare it the current Marlin 2.0.x branch or the 2.0.5.3 tag to quickly see all the changes from the stock firmware. There is a branch, _feature/ender3/base_, which is at the point where the Marlin 2.0 firmware from the 2.0.5.3 release has only the stock configuration changes for the Ender 3 from the Configurations repository. This represents essentially a base build which could be made for the SKR 1.4 Turbo with no other features such as bed leveling enabled. The _feature/ender3/enhanced_ branch are all of my changes for enabling the expanded feature set of Marlin. I suggest using it as a jumping point for any other changes. The _base_ branch needs further validation as I only used it as a place to go back to when my changes in the _enhanced_ branch didn't produce the results I was looking for.

#### Configuration.h Changes

* Custom Ender bootscreen is enabled!
* Base configuration applied from [Marlin - Configurations](https://github.com/MarlinFirmware/Configurations)
* BLTouch - Use the pins as specified in the SKR 1.4 Turbo pinout. As the author I've left my defaults in for my BLTouch probe offsets. **Please change these offsets to those THAT YOU MEASURE PHYSICALLY for best results and to prevent any nozzle/bed collisions**
* Bilinear Bed Leveling - This works best for me. I was unhappy with configuring UBL. Customizing this part of the firmware should be easy enough once you've verified that all is well with your printer.
* TMC2209 drivers are configured in UART mode
* S Curve Acceleration is enabled
* Preheat defaults have been upped - PLA: Hotend 210, Bed: 60 - ABS: Bed 70
* Nozzle park is enabled
* SD card support on the mainboard is enabled and set to half speed. _Two things to note. First, if you're using the BTT TFT35 V3 (either version) and want to use the SD card on the LCD you need to disable by commenting `#define SDCARD_CONNECTION ONBOARD` in `Configuration_adv.h`. Secondly, I set it to half speed when attempting to try to get the SD card working from the mainboard. This may not be necessary but since everything is working in my case I felt fine leaving it._
* I've uncommented `REPRAP_DISCOUNT_FULL_GRAPHIC_SMART_CONTROLLER`. I don't feel like I need it to be set since I'm using EXP3 on the TFT 35 E V3 for Marlin emulation mode.

#### Configuration_adv.h Changes

* Quick homing
* BLTouch 5v mode
* Status and long filename scrolling
* SD card support enabled for the mainboard (see note about sd card support in the `Configuration.h` section)
* Babystepping - Double click, Z multiplication factor set to 10 for easier adjustment
* Linear advance enabled
* Advanced auto-pause (change filament)
* Vref's set for based on [knoopx's research](https://gist.github.com/knoopx/e6c40a009e796203b93a75a3ed6a5ab8)
* Motor driver status and TMC debug enabled
* Photo gcode support (not tested)
* Cancel objects support (not tested)

### Other Helpful Tips

My first SKR 1.4 Turbo board was bad. It actually took out a USB port on my desktop. Be careful when plugging in your board. I'm still having connection issues with Octoprint. It will not automatically connection. After the printer is online and Octoprint is online you may have to refresh the connection setting, manually select the serial connection, and then set the baud rate to AUTO. It will connect at 115200 but manually setting that and attempting the connection will not work.

If you're having trouble connecting to Octoprint over USB make sure that the Pi or computer running Octoprint can "see" the SKR 1.4. If on an OctoPi setup then SSH into the Pi, disconnect the mainboard from USB, and then run `lsusb`. Note the devices shown. Plug in the SKR and then run `lsusb` again. You should see a new entry for the printer. If not you may have a bad SKR board as this was the issue with my first board. You can also try `sudo /sbin/udevadm monitor` while plugging in the printer and watch to see what serial connection is assigned to the printer. 

### Next Steps

* Serial connection from SKR or TFT35 to Pi to eliminate the need for the USB cable.
* Fixing random LCD issues (this may have been an installation issue for me)
* Better connection with OctoPi/Octoprint.

### Special Thanks to The Following

* [TeachingTech](https://www.youtube.com/channel/UCbgBDBrwsikmtoLqtpc59Bw)
* [Chris Riley](https://www.youtube.com/channel/UCqRiv7rQuxge63bqJ2hVNUQ)
* [The users who troubleshot the SD card issue](https://github.com/MarlinFirmware/Marlin/issues/14320)

## Marlin 2.0

Marlin 2.0 takes this popular RepRap firmware to the next level by adding support for much faster 32-bit and ARM-based boards while improving support for 8-bit AVR boards. Read about Marlin's decision to use a "Hardware Abstraction Layer" below.

Download earlier versions of Marlin on the [Releases page](https://github.com/MarlinFirmware/Marlin/releases).

## Building Marlin 2.0

To build Marlin 2.0 you'll need [Arduino IDE 1.8.8 or newer](https://www.arduino.cc/en/main/software) or [PlatformIO](http://docs.platformio.org/en/latest/ide.html#platformio-ide). Detailed build and install instructions are posted at:

  - [Installing Marlin (Arduino)](http://marlinfw.org/docs/basics/install_arduino.html)
  - [Installing Marlin (VSCode)](http://marlinfw.org/docs/basics/install_platformio_vscode.html).

### Supported Platforms

  Platform|MCU|Example Boards
  --------|---|-------
  [Arduino AVR](https://www.arduino.cc/)|ATmega|RAMPS, Melzi, RAMBo
  [Teensy++ 2.0](http://www.microchip.com/wwwproducts/en/AT90USB1286)|AT90USB1286|Printrboard
  [Arduino Due](https://www.arduino.cc/en/Guide/ArduinoDue)|SAM3X8E|RAMPS-FD, RADDS, RAMPS4DUE
  [LPC1768](http://www.nxp.com/products/microcontrollers-and-processors/arm-based-processors-and-mcus/lpc-cortex-m-mcus/lpc1700-cortex-m3/512kb-flash-64kb-sram-ethernet-usb-lqfp100-package:LPC1768FBD100)|ARM® Cortex-M3|MKS SBASE, Re-ARM, Selena Compact
  [LPC1769](https://www.nxp.com/products/processors-and-microcontrollers/arm-microcontrollers/general-purpose-mcus/lpc1700-cortex-m3/512kb-flash-64kb-sram-ethernet-usb-lqfp100-package:LPC1769FBD100)|ARM® Cortex-M3|Smoothieboard, Azteeg X5 mini, TH3D EZBoard
  [STM32F103](https://www.st.com/en/microcontrollers-microprocessors/stm32f103.html)|ARM® Cortex-M3|Malyan M200, GTM32 Pro, MKS Robin, BTT SKR Mini
  [STM32F401](https://www.st.com/en/microcontrollers-microprocessors/stm32f401.html)|ARM® Cortex-M4|ARMED, Rumba32, SKR Pro, Lerdge, FYSETC S6
  [STM32F7x6](https://www.st.com/en/microcontrollers-microprocessors/stm32f7x6.html)|ARM® Cortex-M7|The Borg, RemRam V1
  [SAMD51P20A](https://www.adafruit.com/product/4064)|ARM® Cortex-M4|Adafruit Grand Central M4
  [Teensy 3.5](https://www.pjrc.com/store/teensy35.html)|ARM® Cortex-M4|
  [Teensy 3.6](https://www.pjrc.com/store/teensy36.html)|ARM® Cortex-M4|

## Submitting Changes

- Submit **Bug Fixes** as Pull Requests to the ([bugfix-2.0.x](https://github.com/MarlinFirmware/Marlin/tree/bugfix-2.0.x)) branch.
- Follow the [Coding Standards](http://marlinfw.org/docs/development/coding_standards.html) to gain points with the maintainers.
- Please submit your questions and concerns to the [Issue Queue](https://github.com/MarlinFirmware/Marlin/issues).

## Marlin Support

For best results getting help with configuration and troubleshooting, please use the following resources:

- [Marlin Documentation](http://marlinfw.org) - Official Marlin documentation
- [Marlin Discord](https://discord.gg/n5NJ59y) - Discuss issues with Marlin users and developers
- Facebook Group ["Marlin Firmware"](https://www.facebook.com/groups/1049718498464482/)
- RepRap.org [Marlin Forum](http://forums.reprap.org/list.php?415)
- [Tom's 3D Forums](https://forum.toms3d.org/)
- Facebook Group ["Marlin Firmware for 3D Printers"](https://www.facebook.com/groups/3Dtechtalk/)
- [Marlin Configuration](https://www.youtube.com/results?search_query=marlin+configuration) on YouTube

## Credits

The current Marlin dev team consists of:

 - Scott Lahteine [[@thinkyhead](https://github.com/thinkyhead)] - USA &nbsp; [Donate](http://www.thinkyhead.com/donate-to-marlin)
 - Roxanne Neufeld [[@Roxy-3D](https://github.com/Roxy-3D)] - USA
 - Chris Pepper [[@p3p](https://github.com/p3p)] - UK
 - Bob Kuhn [[@Bob-the-Kuhn](https://github.com/Bob-the-Kuhn)] - USA
 - Erik van der Zalm [[@ErikZalm](https://github.com/ErikZalm)] - Netherlands &nbsp; [![Flattr Erik](https://api.flattr.com/button/flattr-badge-large.png)](https://flattr.com/submit/auto?user_id=ErikZalm&url=https://github.com/MarlinFirmware/Marlin&title=Marlin&language=&tags=github&category=software)

## License

Marlin is published under the [GPL license](/LICENSE) because we believe in open development. The GPL comes with both rights and obligations. Whether you use Marlin firmware as the driver for your open or closed-source product, you must keep Marlin open, and you must provide your compatible Marlin source code to end users upon request. The most straightforward way to comply with the Marlin license is to make a fork of Marlin on Github, perform your modifications, and direct users to your modified fork.

While we can't prevent the use of this code in products (3D printers, CNC, etc.) that are closed source or crippled by a patent, we would prefer that you choose another firmware or, better yet, make your own.
