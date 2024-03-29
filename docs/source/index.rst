Welcome to Remora's  documentation
================================================

.. note::
   The documentation is currently being updated to include latest developments
   including the Ethernet capable controller boards.

Remora is a free, opensource LinuxCNC component and Programmable Realtime Unit (PRU) firmware to allow STM32, iMX RT1052 and RP2040 compatible controller boards to be used with LinuxCNC.

The documentation is divided into sections specificly relating to:

* **Introduction** - why Remora? What it is and how LinuxCNC works with an inexpensive controller board
* **Hardware** - connecting a Raspberry Pi to compatible controller boards
* **Software** - installing, setting up and configuring LinuxCNC on a Raspberry Pi to use the Remora component
* **Firmware** - installing the Remora firmware
* **Configuration** - configuring the Remora firmware
* **Development** - contributing to the development of the Remora firmware and LinuxCNC component

**Proposed reorganisation of repository**

.. code-block::

	Remora
	│
	└─── Remora SPI
	│	 └─── Firmware
	│	 └─── Config Samples
	│
	└─── Remora Ethernet
	│	 └─── Firmware
	│	 └─── Config Samples
	│	
	└─── LinuxCNC
		 └─── Components
		 └─── Config Samples

.. toctree::
   :maxdepth: 2
   :hidden:
   :caption: Introduction
   
   /intro/intro
   

.. toctree::
   :maxdepth: 2
   :hidden:
   :caption: Hardware
   
   /hardware/hardware


.. toctree::
   :maxdepth: 2
   :hidden:
   :caption: Software
   
   /software/software

.. toctree::
   :maxdepth: 2
   :hidden:
   :caption: Firmware

   /firmware/firmware
   
.. toctree::
   :maxdepth: 2
   :hidden:
   :caption: Configuration

   /configuration/configuration

.. toctree::
   :maxdepth: 2
   :hidden:
   :caption: Development Guide
   
   /development/development
