Firmware
========

The Remora SPI firmware is available pre-compiled, or alternatively can be compiled using Mbed Studio. All features and modules are available in the pre-compiled version and are enabled and configured using the JSON configuration file "config.txt" on the controller board SD card.

There are several versions of the "firmware.bin" file. Each firmware is for its controller board matching the boards processor. Please ensure that you copy the correct version required for your controller board to the SD card.

.. code-block::

	Remora SPI
	└───Firmware
		└───FirmwareBin
			├───LPC1768
			│	└───firmware.bin
			├───LPC1769
			│	└───firmware.bin
			├───STM32F407
			│	└───BTT_F407
			│	│	└───firmware.bin
			│	└───MKS_MONSTER8
			│		└───firmware.bin
			├───STM32F429
			│	└───BTT_F429
			│		└───firmware.bin
			└───STM32F446
				└───BTT_F446
				│	└───firmware.bin
				└───FYSETC_SPIDER
					└───firmware.bin
			
			
.. toctree::
   :maxdepth: 2
   
   SD-Card-basics
   Setup-Config-File
   ethernet-config
