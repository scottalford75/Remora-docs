Remora Ethernet
========

The latest Remora supported controller boards using Ethernet communications allows LinuxCNC to be run on any supported PC.


Controller Boards
------------

Controller boards for the Remora Ethernet component are more hardware specific than the Remora SPI version. As a result, the firmware is featured accross several branches based on specific hardware. Controller boards include:

**STM32 based controller boards**

* NVEM - an STM32F207 based board with Ethernet PHY chip, originally intended for Mach.  *[No longer in production, Legacy Support - no new features]*
* EC500 - an STM32F407 based board with Ethernet PHY chip, originally intended for Mach3.  *[No longer in production, Legacy Support - no new features]*
* Expatria Technologies  Flexi-HAL with uFlexiNET Ethernet adapter - an STM32F446 based board with W5500 Ethernet SPI adapter designed for Remora


**iMX RT1052 based controller boards**

* NVEM, EC300 & EC500 - iMXRT1052 based controller boards with Ethernet PHY chip, originally intended for Mach3. [In active development]


**RP2040 based controller boards**

* RP2040-W5500-DEV a Raspberry Pi Pico RP2040 based development board with on-board Wiznet W5500 Ethernet SPI adapter
* Expatria Technologies PicoBOB DLX - a Raspberry Pi Pico RP2040 based dev board with on-board W5500 Ethernet SPI adapter designed for Remora
