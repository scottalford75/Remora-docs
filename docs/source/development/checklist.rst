SETUP CHECKLIST
===================

This page is meant to serve as a quick checklist of requirments in order to get a working Remora setup. All these items have dedicated, detailed sections in the Remora documents. 

HARDWARE
--------

- Connect your Remora Hardware to your LinuxCNC computer (Ethernet) or RaspberryPi (SPI or Ethernet)
- Optional, connect your Remora hardware over UART for debug information

SOFTWARE
-------------------------

- Install LinuxCNC Premade Image on your PC or RPi 
- Install Remora on LinuxCNC :
    -  RaspberryPi (Remora-spi or Remora-eth) 
    - regular PC (Remora-eth)

- Configure Remora/LinuxCNC Hal File

FIRMWARE/CONFIGURATION
------------------------

- Load Remora Firmware on to your Remora Hardware
- Configure Remora Firmware config.txt file
- Load your config.txt file to Remora 