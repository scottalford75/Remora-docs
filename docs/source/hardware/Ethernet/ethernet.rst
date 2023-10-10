Remora Ethernet
------------

As the hard realtime requirements are offloaded onto the controller board, Remora can run on LinuxCNC based PC's via Ethernet.


Controller Boards
------------

Controller boards for the Remora Ethernet component are more hardware specific than the Remora SPI version. As a result, the firmware is featured accross several branches based on specific hardware. Controller boards  include:

* NVEM EC300 - an STM32F207 based board with Ethernet PHY chip, originally intended for Mach3
* NVEM EC500 - an STM32F407 based board with Ethernet PHY chip, originally intended for Mach3
* NVEM RT1052 - an iMXRT1052 based board with Ethernet PHY chip, originally intended for Mach3
* RP2040 W5500 DEV - a Raspberry Pi Pico RP2040 based dev board with on-board W5500 Ethernet SPI adapter
* Expatria Technologies PicoBOB DLX - a Raspberry Pi Pico RP2040 based dev board with on-board W5500 Ethernet SPI adapter designed for Remora
* Expatria Technologies  Flexi-HAL with uFlexiNET Ethernet adapter - an STM32F446 based board with W5500 Ethernet SPI adapter designed for Remora



.. toctree::
   :maxdepth: 2
   
   Flexi-HAL



