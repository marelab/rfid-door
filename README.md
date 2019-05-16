# ESP8266 RFID - Access Control with Wiegand Protocol powerd by Door Bell transformer 8-12V AC or DC

NFC/RFID WIFI Access Control hardware interfacing a cheap MFRC522, PN532 RFID, RDM6300 readers or Wiegand RFID readers with a Espressif's ESP8266 Microcontroller as Wifi Bridge to drive the door open coil to unlock a home door and to include it into a home automatisation system. 

![Showcase Gif](https://github.com/marelab/nfc-door/blob/master/grafics/nfc-door-V-1-1-3D.png)

## First of all 
big thanks to the ESP-RFID Project with the work they have done. The "nfc-door" hardware is 100% compatible with their software. Just the hardware design is differnt in my version. The nfc-door pcb is designed to be powered directly from the AC Ring Bell transformer (8-12V AC). But can also be configured to use a 8-12V DC power by just solder bridges. Main reason why I used a different design was the fact that I didn't wanted to setup a new DC source and cables for that device, it can be integrated without any extra power infrastructure. Most german households have a AC Ringbell with 8 or 12V transformer next to the door. 

## Differnces to the orignal design of the ESP-RFID Hardware:
* Can be powered by AC RingBell transformer directly no seperate power source need at the installation spot
* No Relais just Silicon Door coil is driven by smal opto mosfet
* Current start limitation of the door coil to save the mosfet and IC during current breakdown when the door coil get activated  
* 5V kompatible Wiegand DO/D1 Lines as default




Just for details about german standard and regulations for the bell transformer that has to be used. https://de.wikipedia.org/wiki/Klingeltransformator


## Features
### For Users
* Minimal effort for setting up your Access Control system, just flash and everything can be configured via Web UI
* Capable of managing up to 1.000 Users (even more is possible)
* Great for Maker Spaces, Labs, Schools, etc
* Cheap to build and easy to maintain
### For Tinkerers
* Open Source (minimum amount of hardcoded variable, this means more freedom)
* Using WebSocket protocol to exchange data between Hardware and Web Browser
* Data is encoded as JSON object
* Records are Timestamped (Time synced from a NTP Server)
* MQTT enabled
* Bootstrap, jQuery, FooTables for beautiful Web Pages for both Mobile and Desktop Screens
* Thanks to ESPAsyncWebServer Library communication is Asynchronous
### Official Hardware
* Small size form factor, sometimes it is possible to glue it into existing readers.
* Single power source to power 12V/2A powers ESP12 module, RFID Wiegand Reader and magnetic lock for opening doors.
* Exposed programming pins for ESP8266
* Regarding hardware design, you get multiple possible setup options:
* Forward Bell ringing on reader to MCU or pass it out of board
* Track Door Status
* Control reader’s status LED
* Control reader’s status BUZZER sound *
* Power reader, lock and the board through single 12V, 2A PSU
* Optionally power magnetic lock through external AC/DC PSU
* Possible to use any kind and any type of Wiegand readers
* Enables you to make IOT Access System with very little wiring
* Fits in an universal enclosures with DIN mount
* Open Source Hardware

## Getting Started
This project still in its development phase. New features (and also bugs) are introduced often and some functions may become deprecated. Please feel free to comment or give feedback.

* Get the latest release from [here.](https://github.com/marelab/nfc-door/releases)
* See Building [Building](https://github.com/marelab/nfc-door#building)
* See [Security](https://github.com/esprfid/esp-rfid#security) for your safety.
* See [ChangeLog](https://github.com/esprfid/esp-rfid/blob/dev/CHANGELOG.md)

### What You Will Need
### Hardware
* [Official ESP-RFID Relay Board](https://www.tindie.com/products/nardev/esp-rfid-relay-board-12v-in-esp8266-board/)
or
* An ESP8266 module or a development board like **WeMos D1 mini** or **NodeMcu 1.0** with at least **32Mbit Flash (equals to 4MBytes)** (ESP32 does not supported for now)
* A MFRC522 RFID PCD Module or PN532 NFC Reader Module or RDM6300 125KHz RFID Module Wiegand based RFID reader
* A Relay Module (or you can build your own circuit)
* n quantity of Mifare Classic 1KB (recommended due to available code base) PICCs (RFID Tags) equivalent to User Number

## **Building** 
Be carefull with 12V AC Ring bell transformers. The DC Voltage after the Diode bridge can be around 18V DC!!! That will not kill the NFC-DOOR hardware but it will kill a 12V RFID Reader that is connected to the NFC-DOOR. Why is that happening most Readers have a build in AMS1117 Voltage regulator wich has a maximum 16V input. The NFC-DOOR transforms the AC input Voltage in three stages first Stage is a diode bridge to transform AC to rippled DC then a Step Down Regulator generates 5V DC that can handle 24V DC input next the 5V is converted to 3,3V with a AMS1117 for the ESP. The NFC-DOOR won't get killed because the AMS1117 lives behind the 5V StepDown stage. The Rippled DC is connected directly to the power reader connectors. The easiest way to get around that problem is using a 8V AC Ringbell transformer. The rippled DC after the Diode Bridge is around 14-15V and in the range a AMS1117 of the Reader can handle.



The ACT4088 Step Down is a quite tiny part and its quite inpossible to indetify Pin1 without a mircoscope or strong lense. As hint thats a picture of the ACT4088 to find Pin1 as marked in the image.
![Showcase Gif](https://github.com/marelab/nfc-door/blob/master/grafics/act4088_oriantation.png)

### Software

#### Using Compiled Binaries
Download compiled binaries from GitHub Releases page
https://github.com/esprfid/esp-rfid/releases
On Windows you can use **"flash.bat"**, it will ask you which COM port that ESP is connected and then flashes it. You can use any flashing tool and do the flashing manually. The flashing process itself has been described at numerous places on Internet.

#### Building With PlatformIO
##### Backend
The build environment is based on [PlatformIO](http://platformio.org). Follow the instructions found here: http://platformio.org/#!/get-started for installing it but skip the ```platform init``` step as this has already been done, modified and it is included in this repository. In summary:

```
sudo pip install -U pip setuptools
sudo pip install -U platformio
git clone https://github.com/esprfid/esp-rfid.git
cd esp-rfid
platformio run
```

When you run ```platformio run``` for the first time, it will download the toolchains and all necessary libraries automatically.

##### Useful commands:

* ```platformio run``` - process/build all targets
* ```platformio run -e generic -t upload``` - process/build and flash just the ESP12e target (the NodeMcu v2)
* ```platformio run -t clean``` - clean project (remove compiled files)

The resulting (built) image(s) can be found in the directory ```/bin``` created during the build process.

##### Frontend
You can not simply edit Web UI files because you will need to convert them to C arrays, which can be done automatically by a gulp script that can be found in tools directory or you can use compiled executables at the same directory as well (for Windows PCs only).

If you want to edit esp-rfid's Web UI you will need (unless using compiled executables):
* NodeJS
* npm (comes with NodeJS installer)
* Gulp (can be installed with npm)

Gulp script also minifies HTML and JS files and compresses (gzip) them. 

In order to test your changes without flashing the firmware you can launch websocket emulator which is included in tools directory.
* You will need to Node JS for websocket emulator.
* Run ```npm update``` to install dependencies
* Run emulator  ```node wserver.js```
* then you will need to launch your browser with CORS disabled:
* ```chrome.exe --args --disable-web-security -–allow-file-access-from-files --user-data-dir="C:\Users\USERNAME"```

Get more information here: https://stackoverflow.com/questions/3102819/disable-same-origin-policy-in-chrome


### Pin Layout

The following table shows the typical pin layout used for connecting readers hardware to ESP:

| ESP8266 | NodeMcu/WeMos | Wiegand | PN532 | MFRC522 | RDM6300 | 
|--------:|:-------------:|:-------:|:-----:|:-------:|:-------:|
| GPIO-15 | D8            |         | SS    | SDA/SS  |         |
| GPIO-13 | D7            | D0      | MOSI  | MOSI    |         |
| GPIO-12 | D6            | D1      | MISO  | MISO    |         |
| GPIO-14 | D5            |         | SCK   | SCK     | TX      |
| GPIO-04 | D2            |         |       |         |         |
| GPIO-05 | D1            |         |       |         |         |

For Wiegand based readers, you can configure D0 and D1 pins via settings page. By default, D0 is GPIO-4 and D1 is GPIO-5

### Steps
* First, flash firmware (you can use /bin/flash.bat on Windows) to your ESP either using Arduino IDE or with your favourite flash tool
* (optional) Fire up your serial monitor to get informed
* Search for Wireless Network "esp-rfid-xxxxxx" and connect to it (It should be an open network and does not require password)
* Open your browser and type either "http://192.168.4.1" or "http://esp-rfid.local" (.local needs Bonjour installed on your computer) on address bar.
* Log on to ESP, default password is "admin"
* Go to "Settings" page
* Configure your amazing access control device. Push "Scan" button to join your wireless network, configure RFID hardware, Relay Module.
* Save settings, when rebooted your ESP will try to join your wireless network.
* Check your new IP address from serial monitor and connect to your ESP again. (You can also connect to "http://esp-rfid.local")
* Go to "Users" page
* Scan a PICC (RFID Tag) then it should glimpse on your Browser's screen.
* Type "User Name" or "Label" for the PICC you scanned.
* Choose "Allow Access" if you want to
* Click "Add"
* Congratulations, everything went well, if you encounter any issue feel free to ask help on GitHub.

#### Time
We are syncing time from a NTP Server (in Client -aka infrastructure- Mode). This will require ESP to have an Internet connection. Additionally your ESP can also work without Internet connection too (Access Point -aka Ad-Hoc- Mode),  without giving up functionality.
This will require you to do syncing manually. ESP can store and hold time for you approximately 51 days without a major issue, device time can drift from actual time depending on usage, temperature, etc.
So you have to login to settings page and sync it in a timely fashion.

## **Security**
We assume **ESP-RFID** project -as a whole- does not offer strong security. There are PICCs available that their UID (Unique Identification Numbers) can be set manually (Currently esp-rfid relies only UID to identify its users). Also there may be a bug in the code that may result free access to your belongings. And also, like every other network connected device esp-rfid is vulnerable to many attacks including Man-in-the-middle, Brute-force, etc.

This is a simple, hobby grade project, do not use it where strong security is needed.

What can be done to increase security? (by you and by us)

* We are working on more secure ways to Authenticate RFID Tags.
* You can disable wireless network to reduce attack surface. (This can be configured in Web UI Settings page)
* Choose a strong password for the Web UI

## Scalability
Since we are limited on both flash and ram size things may get ugly at some point in the future. You can find out some test results below.

## License
The code parts are licensed under [MIT License](https://github.com/esprfid/esp-rfid/blob/stable/LICENSE), 3rd party libraries that are used by this project are licensed under different license schemes, please check them out as well.
