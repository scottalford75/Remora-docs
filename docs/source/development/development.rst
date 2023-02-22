Development
===========


SPI speed
---------

Using a ribbon cable the highst stable SPI speeds achieved are as follows:

+---------------+-------+--------+-----+
|               | RPi 3 | RPi 4  |     |
+---------------+-------+--------+-----+
| Core clock    | 400   | 500    | Mhz |
+---------------+-------+--------+-----+
| Clk Divider   | 64    | 64     |     |
+---------------+-------+--------+-----+
| SPI frequency | 6.25  | 7.8125 | Mhz |
+---------------+-------+--------+-----+

MbedOS6
--------
Remora currently uses the MBED5, MBED6 has more board support and better supports unmanaged bootloaders, but currently does not support the library needed to run the LPC based boards. 

Static Config aka no SD card
----------------------------

Several users have created their own branch of remora to use on dedicated hardware. which either has no SD card or no need for one. The user would be required to recompile remora with any changes needed to the pin configuration. This is still a work in progress and not 100% supported yet. 

Future Board Support
--------------------

Other boards could be supported in the future and there are several boards where support is in progress. If you have supported a new board, you can submit a pullrequest on github to add it to remora. Some boards are inprogress but stuck at one development stage or another. Boards currently *almost* supported but not complete include, the following and need work in these areas

- SKRv3 - STM32H743 - Bootloader, SDIO, and SPI DMA
- MANTA M8P - STM32G0B1 - SPI DMA MUX
- MKS ROBIN/MONSTER/SKIPR - SMT32F407 - Boodloader SPI conflict, issue with SPI SD on SPI3
