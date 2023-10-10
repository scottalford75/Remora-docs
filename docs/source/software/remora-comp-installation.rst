Installing the Remora component
================================

The Remora component is the SPI interface between LinuxCNC and the LPC17xx/STM32F4 controller board as well as being the LinuxCNC side of the step pulse generators. The Remora Linuxcnc component contains 2 seperate components, remora.c is for STM32 based boards and remora_lpc.c is for LPC17xx based boards. The Remora Ethernet component is named remora-nv.c  If you wish to use the Remora PRUencoder or Remora PIDcontroller for temperature control, those components must also be installed. Further details can be found in the Development Guide.

To install the Remora component:

1. Open a terminal window on the rPi, download the Remora repository and change to the Remora component directory

.. code-block::

    pi@raspberry:~ $ mkdir ~/linuxcnc
    pi@raspberry:~ $ cd ~/linuxcnc
    pi@raspberry:~ $ git clone https://github.com/scottalford75/Remora
    pi@raspberry:~ $ cd Remora/LinuxCNC/Components/Remora
    	
2. You should see the source files into the directory with the 'ls' command

.. code-block::

    bcm2835.c
    bcm2835.h
    remora.c
    remora_lpc.c
    remora.h

3. Install the component using halcompile, if you are using an LPC17xx based board, install remora_lpc.c

.. code-block::

    pi@raspberry:~ $ sudo halcompile --install remora.c
