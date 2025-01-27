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


Static Config aka no SD card
----------------------------

Several users have created their own branch of remora to use on dedicated hardware. which either has no SD card or no need for one. The user would be required to recompile remora with any changes needed to the pin configuration. This branch does not have 100% of modules supported.  It can be found under the branch static_config

Ethernet Support
----------------

The STM32/RP2040 W5500 ethernet branch is in a functioning state, but it needs some work. Modules need to be added, such as PWM and encoders, and aresa need polishing

Future Board Support
--------------------

Other boards could be supported in the future and there are several boards where support is in progress. If you have supported a new board, you can submit a pullrequest on github to add it to remora. Some boards are inprogress but stuck at one development stage or another. Boards currently *almost* supported but not complete include, the following and need work in these areas

- SKRv3 - STM32H743 - Bootloader, SDIO, and SPI DMA
- MANTA M8P - STM32G0B1 - SPI DMA MUX

