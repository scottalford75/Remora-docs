Installing the Remora component
================================

The Remora component is the interface between LinuxCNC and the controller board 
as well as being the LinuxCNC side of the step pulse generators. 
The Remora Linuxcnc install contains several seperate components. 
You can install them all, or only what you need. The components are 
found in the Remora Repository under 

Remora/LinuxCNC/Components/ :

- remora-spi.c :  Raspberry Pi3/4/5 SPI driver for STM32 based boards
- remora_lpc.c   : Raspberry Pi4 SPI driver for LPC17xx based boards
- remora-eth-3.0.c   : Ethernet Driver for Remora Ethernet Firmware 
- PRUencoder.c   : Remora Encoder driver 
- PIDcontroller.c : Remora PIDcontroller for temperature control
- NVMPG.c         : Driver for the NVEM serial pendant, the NVMPG 


To install a Remora component:

1. Open a terminal window , download the Remora repository and change to the Remora component directory

.. code-block::

    pi@raspberry:~ $ mkdir ~/linuxcnc
    pi@raspberry:~ $ cd ~/linuxcnc
    pi@raspberry:~ $ git clone https://github.com/scottalford75/Remora
    pi@raspberry:~ $ cd Remora/LinuxCNC/Components
    	


2. Install the component using halcompile

.. code-block::


    pi@raspberry:~ $ sudo halcompile --install ./Remora-eth/remora-eth-3.0.c
    pi@raspberry:~ $ sudo halcompile --install ./Remora-spi/remora-spi.c
    pi@raspberry:~ $ sudo halcompile --install ./Remora/remora_lpc.c
    pi@raspberry:~ $ sudo halcompile --install ./NVMPG/nvmpg.c
    pi@raspberry:~ $ sudo halcompile --install ./PIDcontroller/PIDcontroller.c
    pi@raspberry:~ $ sudo halcompile --install ./PRUencoder/PRUencoder.c
