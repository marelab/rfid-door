![marelab rfid-door](https://github.com/marelab/rfid-door/blob/master/grafics/marelab-rfid-door-logo.png)
# ESP8266 RFID - Access Control with Wiegand Protocol powerd by Door Bell transformer 8-12V AC or DC

## First of all 
big thanks to the [ESP-RFID Project](https://github.com/esprfid/esp-rfid) with the work they have done. The "rfid-door" hardware is 100% compatible with their software also a modified [Firmware](https://github.com/marelab/esp-rfid/releases) with extended MQTT support is included with the marelab RFID-DOOR Version that can be used. Just the hardware design is differnt in my version. The RFID-DOOR pcb is designed to be powered directly from the AC Ring Bell transformer (8-12V AC). But can also be configured to use a 8-12V DC power by just solder bridges. Main reason why I used a different design was the fact that I didn't wanted to setup a new DC source and cables for that device, it can be integrated without any extra power infrastructure. Most german households have a AC Ringbell with 8 or 12V transformer next to the door. 

## RFID-DOOR
NFC/RFID WIFI Access Control hardware interfacing a cheap MFRC522, PN532 RFID, RDM6300 readers or Wiegand RFID readers with a Espressif's ESP8266 Microcontroller as Wifi Bridge to drive the door open coil to unlock a home door and to include it into a home automatisation system. The pcb is designed with [KiCad](http://kicad-pcb.org/) an opensoure EDA System. Ready Gerber Files can be found under Gerber. Plz use the Version Tab for download the newest Release the Master Branch includes the current development stage! 

![Board](https://github.com/marelab/rfid-door/blob/master/grafics/rfid-door-pcb2.png) ![logo](https://github.com/marelab/rfid-door/blob/master/grafics/pure-rfid-door-logo.png)

## Differnces of the ESP-RFID Hardware to RFID-DOOR Hardware
The marelab RFID-DOOR hardware: 
```
* Can be powered by AC RingBell transformer directly no seperate power source need at the installation spot
* No mechanical Relais just Silicon Door coil is driven by smal opto mosfet
* Currentlimitation of the door coil to limit the current breakdown while magentic lock is activated  
* 5V kompatible Wiegand DO/D1 Lines as default
```

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
* MQTT enabled for using by Smart Home automatisation.
* Bootstrap, jQuery, FooTables for beautiful Web Pages for both Mobile and Desktop Screens
* Thanks to ESPAsyncWebServer Library communication is Asynchronous
### Official Hardware
* Small size form factor just 4,5cm * 4,5 cm
* Single power source (Ring Bell transformer) for RFID-DOOR hardware & RFID Wiegand Reader and magnetic lock for opening doors.
* Exposed programming pins for ESP8266
* Regarding hardware design, you get multiple possible setup options:
* Forward Bell ringing on reader to MCU or pass it out of board
* Control reader’s status LED
* Control reader’s status BUZZER sound *
* Power reader, lock and the board through single 8V, AC Ring Bell Transformer
* Power for Magnetic Lock / Door Relais is supplied by RFID-DOOR Hardware
* Possible to use any kind and any type of Wiegand readers
* Smart Home integration over MQTT
* Fits in an universal enclosures with DIN mount
* Open Source Hardware


## Getting Started
New features (and also bugs) are introduced often and some functions may become deprecated. Please feel free to comment or give feedback.

### Standard Door Bell Setup
![Showcase Gif](https://github.com/marelab/rfid-door/blob/master/grafics/basicSetup.png)

### RFID-DOOR integrated into Door Bell Setup
![Showcase Gif](https://github.com/marelab/rfid-door/blob/master/grafics/rfidSetup.png)



## Hardware you need

### Hardware
* [marelab RFID-DOOR Board](https://www.marelab.org/index.php/smarthome-iot/rfid-door) can be ordered here
* A MFRC522 RFID PCD Module or PN532 NFC Reader Module or RDM6300 125KHz RFID Module Wiegand based RFID reader
* n quantity of Mifare Classic 1KB (recommended due to available code base) PICCs (RFID Tags) equivalent to User Number
* A door Relay 8-12V
* A Ringbell Transformer 8-12V 

### Software you need
* Get the latest hardware release from [here.](https://github.com/marelab/rfid-door/releases)
* Get the firmware software from [here.](https://github.com/marelab/esp-rfid/releases)
* See Building [Building](https://github.com/marelab/rfid-door#building)
* See [Security](https://github.com/marelab/esp-rfid#security) for your safety.
* See [ChangeLog](https://github.com/marelab/esp-rfid/blob/dev/CHANGELOG.md)

### Setup & get it running 
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

### RFID-DOOR Connectors
![Showcase Gif](https://github.com/marelab/rfid-door/blob/master/grafics/rfid-door-con.png) 
 
| Number | Function                                     | ESP8266           |
|-------:|:--------------------------------------------:|:------------------|
| 1      | DoorR / Magnetic Look / Door Relais Conector |    -              |
| 2      | DoorR / Magnetic Look / Door Relais Conector |    -              |
| 3      | 12V out / 12V+ DC                            |    -              |
| 4      | D0 Wiegand D0                                | GPIO-04*          |
| 5      | D1 Wiegand D1                                | GPIO-05*	        |
| 6      | LED                                          | GPIO-05           |
| 7      | W26                                          | GPIO-05           |
| 8      | BUZ                                          | GPIO-05           |
| 9      | GND                                          |    -              |
| 10     | GND                                          |    -              |
| 11     | 12V AC                                       |    -              |
| 12     | 12V AC                                       |    -              |
| 13     | JP4 Reset                                    | GPIO-05           |
| 14     | JP5 Firmware Upload enable                   | GPIO-05           |
|        | JP3-1 3,3V Serial Connector to ESP8266       |                   |
|        | JP3-2 TX Serial Connector to ESP8266         | GPIO-05           |
|        | JP3-3 RX Serial Connector to ESP8266         | GPIO-05           |
|        | JP3-4 GND Serial Connector to ESP8266        | GPIO-05           |

(*) 5V In/Output Signal

### RFID-DOOR Functional Blocks
![Showcase Gif](https://github.com/marelab/rfid-door/blob/master/grafics/rfid-door-V1-1-top-overview.png)

| Letter | Hardware Function Block                                                                                             | 
|-------:|:-------------------------------------------------------------------------------------------------------------------:|
| A      | Switches the Magnetic Lock/Door Relais by Opto Mosfet and 2W Magnetic Lock Resistor that limits starting current    |
| B      | 3,3V Linear Driver for ESP8266 supply                                                                               | 
| C      | Diode Bridge & Self Resetable Fuse for converting AC to DC Ripple power. The Cxx Cap is decoupled by a the Diode Dxx if the 12V of the Bridge falls down for ms while the magnetic lock is used it delivers the energy for the 5V stage  | 
| D      | 5V StepDown Converter that powers (B) and the MosFet for (E)                                                        | 
| E      | In / Out 3,3V Signal Lines ESP to 5V on Conector 



## **Building** & Setup Hardware

#### The AC Ring Bell Transformer
Be carefull with 12V AC Ring bell transformers. The DC Voltage after the Diode bridge can be around 18V DC!!! That will not kill the RFID-DOOR hardware but it will kill a 12V RFID Reader that is connected to the RFID-DOOR. Why is that happening most Readers have a build in AMS1117 Voltage regulator wich has a maximum 16V input. The RFID-DOOR transforms the AC input Voltage in three stages first Stage is a diode bridge to transform AC to rippled DC then a Step Down Regulator generates 5V DC that can handle 24V DC input next the 5V is converted to 3,3V with a AMS1117 for the ESP. The RFID-DOOR won't get killed because the AMS1117 lives behind the 5V StepDown stage. The Rippled DC is connected directly to the power reader connectors. 
The easiest way to get around that problem is using a 8V AC Ringbell transformer. For example someting like this:
![Showcase Gif](https://github.com/marelab/rfid-door/blob/master/grafics/klingeltrafo.jpg)
Just use the 8V instead of 12V. The rippled DC after the Diode Bridge is around 14-15V and in the range a AMS1117 of the Reader can handle.
Just for details about german standard and regulations for the bell transformer that has to be used. https://de.wikipedia.org/wiki/Klingeltransformator

####  Placing the ACT4088 IC
The ACT4088 Step Down is a quite tiny part and its quite inpossible to indetify Pin1 without a mircoscope or strong lense. As hint thats a picture of the ACT4088 to find Pin1 as marked in the image.
![Showcase Gif](https://github.com/marelab/rfid-door/blob/master/grafics/act4088_oriantation.png)

#### Cad drawing 
![Showcase Gif](https://github.com/marelab/rfid-door/blob/master/grafics/rfid-door-cad.png)
The Encloser drill holes dimension is 2,5 mm

#### Special Configuration solder bridges
The board can be customized for special needs if you don't want or have a standard configuration. Some solder Bridges see the pictures of the PCB Back & Top where to find them can configure direct 12V DC Input and or disable the 5V Stage for the D0 / D1 pins. 

![RFID-DOOR back PCB](https://github.com/marelab/rfid-door/blob/master/grafics/nfc-door-V1-1-back-pcb.png) ![RFID-DOOR top PCB](https://github.com/marelab/rfid-door/blob/master/grafics/rfid-door-V1-1-top-pcb.png)

!!!Warning all described here might disable some safety features of the board so you should totaly know what you are doing using this!!!

```
##### 12V DC Power Input
If your power source is 12V DC you don't need the Diode Bridge and the 
Capacitor Cxx to be soldered on the board. Solder the JP1 and JP2 
Solder Bridge Jumpers  
```

```
##### 3,3V DO / D1 Wiegand Interface
If you don't need a 5V TTL compatble In/Out of the D1/D0 Lines just 
solder the Solder Bridges JP7/JP8. Don't solder then the Mosfets Q1 & 
Q2 as well as the four Resistors Rxx,Rxx,Rxx,Rxx. 
```

```
##### Bridge the Magnetic Lock / Door Relais current limitation 
By solder the JP3 Bridge the R1 2W Resistor is bridged and so no current 
limitation of a starting magentic coil / Door Lock saves the MosFet!!! 
The R1 Resitor don't need to be solderd anymore. 
```


## Software

#### Wich firmware should I use?
If you want to have a smart home integration or want to manage more then one Door I would recomend the marelab version, because of the extended MQTT functions and multi device support.
Otherwise you can use also the orginal firmware from the ESP-RFID Project. 

#### Using Compiled Binaries
Download compiled binaries from GitHub Releases page:
marelab modified firmware with extended MQTT support https://github.com/marelab/esp-rfid/releases 
or the orignal firmware from https://github.com/esprfid/esp-rfid/releases

#### Flashing the device
For a selfmade NF-DOOR pcb you need a USB to serial Bridge to flash the first Version. New Firmware Versions can then be updated by OTA (Over the Air Wifi Update) done with the web gui.
If you bought the RFID-DOOR Hardware the ESP is already programmed with the firmware so you don't need the USB to serial adapter for flashing.
On Windows you can use **"flash.bat"**, it will ask you which COM port that ESP is connected and then flashes it. You can use any flashing tool and do the flashing manually. The flashing process itself has been described at numerous places on Internet.

#### Self building With PlatformIO
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

##### Debug information
When you compiling and flashing a debug Version the ESP sends Debug Messages over the Serial Port thats usefull for trace and test purpose.

##### Useful commands:

* ```platformio run``` - process/build all targets
* ```platformio run -e generic -t upload``` - process/build and flash just the ESP12e target 
* ```platformio run -e debug -t upload``` - process/build and flash just the ESP12e target 
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

## Time
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
