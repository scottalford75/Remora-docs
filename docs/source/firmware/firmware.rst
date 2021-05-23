Firmware
========

The Remora firmware is available pre-compiled, or alternatively can be compiled using Mbed Studio. All features and modules are available in the pre-compiled version and are enabled and configured using the Json configuration file "config.txt" on the controller board SD card.

There are two versions of the "firmware.bin" file. One for the LPC1768 and one for the LPC1769. Please ensure that use copy the correct version required for your controller board to the SD card.

.. code-block::

	Remora
	└───Firmware
		└───FirmwareBin
			├───LPC1768
			│	└───firmware.bin
			└───LPC1769
				└───firmware.bin