Installing the Remora component
===============================

The Remora component is the SPI interface between LinuxCNC and the LPC17xx controller board as well as being the LinuxCNC side of the step pulse generators. Further details can be found in the Development Guide.

To install the Remora component:

1. Create a directory for the component source code

.. code-block::

    pi@raspberry:~ $ mkdir linuxcnc
    pi@raspberry:~ $ cd linuxcnc
    pi@raspberry:~ $ mkdir components
    pi@raspberry:~ $ cd components
    pi@raspberry:~ $ mkdir remora
    pi@raspberry:~ $ cd remora
	
2. Copy the source files into the directory

.. code-block::

    bcm2835.c
    bcm2835.h
    remora.c
    remora.h

3. Install the component using halcompile

.. code-block::

    pi@raspberry:~ $ sudo halcompile --install remora.c
