# CODESYS V3 Library for Monarco HAT

![Alt text](http://pigeoncomputers.com/wp-content/uploads/2017/08/Codesys_Logo_250px.png "CODESYS")
![Alt text](https://www.robotshop.com/media/files/images2/monarco-board-aluminum-housing-desc_image-1.jpg "Monarco HAT")

The Monarco HAT is a robust industrial graded HAT, perfectly suited for IOT projects, small home-automation or industrial projects and much more ... It protects your Raspberry Pi from overvoltage or short-circuiting and simultaneously provides you with enough IO channels and channel configuration versatility. 

A CODESYS V3 library was missing, so I wrote one to fill the gap after studying the C & Java Node-JS code examples and documentation provided by Monarco. Though tested, it will probably still contain bugs =( 
If you spot a bug, share it so we can fix it.

As the CODESYS runtime for Raspbery Pi also contains a very good responsive HTML5 compatible webinterface (via a built-in webserver in the CODESYS runtime itself), no extra software like OpenHAB or Domoticz is neccesary. Offcourse, you can connect to any external platform if needed. 

For the record, I am not connected with Monarco or CODESYS in any way.

# V2.0.0.1 library info
- Implemented as CODESYS 3 IO device-driver;
   - 100% Open source,
   - 100% Pure IEC 61131-3 code (ST),
- No function block calls in your software necessary, just write code, attach variable to an I/O channel, ready! 
- Breaks compatibility with earlier FB version but improves highly on ease of use,
   - 4 Di,
   - 4 Do,
   - 2 Ai,
   - 2 Ao,
   - Hardware Watchdog,
   - Control Byte (experimental).
- Stable, but work in progress ...

# Missing in action
At this moment all functionality is implemented into the core of the driver (as FB), but not yet routed to the user as parameters;
- Not yet routed to parameters are;
   - RS485 configuration, 
   - DO channels PWM configuration,
   - DI channels Counter configuration,
   - 1-Wire bus.

# Previous versions info
https://github.com/Aliazzzz/Monarco-HAT-library-for-CODESYS-V3/blob/master/VERSIONS.md

# The main software components
- .package file (CODESYS package installer) which installs the necessary files in a user friendly manner;
   - Hardware device description file,
   - Software library file,
   - Example project file, 
   - A copy of the "unlicense" agreement...

# Hardware prerequisitories
- A Raspberry Pi, see https://www.raspberrypi.org/
- A Monarco HAT, see https://www.monarco.io/
- 8 GB SD Card ...

# Software prerequisitories
- CODESYS V3 IDE available at https://store.codesys.com/codesys.html,
- CODESYS Raspberry Pi .package available at https://store.codesys.com/codesys-control-for-raspberry-pi-sl.html,
- Codesys Raspberry Pi SL Demo or License (The Demo is *unrestricted in technical capabilities* but will only run for two hours straight, after which it stops and you have to set it in run again yourself by logging in)
- Latest Monarco IO Hat library for CODESYS package avilable at https://github.com/Aliazzzz/Monarco-HAT-library-for-CODESYS-V3/tree/master/Monarco/2.0.0.1/package

# Hardware installation
Attach Monarco HAT to Raspberry Pi and power it up; 

    sudo cat /proc/device-tree/hat/vendor
    
should return "REX Controls". 

    sudo cat /proc/device-tree/hat/product

should return "Monarco HAT".

# Disable system console on UART

    sudo sed 's/ console=serial0,[0-9]\+//' -i /boot/cmdline.txt
    sudo reboot
    
# Install essential tools
    
    sudo apt update
    sudo apt install git

# Flash Monarco HAT EEPROM (to avoid manual installation of overlay)

    sudo git clone https://github.com/monarco/monarco-hat-firmware-bin
    cd monarco-hat-firmware-bin
    ./monarco-eeprom.sh update

# Install CODESYS runtime component on pi (Demo or licensed)
Follow Codesys online help steps, its easy!

https://help.codesys.com/webapp/_rbp_install_runtime;product=CODESYS_Control_for_Raspberry_Pi_SL;version=3.5.12.0


# Monarco codesys package installation
Either install the Monarco codesys package via;
    Double-click the package or 
    via CODESYS IDE Package Manager or
    Install the loose components via the Library / Device Repository, found under Tools menu option in CODESYS IDE.

# Attach CODESYS to the RS485 UART
Switch to etc direcory and edit the CODESYSControl.cfg;

    cd etc/
    sudo nano CODESYSControl.cfg

Add the following lines;
    
    [SysCom] 
    Linux.Devicefile=/dev/ttyAMA

Now save and Quit nano. 

# Do a forced NTP sync
    sudo timedatectl

This will force to sync time with some time server and returns something like, depending on date/time and your time-zone;

         Local time: zo 2018-09-23 14:46:17 CEST
         Universal time: zo 2018-09-23 12:46:17 UTC
         RTC time: zo 2018-09-23 12:46:18
         Time zone: Europe/Amsterdam (CEST, +0200)
         Network time on: yes
         NTP synchronized: yes
         RTC in local TZ: no


# Ready! But wait?

Now you can use the HAT, RS-485 and the Real-Time Clock from within a CODESYS IEC application!
Access the RS485 UART via a comlib of you own flavour in CODESYS (like CAA SerialCOM library).


# Run CODESYS Project
Open Codesys and the provided example project file.

Check/Set SPI master parameters:

    Mode 0,
    SPI bits 8,
    Speed(Hz) 1000000 (=1MHz) => can be set up to 4 MHz, slower speeds avoid chance on crc errors
    
Select your Target, Compile, download and run

# Enjoy!
