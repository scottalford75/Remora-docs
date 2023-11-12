Installing the Remora component
================================

The Remora component is the SPI interface between LinuxCNC and the LPC17xx/STM32F4 controller board as well as being the LinuxCNC side of the step pulse generators. The Remora Linuxcnc install contains several seperate components. You can install them all, or only what you need. The components are found in the Remora Repository under 

Remora/LinuxCNC/Components/ :

- remora_rpspi.c :  Raspberry Pi SPI driver for STM32 based boards
- remora_lpc.c   : Raspberry Pi SPI driver for LPC17xx based boards
- remora_eth.c   : Ethernet Driver for Remora Ethernet Firmware 
- PRUencoder.c   : Remora Encoder driver 
- PIDcontroller.c : Remora PIDcontroller for temperature control
- NVMPG.c         : Driver for the NVEM serial pendant 


To install a Remora component:

1. Open a terminal window , download the Remora repository and change to the Remora component directory

.. code-block::

    pi@raspberry:~ $ mkdir ~/linuxcnc
    pi@raspberry:~ $ cd ~/linuxcnc
    pi@raspberry:~ $ git clone https://github.com/scottalford75/Remora
    pi@raspberry:~ $ cd Remora/LinuxCNC/Components
    	


2. Install the component using halcompile

.. code-block::


    pi@raspberry:~ $ sudo halcompile --install ./Remora-eth/remora_eth.c
    pi@raspberry:~ $ sudo halcompile --install ./Remora-rpspi/remora_rpspi.c
    pi@raspberry:~ $ sudo halcompile --install ./Remora-rpspi/remora_lpc.c
    pi@raspberry:~ $ sudo halcompile --install ./NVMPG/nvmpg.c
    pi@raspberry:~ $ sudo halcompile --install ./PIDcontroller/PIDcontroller.c
    pi@raspberry:~ $ sudo halcompile --install ./PRUencoder/PRUencoder.c
