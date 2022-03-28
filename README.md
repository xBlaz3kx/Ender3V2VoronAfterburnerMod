# Ender 3 V2 Voron Afterburner Mod

## About

This project is a modification of the Ender 3 V2, replacing the original print head with
a [Voron Afterburner](https://github.com/VoronDesign/Voron-Afterburner). All the parts needed for this modification are
listed in [BOM](#bill-of-materials). The part of the mod is changing the firmware
to [Klipper](https://www.klipper3d.org/) instead of Marlin, which is optional and can be done separately.

**_Disclaimer: Follow these instructions at your own risk._**

## Bill of materials

| Item | # |
|   :---:    |   :---: | 
| Genuine or clone Micro Swiss hotend | 1 |
| Genuine or clone BMG extruder or it's parts | 1 |
| Filament (PETG or ABS)| around 150g |
| Raspberry Pi 3 or 4 | 1 |
| Power supply for RPi 3/4 | 1 |
| 4020 blower/radial fan | 1 |
| 4010 axial fan | 1 |
| Heat inserts M3 | 11 |
| M3x40 screws | 4 |
| M3x30 screws | 5 |
| M3x20 screws | 2 |
| M3x16 screws | 2 |
| M3x12 screws | 2|
| M3x8 screws | 4 |
| M3 spacers |*4|
| M4 spacers | *1 |
| 24 AWG wire | around 2m  |
| *Optional: BTT SKR Mini E3 V2 | 1 |

## Printing and assembly

### Tools required

1. Soldering Iron
2. Cutters
3. Allen keys
4. Crimpers
5. 3D printer

### Printing the parts

It is recommended to print the hotend assembly parts with temperature-resistant materials such as PETG or ABS. Voron
design team recommends using primary and accent color (for more information, check out
their [docs](https://docs.vorondesign.com/)). They recommend printing the parts with 40% infill and 4 outer walls.

### Assembly

_Detailed assembly instructions coming soon.._

## Klipper installation and configuration

### Installing Fluidd

Installing [Fluidd](https://github.com/cadriel/fluidd), the Klipper WebUI, is fairly straightforward:

Download the Raspberry Pi image [here](https://github.com/cadriel/FluiddPI/releases/latest)
or clone the [repository](https://github.com/cadriel/fluidd) onto an existing OS. After downloading, flash the image to
a SD card. Insert the card into the Raspberry Pi, and you're set to go.

There are instances of running Fluidd and Klipper in Docker, so you could also try that.

### Klipper

#### Installation

To install Klipper:

    ```
    cd klipper/
    make menuconfig
    ```

*This step is for SKR Mini E3 V2*

1. Select "_Enable extra-low level configuration_"
    - STM32F1
    - STM32F103
2. Bootloader Offset: 28KiB bootloader
3. USB for communication
4. Enter the USB ID. You can find them by `lsusb -v`. Vendor ID should be `0x1eaf`, USB Device ID should be `0x0004` and USB
   Serial Number is the "iSerial" from lsusb -v, so 0.
5. Put !PA14 into the GPIO Pins
6. Save the config

Then compile the firmware:

```
make clean #use when updating firmware!
make
```

When compiled, copy and rename the **klipper.bin** file in _/klipper/out/_ folder to **firmware.bin** (check your board
for naming convention) and copy it to an SD card. Turn off the printer and insert the card. Turn on the printer. The
firmware should be flashed.

Then find the USB ID:

``` 
 ls /dev/serial/by-id/*
```
Change the serial in **printer.cfg**:

```
   [mcu]
   serial: /dev/serial/by-id/usb-1a86_USB2.0-Serial-if00-port0
```

#### Configuring Klipper

Since I've got a BTT SKR Mini E3 V2 board, I copied the configuration I found for this board and modified it to suit my
printer. The configuration file is attached and there are marks where any additional changes are required.

Copy the printer.cfg to _klipper_config/_.

## Helpful docs:

- [Calibration guide](https://teachingtechyt.github.io/calibration.html)
- [Voron docs](https://docs.vorondesign.com/)
- [Klipper installation from Reddit](https://www.reddit.com/r/BIGTREETECH/comments/cxjiwx/skr_mini_e3_any_help_with_klipper/ezp5oiw?utm_source=share&utm_medium=web2x&context=3)


