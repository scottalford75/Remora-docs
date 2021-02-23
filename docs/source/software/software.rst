Software
========

In this section of the Remora documentation, the installation, setup and configuration of LinuxCNC and the Remora component are covered.


LinuxCNC basics
---------------

LinuxCNC is well documented, but there are some key concepts of LinuxCNC that are important to know so that we can understand why the Remora component was developed and how it off loads tasks that a traditional parallel port stepper based LinuxCNC setup would do.


LinuxCNC task threads
~~~~~~~~~~~~~~~~~~~~~

In LinuxCNC, a thread is a list of functions that runs at specific intervals as part of a realtime task. When a thread is first created, it has a specific time interval (period), but no functions. Functions are added to the thread, and these will be executed in order every time the thread runs. In a traditional stepper configuration, two threads a created:

* Base Thread - a short period (high frequency) thread that is used for software step generation.
* Servo Thread - a longer period (lower frequency) thread tha is used for calculating motor positions, checking IO and logic functions

The Remora component allows the Base Thread tasks to be off loaded to the controller board


Hardware Abstraction Layer (HAL)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Some details to come here...





LinuxCNC installation
---------------------

Installing on Raspberry Pi OS:

1. Use Raspberry Pi Imager to install Raspberry Pi OS
2. Boot the Pi and open a terminal
3. Add the LinuxCNC Archive Signing Key to the apt keyring

.. code-block::

    sudo apt-key adv --keyserver hkp://keys.gnupg.net --recv-key 3cb9fd148f374fef

4. Add the apt repository

.. code-block::

    echo deb http://linuxcnc.org/ buster base 2.8-rtpreempt | sudo tee -a /etc/apt/sources.list.d/linuxcnc.list
	
5. Update the package list from linuxcnc.org/

.. code-block::

    sudo apt-get update
	
7. Apply the work around for realtime kernel issue with memory greater than 3GB. Add the following line to /boot/config.txt

.. code-block::

    # limit total memory
    total_mem=3072

8. Install the realtime kernel

.. code-block::

    sudo apt-get install linux-image-4.19.71-rt24-v7l+
	
9. Install LinuxCNC development brach. Dev is needed to make halcompile available for the installation of components not included in the LinuxCNC distribution, ie Remora.

.. code-block::

    sudo apt-get install linuxcnc-uspace-dev





Remora installation
~~~~~~~~~~~~~~~~~~~



LinuxCNC configuration
----------------------




E-Stop Loop
~~~~~~~~~~~

To alert and stop LinuxCNC if SPI communication with the controller board cannot be estabilished, or if it is lost during operation, the Remora component is included as part of the LinuxCNC E-Stop loop. The HAL configuration is shown below.

.. code-block::

    # estop loopback, SPI comms enable and feedback

    net user-enable-out     <= iocontrol.0.user-enable-out      => remora.SPI-enable
    net user-request-enable <= iocontrol.0.user-request-enable  => remora.SPI-reset
    net remora-status       <= remora.SPI-status                => iocontrol.0.emc-enable-in


Diagramatically this is shown in the following figure.

.. image:: images/hal-estop.png


.. note::

    TODO - include the ability to have a physical E-Stop wired to the controller board. 
	* E-Stop to be a normally closed switch
	* Json config to turn feature on
	* new module needed to monitor input pin and halt SPI on E-Stop activation (pin going low)