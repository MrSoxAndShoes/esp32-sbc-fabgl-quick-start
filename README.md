# Quick Start Guide to the ESP32-SBC-FabGL
A quick-start guide to setting up the **FabGL PCEmulator** example for the [**Olimex ESP32-SBC-FabGL**](https://www.olimex.com/Products/Retro-Computers/ESP32-SBC-FabGL/open-source-hardware).

## Purpose

![My Olimex ESP32-SBC-FabGL board](/assets/images/olimex-esp32-sbc-fabgl.jpg).

I'm a complete novice to Arduino and Raspberry Pi development, so I wrote down these steps to help myself and others get quickly started on using/playing with the board.

Explanations are not included, just the steps.

## What I Have Installed

At the time of this writing,

- Arduino IDE 2.1.1
- Espressif 2.0.11
- Olimex FabGL 1.0.9

## Setting up the Arduino environment

1. Install and start [**Arduino IDE**](https://www.arduino.cc/en/software).
2. Add the esp32 boards by [**Espressif**](https://github.com/espressif/arduino-esp32) (Tools > Board > Board Manager and search for "esp32").
3. The Olimex fork of FabGL adds support for the ESP32-SBC-FabGL board and needs to be installed as a local library. *Unfortunately, you'll need to uninstall Fabrizio's FabGL library if it's already installed.*
    - Go to the [**Olimex FabGL**](https://github.com/OLIMEX/FabGL) repo on GitHub.
    - Click "Download ZIP" from the green "Code" drop-down button. I saved the ZIP file as "Olimex-FabGL.zip" so I didn't get it confused with Fabrizio's repo.
        > ![Downloading the ZIP file from the repo](/assets/images/download-zip.png)
    - Unzip and copy the contents to a new folder in the "Documents\Arduino\libraries" folder (e.g. "C:\Users\username\Documents\Arduino\libraries\Olimex-FabGL").
4.	Close Arduino IDE. The next time the IDE is started, the local Olimex FabGL library will be available for use.

## Setup the SD card

1. Format an SD card as FAT32. For SD cards larger than 32GB (e.g. 64GB, 128GB, etc.), a 32GB (or smaller) partition will need to be created.
2. Download the boot image files found in [`/examples/VGA/PCEmulator/mconf.h`](https://github.com/OLIMEX/FabGL/blob/master/examples/VGA/PCEmulator/mconf.h) and copy to the root folder of the SD card. This saves having to repeatedly download a boot image from the FabGL website whenever the PCEmulator is started.
3. Copy the [`mconfs.txt`](/assets/mconfs.txt) file to the root folder of the SD card. The source for this file was created from the `DefaultConfFile` variable also found in [`/examples/VGA/PCEmulator/mconf.h`](https://github.com/OLIMEX/FabGL/blob/master/examples/VGA/PCEmulator/mconf.h), removing C-code constructs, and changing the boot image URLs to local file names.
4. There are many ways to create or add custom boot images, whether it be a floppy disk or a hard drive. My suggestion would be to copy one of the existing image files edit it using [**WinImage**](https://www.winimage.com/download.htm). To make it usable from FabGL,
    - Copy the boot image to the SD card.
    - Add an entry to it in "mconfs.txt".
    - Specify the media type ("fd0" or "hd0") and add "boot hd0" if booting a hard drive image.

## Compiling and Deploying PCEmulator from the Arduino IDE

1. Before starting the Arduino IDE, connect the ESP32-SBC-FabGL board to your desktop PC using a USB cable. The USB-C port on the board serves as both power and data.
2. Start Arduino IDE.
3. Verify the Olimex FabGL library is loaded by,
    - Opening the Library Manager (Tools > Manage Libraries).
    - Typing "FabGL" in to the "filter" textbox.
    - Changing the "Type" to "Installed".
    - It should list "FabGL 1.0.9" (at the time of this writing) as one of the installed libraries.
4. Select the FabGL PCEmulator project (File > Examples > FabGL > VGA > PCEmulator). The FabGL examples will be at the bottom of a lengthy list.
5. Configure the board.
    - Select the "ESP32 Dev Module" board (Tools > Board > esp32 > ESP32 Dev Module).
    - Set the board port to upload to (Tools > Port > COM#).
    - Set the partition scheme (Tools > Partition > Huge APP).
    - Disable PSRAM (Tools > PSRAM > Disabled).
    - Set the Upload Speed (Tools > Upload Speed > 921600).
        - If an upload error occurs, lower the transfer speed.
6. Edit "PCEmulator.ino" project file.
    - The wireless SSID and password can be preset in the "tryToConnect()" function.
    - Time zone can be preset in the "updateDateTime()" function with your [locale](https://github.com/nayarsystems/posix_tz_db/blob/master/zones.csv).
    - I would not recommend it but PCEmulator can format the SD card by uncommenting the "FileBrowser::format()" method.
7. Compile and upload to the board (Sketch > Upload).
    - If all goes well, the ESP32-SBC-FabGL board will reboot after the compilation and upload complete.
    - If the wireless parameters were not set, a prompt asking to configure the wireless connection will appear.
    - After that, the boot menu should display.

Copyright (C) 2023 Erik Anderson

**Subjects**: BASIC, IBM PC, Nostalgia, Programming, Video Games, Vintage Computers, Windows
**Tags**: Arduino, Emulators, ESP32, FabGL, Olimex
