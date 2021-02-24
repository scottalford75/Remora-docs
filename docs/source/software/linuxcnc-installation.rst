LinuxCNC installation
=====================

Installing on Raspberry Pi OS:

1. Use Raspberry Pi Imager to install Raspberry Pi OS
2. Boot the Pi and open a terminal
3. Add the LinuxCNC Archive Signing Key to the apt keyring

.. code-block::

    sudo apt-key adv --keyserver hkp://keys.gnupg.net --recv-key 3cb9fd148f374fef

4. Add the apt repository

.. code-block::

    echo deb http://linuxcnc.org/ buster base 2.8-rtpreempt | sudo tee -a /etc/apt/sources.list.d/linuxcnc.list
	
5. Update the package list from linuxcnc.org

.. code-block::

    sudo apt-get update
	
7. Apply the work around for realtime kernel issue with memory greater than 3GB. Add the following line to /boot/config.txt

.. code-block::

    # limit total memory
    total_mem=3072

8. To ensure stable SPI frequency, force the GPU frequency to be constant. Add the following line to /boot/config.txt

.. code-block::

    # for stable SPI, force the GPU requency
    gpu_freq=200

9. Install the realtime kernel

.. code-block::

    sudo apt-get install linux-image-4.19.71-rt24-v7l+
	
10. Install LinuxCNC development brach. Dev is needed to make halcompile available for the installation of components not included in the LinuxCNC distribution, ie Remora.

.. code-block::

    sudo apt-get install linuxcnc-uspace-dev
	
.. note::

    It is also possible to monitor the controller board via the optional serial interface. To enable this on the Raspberry Pi add the following to /boot/config.txt 
	
    enable_uart=1
	
    Install a serial terminal application such as CuteCom or MiniCom.
	
    port = /dev/ttyS0
    baud rate = 115200

    Wiring is between RPi pins 8 (TXD) and 10 (RXD) to an available uart pins on the controller board.